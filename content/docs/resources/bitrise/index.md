+++
title = "Bitrise Resources"
description = "A bit of info for Bitrise and how to set it up."
date = 2023-01-18T20:33:00+00:00
updated = 2023-01-18T20:33:00+00:00
draft = false
weight = 420
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
authors = ["jonholtan"]
+++

[Bitrise](https://bitrise.io) is a fully-hosted Mobile DevOps and CI/CD Platform. Automate your builds, tests, and releases. Deliver faster, with confidence. 

We are working on a standardized yaml configuration to share with the company. For now if you are interested in setting up Bitrise for your project, consult with Jon.

## Bitrise Codesign Export Setup

Pre-requisties:
- Must have an Apple Development Cert.
	- This is Xcode managed.

- Must have an Apple Distribution Cert.
	- This is created at most twice per project. Get the key from 1Password or from your tech lead.
	- This is a Private Key.

- Must go through an Archive flow in Xcode for each flavor. Do NOT actually submit to TestFlight. On the final page where it says `Upload`, select cancel.
- Supply mac password for each prompt and select always allow. This gives Xcode access to to Keychain to check your certs. 

After the manual archive flow is complete for each flavor. Move on to the profile/cert collection via the bash script.

Bash Script: 
```bash 
bash -l -c "$(curl -sfL https://raw.githubusercontent.com/bitrise-tools/codesigndoc/master/_scripts/install_wrap.sh)"
```

- Run the bash script in the iOS directory
- Select the scheme (dev, stage, prod)
- Select app-store
- Specify you wish to upload to bitrise
- Enter your personal acces token (created in the Bitrise Console)
- Select the app to upload too.
- Repeat for the next flavors


