---
layout: page
title: Commit Signing
---

Commit signing [GitHub Docs](https://github.com/microsoft/vscode/wiki/Commit-Signing) are here

> If you change your key later you will have issues committing.  The easiest way I found to fix them is to delete all the containers, images, volumes, and builds in Docker Desktop and then clone the repo in a named volume again.  I'm sure there is a way to fix this but I did not feel like investing the time to figure it out.
{: .prompt-tip }

1. Install [Gpg4win](https://gpg4win.org/) or [whatever version you need](https://gnupg.org/download/)
2. Generate a GPG key pair
   - `gpg --full-generate-key`
   - Use the following config:
     - RSA and RSA
     - 4096 bits
     - key does not expire
     - Real name: (your name)
     - Email address: 
       - If your email is private, use the private version of your email (your github no-reply email)@users.noreply.github.com
         > This email address can be found [here](https://github.com/settings/emails)
         {: .prompt-tip }
       - If your email is not private, use your public email address
     - Comment: GitHub GPG
   - Confirm settings
   - Create a passphrase
3. Get the key ID
   - `gpg --list-secret-keys --keyid-format=long`
     - The ID is the 16 characters after `sec   rsa4096/` 
4. Export your public key
   - `gpg --armor --export (ID from above)`
5. Add the output from above to your [GitHub GPG keys](https://github.com/settings/gpg/new)
6. If you want to use this key on other devices, or back it up use the following command
    >This creates a file that contains the unencrypted public and private keys, be careful with it
    {: .prompt-tip }
  - `gpg --output backupkeys.pgp --armor --export-secret-keys --export-options export-backup (email@address.com)`
7. Set git gpg program and config
   - `git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"`
   - `git config --global user.signingkey (ID from above)`
   - `git config --global commit.gpgsign true`
8. Make sure the email you configured in git matched the email in the GPG key.  If the email is not the same the commit will not show as verified.
   - Show configured email
     - `git config --global user.email`
   - Update email
     - `git config --global user.email "your@example.com"`
9. If you are using Windows with Kleopatra you can set the passphrase timeout so you don't have to enter it on every commit
   - Launch Kleopatra
   - Click Settings > Configure Kleopatra...
   - Navigate to GnuPG System > Private Keys
   - Change Expire cached PINs after N seconds to 28800 (this is 8 hours)
10. Push a commit and check to make sure it shows Verified
