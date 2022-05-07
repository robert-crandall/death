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

Photos are very safe. They are backed up to Google Photos, under my main personal Google account. This is an automated process directly from all phones. All phones backup to the single Google account. I've previously backed up all standalone camera photos as well.

All photos primary location is stored on my laptop hard drive.

Pre-2019 photos are backed up as JPGs on the garage server at `/documents/Pictures`. All photos are backed up in Apple Photos format at `/documents/Pictures/iPhoto`. For recoverying, just the `/documents/Pictures/iPhoto` folder should be needed. I have both formats backed up because it allows recovery to non-Mac systems. 

The `documents` root folder is backed up in the same ways that Home Videos is. See below.

### Home Videos

This is the real motivation for writing this document. My wife has currated personal videos for each child. These files are highly important, and I treat them that way.

Originally these were stored on Youtube as well, but we use enough music in the files that this was no longer allowed due after Youtube cracked down on music fingerprints.

#### Primary location

These files live at `/media/movies/backedup/` on the garage server.

#### Backups

These are backed up in multiple locations, for different purposes. Listed in order most likely needed for recovery.

- Azure Storage - Cloud Sync
  - The purpose of this backup is to have easy recovery available, without needing a Synology. It can restore single files at a time via a browser. I believe the entire backup can also be recovered, though I haven't tested this.
  - These files are backed up live. Once they are written to Synology, they are also written to Azure.
  - At this time, this is not a complete backup. I'm migrating from AWS to this. (AWS is up to date until this transition is done).
  - It also supports versioned files.
  - Recovery
    - Login to portal.azure.com (U is outlook, P in 1password. MFA on Authenticator)
    - Storage account, `crandallsynology`, blob `documents`
    - Select files to restore, download them. Use the `View Previous Versions` to recover a previous version.
- Azure Storage - Hyper Backup
  - The purpose of this backup is the insurance type backup. It can restore the entire directory to a Synology easily. It can also do single file restores, but requires the Synology for this.
  - These files are backed up weekly.
  - At this time, this is not a complete backup.
  - Recovery
    - Launch Synology, Hyper Backup.
    - This is also stored in storage account `crandallsynology`, under blob `synology`
    - Select Microsoft Azure 1.
    - For single files, select version list. Otherwise, select Restore.
    - Select Restore Data. Do not restore configuration.
- AWS Glacier
  - This backup requires a Synology to restore. It is backed up using `Glacier Backup` on Synology.
  - The purpose of this backup is version control. If I accidentally overwrite these files with junk, I need to restore that.
  - Recovery
    - Log into AWS Console. U/P in 1password. MFA in Authenticator on my phone.
    - Go into IAM, Users, `synology`, Security Credentials. Create an access key.
    - On Synology, install Glacier Backup. Go into Restore, and Retrieve Task. Retrieve the Oregon backup task.
    - Now in Restore, you can restore the folders.

## AWS to Azure Migration

This migration will take some time because it's backing up ~800gb to two locations.

- [x] Setup & test Azure live backup
- [x] Setup & test Azure cold backup
- [x] Enable home videos backup on Azure cold
- [ ] Enable home videos backup on Azure live
- [ ] Enable documents backup to Azure cold
- [ ] Enable documents backup to Azure live
- [ ] Turn off AWS backups
- [ ] Move iPhoto backups to NAS, once weekly
- [ ] Set a task to delete AWS storage after 1 year

## Todo

- Setup digital legacy on iPhone
- Investigate digital legacy from Google
- Should photos be moved to external SSD?
- Move AWS glacier backup to Azure
