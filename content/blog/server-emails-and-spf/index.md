---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Server Emails and SPF"
subtitle: ""
summary: ""
authors:
  - admin
tags: []
categories: []
date: 2021-01-04T13:55:12Z
lastmod: 2021-01-04T13:55:12Z
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

I've wanted my server to be able to notify me about any issues it encounters, such as high temps or failed drives pretty much as soon as I started tinkering with servers sometime in 2013 (I've since setup MatterMost for apps that support slack-style notifications, but not all do). For a few years I took the easy way out and just created a separate gmail account for my single server and set that server up with those gmail credentials, but, I'm not a fan of that solution since it:

* a. Requires the server to have full control over that account.
* b. Requires emails to come from an available @gmail.com address.
* c. Would require a short 'app password' since I use 2FA on everything that supports it.
* d. Encryption of the emails

Mainly because of b (wanting emails to come from one of my own domains), I looked for alternatives a few years ago. Since I was using G-Suite at the time, I looked for ways to email myself and found that I could add aliases to my account and email from those, allowing me to overcome issue b. Until around a year ago, this was the solution I used as my server (and the many services it was running) was pretty much only accessible locally or over VPN. 

<br />
<br />

### SPF - ASPMX.L.GOOGLE.COM
I stuck with this method until I got sick of having to create new aliases for every new address I wanted to email from, and so eventually did more reaserch until I came across https://support.google.com/a/answer/176600. Under 'Use the restricted Gmail SMTP server' I found that I could send internal messages to users in my G-Suite (now Google Workspace) without any authentication. The thing restricting you from sending the emails is the SPF records for the domain you're trying to email from and whether they contain the IPv4 or IPv6 IP you're trying to email from. After setting up the spf records, I found that this removed the alias creation requirement and so if your IP was in the SPF record for example.com, if example.com was in your G-Suite account, you'd be able to email from *@example.com to any of your G-Suite addresses that would receive mail. This method would overcome all the issues other than d as emailing this way cannot use any form of encryption, but I was fine with this since only my G-Suite domains could send or receive these emails.

<br />
<br />

### Ansible - Update SPF records automatically
Since one of my servers is running at home, it uses a dynamic IP address as Virgin Media won't give a static IP unless you upgrade to business. For a few years I've overcome this by just running Dynamic DNS on pfSense (OPNsense now). This meant that when my IP changed, if I still wanted these emails to send  from my home server, I'd need to manually update the SPF records. Immediately I started looking for a way to update the SPF records I specify with the updates IP addresses, and I found that Ansible has a cloudflare_dns plugin that could be used to perform these DNS updates just by running a playbook (which I could hook up to my AWX server to run automatically at a regular interval). Since I'm an Red Hat Certified Specialist in Ansible Automation (and deploy/maintain every service my servers run through it), I created a playbook that would perform DNS lookups on various domains I specify and then create the SPF records for me with the IPv4/IPv6 addresses they resolve to. This playbook can be found here https://github.com/JosephKav/cloudflare-dns-update-google-spf.

<br />
<br />

### Dynamic IP's and Spamhaus
I implemented the above solution until a month or two ago. I stopped since emails failed to send from my home IP and upon further investigation, I saw that my IP address was listed in Spamhaus and so filled out the forms to get it removed, waited for it to be removed and tried sending emails again. This did not work. I'm not sure if this was related to the frequency that gmail checks though. Instead I looked into setting up a VPS with a static IP that has no issues in sending mail, to act as a forwarder for my domains and so I set this up in postfix. Though, I did not want the whole world to be able to send emails to me (though, I doubt anyone would be able bruteforce a valid domain name before my VPS got put in Spamhaus). To reduce the chance of this I set up iptables rules using ipset's. I have a script (below) that can run in cron to maintain the correct IP for my domain in the ipset, and so in the iptables rules.

```bash
#!/bin/bash
set=IPSETNAME
host=HOSTNAME
  
me=$(basename "$0")
  
ip=$(dig +short $host)
  
if [ -z  "$ip" ];  then
exit 1
fi
  
# make sure the set exists
/usr/sbin/ipset -exist create $set hash:ip

if ! /usr/sbin/ipset -q test $set $ip; then
    /usr/sbin/ipset flush $set
    /usr/sbin/ipset add $set $ip
fi
```

I have this ipset and then add the rule

`iptables -I INPUT -p tcp --dport PORT -m set --match-set IPSETNAME src -j ACCEPT`

along with a rule to block by default on that port

`iptables -A INPUT -p tcp --dport PORT -j DROP`

This only allows access to port PORT from the IP that HOSTNAME resolves to.

<br />
<br />

### Authentication
So now I had a VPS with a static IP accessible from the IP's/Domain's I set, but there's still the possibility of getting listed on the Spamhaus blocklist if my IP changes and someone that gets assigned that old IP starts spamming emails before the cronjob updates the ipset. So, to prevent this, I have added SMTP-AUTH as well as TLS to the postfix forwarder (LetsEncrypt to get a valid SSL cert). This way, if the IP changes, whoever has that new IP must then bruteforce a valid username and password for the forwarder before the cronjob runs. Probably overkill, but I wanted to prevent bruteforce attempts and so opted for a fail2ban jail so that if a bruteforce attempt started, the server could IP ban the brute-forcer. Since my servers would never fail that SASL auth, I opted for fail2ban to ban IP's entirely if they failed a single time. I set maxretry to 1 and make fail2ban use the following failregex in its filter of /var/log/mail.log

`failregex = ^(.*)\[<HOST>\]: SASL (?:LOGIN|PLAIN) authentication failed:(.*)$`

<br />
<br />

### Conclusion
This work has resulted in my servers being able to send emails to anyone in my G-Suite without having to provide any of them with credentials that could potentially be used to gain more information about me. And, the most significant accomplishment I'd say, is not having to maintain any SPF records or email aliases. I can just let Ansible set each server up with the correct credentials and postfix relayhost, as well as maintain the config/users on the postfix relay. The only time I need to intervene is if I want to create new user accounts, and that would just be a simple addition to the vars used in my postfix-server role.