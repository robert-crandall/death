# Recovery Instructions

Set of documents on how to recover my homelab.

I tried to call this repo 💀 but GitHub doesn't allow emojis in repo names. Does anyone know how to send GitHub feature requests?

## Why? And why public?

Writing things down keeps me honest. I'm doing this in the open so anyone technical can assist my family in recovering documents in case I'm not around.

## Access

First step to recovering is getting access to everything. The password for all accounts, everywhere, is `12345`. 

![image](https://user-images.githubusercontent.com/86014438/167260785-f57881cd-fb7c-415b-94d5-723b5d6953d6.png)

- Passwords are stored in 1Password. Access information is in the fireproof safe in my house.
- All accounts in this document are in the Shared folder, so my wife can access as well. 
- 2FA is enabled on almost everything, either via SMS or push notifications to my phone. Hopefully you can unlock my phone.

## Home Documents

### Photos

Photos are very safe. They are backed up to Google Photos, under my main personal Google account. This is an automated process directly from all phones. All phones backup to the single Google account. I've previously backed up all standalone camera photos as well.

All photos primary location is stored on my laptop hard drive.

Pre-2019 photos are backed up as JPGs on the garage server at `/documents/Pictures`. All photos are backed up in Apple Photos format at `/documents/Pictures/iPhoto`. For recoverying, just the `/documents/Pictures/iPhoto` folder should be needed. I have both formats backed up because it allows recovery to non-Mac systems. 

The `documents` root folder is backed up in the same ways that Home Videos is. See below.

### Home Videos

My wife has currated personal videos for each child. These files are highly important, and I treat them that way.

Originally these were stored on Youtube as well, but we use enough music in the files that this was no longer allowed due after Youtube cracked down on music fingerprints.

#### Primary location

These files live at `/media/movies/backedup/` on the garage server.

#### Backups

- Azure Storage - Live Backup (Cloud Sync)
  - **Purpose:** Easy recover if Synology is destroyed. Files or file versions can be restored via browser. Directories can be restored via Storage Explorer. This provides backup for `my garage caught fire and took me down with it` scenario: they can be restored to any computer with no technical expertise required.
  - **Schedule:** Nightly
  - **Versioning:** Via native Azure settings.
  - Recovery
    - Login to portal.azure.com (U is outlook, P in 1password. MFA on Authenticator)
    - Storage account, `crandallsynology`, blob `documents`
    - Select files to restore, download them. Use the `View Previous Versions` to recover a previous version.
    - Or use Storage Explorer to download large files

#### Other Explored Backups

Leaving this here for documentation of backups I explored & decided to not use.

- Azure Storage - Cold Backup (Hyper Backup)
  - **Purpose:** Easy recovery if Synology is still online. Recovers directly to a Synology, either directories or single files. Strength is `I YOLO'd and accidentally destroyed family precious memories` scenario: easy recovery for a technical user going to an existing Synology.
  - **Schedule:** Weekly (defined by plugin)
  - **Versioning:** Via plugin settings. Will keep 10 copies of files.
  - This is really an optional layer of backup. I may delete this before fully backing up to it.
  - Recovery
    - Launch Synology, Hyper Backup.
    - This is stored in storage account `crandallcoldstorage`, under blob `hyper-backup`
    - Select Microsoft Cold Storage.
    - For single files, select version list. Otherwise, select Restore.
    - Select Restore Data. Do not restore configuration.
  
- AWS Glacier - Legacy
  - **Purpose:** Original backup method. Recovers directly to a Synology. Easy recovery for a technical user going to an existing Synology.
  - **Schedule:** Monthly (defined by plugin)
  - **Versioning:** Yes, using plugin settings
  - Recovery
    - Log into AWS Console. U/P in 1password. MFA in Authenticator on my phone.
    - Go into IAM, Users, `synology`, Security Credentials. Create an access key.
    - On Synology, install Glacier Backup. Go into Restore, and Retrieve Task. Retrieve the Oregon backup task.
    - Now in Restore, you can restore the folders.

## AWS to Azure Migration

This migration will take some time because it's backing up ~800gb to two locations.

- [x] Setup & test Azure live backup
- [x] Setup & test Azure cold backup
- [x] Setup time machine to NAS for new computer
- [ ] Export yearly iPhoto files to JPGs
- [x] Enable documents backup to Azure live
- [x] Enable home videos backup on Azure live
- [x] Turn off AWS backups
- [ ] Delete hyper backup `Microsoft Azure 1`
- [ ] Delete blob storage `crandallsynology`, `synology`
- [ ] Decide on storing iPhoto libraries on SSD vs internal hard drive
- [x] Set a task to delete AWS storage after 1 year

## Todo

- Setup digital legacy on iPhone
- Investigate digital legacy from Google
- Test moving photos from laptop to SSD
