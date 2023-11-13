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

Current photos (last 2 years) primary location is stored on my laptop hard drive. They are backed up with Time Machine.

Once a year, I use PowerPhotos to split up older photos to yearly iPhoto libraries. These are stored on an external SSD. These are also backed up as zip files to `/[server]/documents/Pictures/iPhoto`, which is backed up to the cloud using the active method below. I've previously also backed up JPGs of the iPhoto library to `/[server]/documents/Pictures/[YEAR]`. These files are redundant, and do not need to be recovered.

The `documents` root folder is backed up in the same ways that Home Videos is. See below.

### Home Videos

My wife has curated personal videos for each child. These files are highly important, and I treat them that way.

#### Primary location

These files live at `/[server]/media/movies/backedup/` on the garage server, and backed up using the active backup method mentioned below.

#### Backups

**Active Method**

- Duplicacy
  - **Purpose:** Easy recover if Unraid is destroyed. Files or file versions can be restored via browser. Directories can be restored via browser. This backup is ideal for `I YOLO'd and destroyed precious memories`, and also provides backup for `my garage caught fire and took me down with it` scenario: they can be restored to any computer. Some technical expertise required.
  - **Schedule:** Nightly
  - **Versioning:** Native.
  - Recovery
    - Login to `backups.[mydomain]`
    - Select `Restore`
    - In case of `YOLO` select local Storage. In case of `garage fire` select `Azure` storage.
    - Select Backup ID and Revision
    - Select a Restore location (suggestion: `/mnt/user/backups/restore`)
    - Select directories to restore
  - Setup Duplicacy
    - If Duplicacy is offline, you can try to access it at `[server_ip]:3875`
    - If the server is offline, you can download a new copy from `https://duplicacy.com/`
      - Azure storage information (Storage account and key) are in 1password. Item is Duplicacy.


#### Other Explored Backups

Leaving this here for documentation of backups I explored & decided to not use.

- Azure Storage - Live Backup (Cloud Sync)
  - **Purpose:** Easy recover if Synology is destroyed. Files or file versions can be restored via browser. Directories can be restored via Storage Explorer. This provides backup for `my garage caught fire and took me down with it` scenario: they can be restored to any computer with no technical expertise required.
  - **Schedule:** Nightly
  - **Versioning:** Via native Azure settings.
  - Recovery
    - Login to portal.azure.com (U is outlook, P in 1password. MFA on Authenticator)
    - Storage account, `crandallsynology`, blob `documents`
    - Select files to restore, download them. Use the `View Previous Versions` to recover a previous version.
    - Or use Storage Explorer to download large files

- Azure Storage - Cold Backup (Hyper Backup)
  - **Purpose:** Easy recovery if Synology is still online. Recovers directly to a Synology, either directories or single files. Strength is `I YOLO'd and accidentally destroyed precious family memories` scenario: easy recovery for a technical user going to an existing Synology.
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
