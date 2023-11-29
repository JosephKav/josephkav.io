---
# Leave the homepage title empty to use the site title
title: ''
date: 2022-10-24
type: landing

# Landing page sections
sections:
  - block: about.biography
    id: about
    content:
      title: Biography
      username: admin
  - block: skills
    content:
      title: Skills
      text: ''
      username: admin
    design:
      columns: '2'
  - block: experience
    content:
      title: Experience
      # Date format for experience
      #   Refer to https://docs.hugoblox.com/customization/#date-format
      date_format: Jan 2006
      # Experiences.
      #   Add/remove as many `experience` items below as you like.
      #   Required fields are `title`, `company`, and `date_start`.
      #   Leave `date_end` empty if it's your current employer.
      #   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
      items:
        - title: Software Engineer
          company: Jamf
          company_url: 'https://www.jamf.com'
          company_logo: org-Jamf
          location: London
          date_start: '2021-09-20'
          date_end: ''
          description: |2-
            Coding tools to make tasks easier for others, with a bit of managing what my team deploys, how we deploy it, updating it and making sure that it’s working as expected. e.g. developed a tool in Python to generate reports with data from various DB's for our Customers and setup scaling of these Python jobs with Celery workers.
        - title: Operations Engineer
          company: Jamf
          company_url: 'https://www.jamf.com'
          company_logo: org-Jamf
          location: London
          date_start: '2021-05-02'
          date_end: '2021-09-17'
          description: |2-
            Learning, managing and fixing/improving the tools/servers in use. Involved bits of Kubernetes, Terraform, Puppet and much more...
        - title: Managed Systems Engineer
          company: Orb Data
          company_url: 'https://www.orb-data.com'
          company_logo: org-OrbData
          location: London
          date_start: '2020-08-05'
          date_end: '2021-05-01'
          description: |2-
            Tools used:
            * Ansible
            * Terraform
            * Docker
        - title: Summer Intern
          company: Flow
          company_url: ''
          location: London
          date_start: '2019-07-01'
          date_end: '2019-09-01'
          description: |2-
            Setting up the business’ software using AWS services. They are a new company, aiming to provide sensors to companies along with a free app that anyone could download so that they can see whether a place that is subscribed to Flow is busy, in real time.

            I was involved in:
            * Setting up the database to store all of the data
            * Creating the backend API that the app talks to when it wants to retrieve data
            * Setting up authentication on this so that each user can only see and edit what they need to.

            The business is still very early in development, so there isn’t much publicly available about this company.
        - title: Summer Intern
          company: IBM
          company_url: 'https://www.ibm.com'
          company_logo: org-IBM
          location: Hursley, Winchester
          date_start: '2018-07-01'
          date_end: '2018-09-01'
          description: |2-
            Cyber Fundamentals program designed to develop strong cyber security related skills and experience. Included working with a company in the energy industry to develop a phishing solution.

            Topics covered included:
            * Internet and Hardware
            * Networking
            * Cyber Crime Industry
            * Cloud architecture, management, and security
            * Linux administration
            * Defensive and Offensive Cyber
            * JS/Python/C++/Bash
  - block: accomplishments
    content:
      # Note: `&shy;` is used to add a 'soft' hyphen in a long heading.
      title: 'Cert&shy;ifications'
      subtitle:
      # Date format: https://docs.hugoblox.com/customization/#date-format
      date_format: Jan 2006
      # Accomplishments.
      #   Add/remove as many `item` blocks below as you like.
      #   `title`, `organization`, and `date_start` are the required parameters.
      #   Leave other parameters empty if not required.
      #   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
      items:
        - title: Red Hat Certified Specialist in Ansible Automation
          certificate_url: https://rhtapps.redhat.com/certifications/badge/verify/DWT2UIEGA7BJPNDYKTN5IJFQWQAEQU3CUPSQX2KSDXT6RW46LQ34UFHA6EGV4MX6OEQWWNEDUIWXWPUWTPNOZCAXTQD32BJ2PLFPHS3STVWDCMJUD3KGSZYJTPS2YGTCOKOWYMJRGQPNI2LHBGN6LLA2MI======
          icon: cert-RedHat_EX407
          organization: 'Red Hat'
          date_start: 2020-12-07
          date_end: 2023-12-07
          description: ''
          organization: Red Hat
          organization_url: https://www.redhat.com
          url: ''
        - title: Network+
          certificate_url: https://www.credly.com/badges/45bc620f-f4f4-4115-9eec-82a6ae49bee7/public_url
          icon: cert-CompTIA_Network+
          organization: CompTIA-Network+
          date_start: 2021-02-26
          date_end: 2024-02-26
          decription: ''
          organization: CompTIA
          organization_url: 'https://www.comptia.org'
          url: 'https://www.comptia.org/certifications/network'
    design:
      columns: '2'
  - block: collection
    id: posts
    content:
      title: Recent Posts
      subtitle: ''
      text: ''
      # Choose how many pages you would like to display (0 = all pages)
      count: 5
      # Filter on criteria
      filters:
        folders:
          - blog
        author: ""
        category: ""
        tag: ""
        exclude_featured: false
        exclude_future: false
        exclude_past: false
        publication_type: ""
      # Choose how many pages you would like to offset by
      offset: 0
      # Page order: descending (desc) or ascending (asc) date.
      order: desc
    design:
      # Choose a layout view
      view: compact
      columns: '2'
  - block: portfolio
    id: projects
    content:
      title: Projects
      filters:
        folders:
          - project
      # Default filter index (e.g. 0 corresponds to the first `filter_button` instance below).
      default_button_index: 0
      # Filter toolbar (optional).
      # Add or remove as many filters (`filter_button` instances) as you like.
      # To show all items, set `tag` to "*".
      # To filter by a specific tag, set `tag` to an existing tag name.
      # To remove the toolbar, delete the entire `filter_button` block.
      # buttons:
      #   - name: All
      #     tag: '*'
      #   - name: Deep Learning
      #     tag: Deep Learning
      #   - name: Other
      #     tag: Demo
    design:
      # Choose how many columns the section has. Valid values: '1' or '2'.
      columns: '2'
      view: compact
      # For Showcase view, flip alternate rows?
      flip_alt_rows: false
  - block: contact
    id: contact
    content:
      title: Contact
      subtitle:
      # Email form provider
      form:
        provider: netlify
        formspree:
          id: xbjpqrvy
        netlify:
          # Enable CAPTCHA challenge to reduce spam?
          captcha: true
    design:
      columns: '2'
---