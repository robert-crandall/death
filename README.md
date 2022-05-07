# Recovery Instructions

Set of documents on how to recover my homelab.

I tried to call this repo 💀 but GitHub doesn't allow emojis in repo names. Does anyone know how to send GitHub feature requests?

## Why? And why public?

Writing things down keeps me honest. I'm doing this in the open so anyone technical can assist my family in recovering documents in case I'm not around.

## Access

First step to recovering is getting access to everything. The password for all accounts, everywhere, is `12345`. 

![image](https://user-images.githubusercontent.com/86014438/167260785-f57881cd-fb7c-415b-94d5-723b5d6953d6.png)

- Passwords are stored in 1Password. My wife can access many shared passwords in this account. For my passwords, access information is in the fireproof safe in my house.
- 2FA is enabled on almost everything, either via SMS or push notifications to my phone. Hopefully you can unlock my phone.

## Home Documents

The most important thing to recover is home photos & videos. This is why I'm writing this document.

### Photos

Photos are very safe. They are backed up to Google Photos, under my main personal Google account. This is an automated process directly from the phones. I've previously backed up all camera photos as well, but do not take pictures with standalone cameras anymore.

All photos primary location is stored on my laptop hard drive.

Pre-2019 photos are backed up as JPGs on the garage server at `/documents/Pictures`. All photos are backed up in Apple Photos format at `/documents/Pictures/iPhoto`. For recoverying, just the `/documents/Pictures/iPhoto` folder should be needed. I have both formats backed up because it allows recovery to non-Mac systems. 

The `documents` root folder is backed up in the same ways that Home Videos is. See below.

### Home Videos

This is the real motivation for writing this document. My wife has currated personal videos for each child. These files are highly important, and I treat them that way.

Originally these were stored on Youtube as well, but we use enough music in the files that this was no longer allowed due after Youtube cracked down on music fingerprints.

#### Primary location

These files live at `/media/movies/backedup/` on the garage server.

#### Backups

These are backed up in multiple locations, for different purposes:

- AWS Glacier
  - This backup probably requires a Synology to restore. It is backed up using `Glacier Backup` on Synology.
  - The purpose of this backup is an extra layer of insurance. If the garage burns down, it's good to know files can eventually be restored.
  - Recovery
    - Log into AWS Console. U/P in 1password. MFA in Authenticator on my phone.
    - Go into IAM, Users, `synology`, Security Credentials. Create an access key.
    - On Synology, install Glacier Backup. Go into Restore, and Retrieve Task. Retrieve the Oregon backup task.
    - Now in Restore, you can restore the folders.



## Todo

- Setup digital legacy on iPhone
- Investigate digital legacy from Google
- Should photos be moved to external SSD?
