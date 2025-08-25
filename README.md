# Password_Cracking_Ansible
**Bottom Line Up Front (BLUF):** Ansible playbooks for configuring and updating an **Ubuntu 24.04 LTS Server** with a **NVidia graphics card** for password cracking and password security research.

## Background and Overview
I often find myself needing to provision and update my password cracking servers. This could be because I'm deploying a new system, making use of cloud resources, or simply that I totally trashed my previous setup trying to install a random machine learning library. In the past this would usually take me a day to get my core tools installed and working. I'd also always seem to forget some tool or resource which would lead to more wasted time, usually when I was in the middle of another task, I'd rather be working on!

That's the problem these Ansible playbooks are meant to solve. By automating the installation process, I can kick these scripts off, walk away, and come back to a system that is already set up and ready to crack hashes. As an added bonus, I can also periodically re-run these playbooks to update my system. Considering John the Ripper tends to recieve updates monthly (if not weekly), being able to automate the build process has been incredibly helpful for me to quickly gain access to new features and fixes without having to manually put in the work to keep on top of my patching.

## Quick Setup and Running Instructions
1. Install Ubuntu 24.04 LTS to the target system
    - Create an account that is remotely accessible via SSH and has sudo privileges. This will be the main password cracking user and the tools/wordlists will be provisioned for this user.
    - **(Optional)**: Generate (or copy over an existing) SSH certificate for the user so you don't need to type a password when logging in or running the Ansible playbooks.

2. **(Optional)** Set up a ZeroTier VPN network
    - **Reason**: This is a great way to enable remote connectivity to servers behind NAT that do not have public IP addresses. ZeroTier is free as long as your network size doesn't exceed 25 devices so if you are only managing a few servers is can be a great option. As an added bonus, I've used it in past years on the Defcon Wi-Fi network so it's an effective way to manage remote systems when participating in the Crack Me If You Can competition.
    - ZeroTier Account Setup Info: https://docs.zerotier.com/start/

3. Create/Configure a **hosts.ini** file
    - You can base this on the hosts.example file included in this repository
    - The hosts file takes the form: `HOSTNAME zerotier_network=ZT_NETWORK_ID`
        - If you are not using ZeroTier, you do not need to include the network variable

4. Update **./group_vars/all** to match your desired deployment and config variables
    - This defines things like where to install your tools and where to save your cracked passwords by default
    - You can also define if you want to download/update wordlists from hashmob.com and other sites
    - Another big option is you can configure if you want to install a custom desktop GUI or not

5. Run the ansible scripts to actually perform the updates/installs. You can do this using the following command
    - `ansible-playbook site.yml -i hosts.ini --ask-become-pass`
    - Note: You can use `--extra-vars "ansible_become_pass=SUDO_PASSWORD"` instead of `--ask-become-pass` if you don't mind having your sudo password in your history and/or want to put running this playbook into a script.

6. If you encounter errors, please submit an issue to this github repo so I can look into it. Currently there is a lot of "this works on my machine", so understanding where this fails for others can help me make this repo more resilient and useful.

## List of Installed Applications and Tasks

#### Password Cracking Tools
- Hashcat
- John the Ripper
- MDXFind

#### Software Updates
- Operating System
- Packages
- NVidia Drivers

#### Analysis Tools
- PACK
- PACK2
- Rulefinder
- Jupyter Labs Password Analysis Workbooks

#### Password Guess Generators and Mangling Tools
- PCFG Tools
- RLing
- hashcat_utils

#### Password Dumping Tools
- hcxtools

#### Misc Research Tools
- Reusablesec Password Research Tools

#### Wordlists
- Hashmob found lists
- dic0294
- RockYou
- zxcvbn

#### Desktop Environment
**Warning:** This is more of something I've been playing around with vs. a real work environment. You will probably be better off installing a GUI with Ubuntu vs. using the environment in this deployment.
- Xorg Display Server
- I3 Windows Server
- LightDM Desktop Manager 