
## Setting up config file
<details>
    <summary><b>Click here for more details</b></summary>

```
cp config_sample.env config.env
```
- Remove the first line saying:
```
_____REMOVE_THIS_LINE_____=True
```
Fill up rest of the fields. Meaning of each fields are discussed below:
### Required Field
- **BOT_TOKEN**: The Telegram bot token that you get from [@BotFather](https://t.me/BotFather)
- **TELEGRAM_API**: This is to authenticate to your Telegram account for downloading Telegram files. You can get this from https://my.telegram.org DO NOT put this in quotes.
- **TELEGRAM_HASH**: This is to authenticate to your Telegram account for downloading Telegram files. You can get this from https://my.telegram.org
- **OWNER_ID**: The Telegram user ID (not username) of the Owner of the bot
- **DATABASE_URL**: Your Database URL. See [Generate Database](https://github.com/breakdowns/slam-mirrorbot/tree/master#generate-database) to generate database. (**NOTE**: If you deploying on Heroku using Heroku button, no need to generate database manually, because it will automatic generate database when first deploying)
- **GDRIVE_FOLDER_ID**: This is the folder ID of the Google Drive Folder to which you want to upload all the mirrors.
- **DOWNLOAD_DIR**: The path to the local folder where the downloads should be downloaded to
- **DOWNLOAD_STATUS_UPDATE_INTERVAL**: A short interval of time in seconds after which the Mirror progress message is updated. (I recommend to keep it `5` seconds at least)  
- **AUTO_DELETE_MESSAGE_DURATION**: Interval of time (in seconds), after which the bot deletes it's message (and command message) which is expected to be viewed instantly. (**Note**: Set to `-1` to never automatically delete messages)
- **UPSTREAM_REPO**: Link for Bot Upstream Repo, if you want default update, fill ```https://github.com/breakdowns/slam-mirrorbot```.
- **UPSTREAM_BRANCH**: Branch name for Bot Upstream Repo (Recommended using master branch)
### Optional Field
- **AUTHORIZED_CHATS**: Fill user_id and chat_id of you want to authorize.
- **IS_TEAM_DRIVE**: Set to `True` if `GDRIVE_FOLDER_ID` is from a Team Drive else `False` or Leave it empty.
- **USE_SERVICE_ACCOUNTS**: (Leave empty if unsure) Whether to use Service Accounts or not. For this to work see [Using service accounts](https://github.com/breakdowns/slam-mirrorbot#generate-service-accounts-what-is-service-account) section below.
- **INDEX_URL**: Refer to https://github.com/ParveenBhadooOfficial/Google-Drive-Index The URL should not have any trailing '/'
- **MEGA_API_KEY**: Mega.nz api key to mirror mega.nz links. Get it from [Mega SDK Page](https://mega.nz/sdk)
- **MEGA_EMAIL_ID**: Your email id you used to sign up on mega.nz for using premium accounts (Leave th)
- **MEGA_PASSWORD**: Your password for your mega.nz account
- **BLOCK_MEGA_FOLDER**: If you want to remove mega.nz folder support, set it to `True`.
- **BLOCK_MEGA_LINKS**: If you want to remove mega.nz mirror support, set it to `True`.
- **STOP_DUPLICATE_MIRROR**: (Leave empty if unsure) if this field is set to `True`, bot will check file in Drive, if it is present in Drive, downloading will be stopped. (**Note**: File will be checked using filename, not using filehash, so this feature is not perfect yet)
- **STOP_DUPLICATE_MEGA**: (Leave empty if unsure) if this field is set to `True`, bot will check file in Drive, if it is present in Drive, downloading Mega will be stopped.
- **STOP_DUPLICATE_CLONE**: (Leave empty if unsure) if this field is set to `True`, bot will check file in Drive, if it is present in Drive, cloning will be stopped.
- **CLONE_LIMIT**: To limit cloning Google Drive (leave space between number and unit, Available units is (gb or GB, tb or TB).
- **MEGA_LIMIT**: To limit downloading Mega (leave space between number and unit, Available units is (gb or GB, tb or TB).
- **TORRENT_DIRECT_LIMIT**: To limit the Torrent/Direct mirror size, Leave space between number and unit. Available units is (gb or GB, tb or TB).
- **IMAGE_URL**: Show Image/Logo in /start message. Fill value of image your link image, use telegra.ph or any direct link image.
- **VIEW_LINK**: View Link button to open file Index Link in browser instead of direct download link, you can figure out if it's compatible with your Index code or not, open any video from you Index and check if the END of link from browser link bar is `?a=view`, if yes make it `True` it will work (Compatible with [Bhadoo Index](https://github.com/ParveenBhadooOfficial/Google-Drive-Index) Code)
- **UPTOBOX_TOKEN**: Uptobox token to mirror uptobox links. Get it from [Uptobox Premium Account](https://uptobox.com/my_account).
- **HEROKU_API_KEY**: (Only if you deploying on Heroku) Your Heroku API key, get it from https://dashboard.heroku.com/account.
- **HEROKU_APP_NAME**: (Only if you deploying on Heroku) Your Heroku app name.
- **IGNORE_PENDING_REQUESTS**: (Optional field) If you want the bot to ignore pending requests after it restarts, set this to `True`.
- **SHORTENER_API**: Fill your Shortener api key if you are using Shortener.
- **SHORTENER**: if you want to use Shortener in Gdrive and index link, fill Shortener url here. Examples:
```
exe.io, gplinks.in, shrinkme.io, urlshortx.com, shortzon.com
```

Above are the supported url Shorteners. Except these only some url Shorteners are supported.

**Note**: You can limit maximum concurrent downloads by changing the value of **MAX_CONCURRENT_DOWNLOADS** in aria.sh. By default, it's set to `7`.
### Add more buttons (Optional Field)
Three buttons are already added of Drive Link, Index Link, and View Link, you can add extra buttons, these are optional, if you don't know what are below entries, simply leave them, don't fill anything in them.
- **BUTTON_FOUR_NAME**:
- **BUTTON_FOUR_URL**:
- **BUTTON_FIVE_NAME**:
- **BUTTON_FIVE_URL**:
- **BUTTON_SIX_NAME**:
- **BUTTON_SIX_URL**:

</details>

## Using service accounts for uploading to avoid user rate limit
For Service Account to work, you must set **USE_SERVICE_ACCOUNTS="True"** in config file or environment variables
Many thanks to [AutoRClone](https://github.com/xyou365/AutoRclone) for the scripts
**NOTE**: Using service accounts is only recommended while uploading to a team drive.

## Generate service accounts. [What is service account](https://cloud.google.com/iam/docs/service-accounts)


first enable IAM API from cloud console by Visiting [Google API page](https://console.developers.google.com/apis/library)

Let us create only the service accounts that we need. 
**Warning**: abuse of this feature is not the aim of this project and we do **NOT** recommend that you make a lot of projects, just one project and 100 sa allow you plenty of use, its also possible that over abuse might get your projects banned by google. 

```
Note: 1 service account can copy around 750gb a day, 1 project can make 100 service accounts so that's 75tb a day, for most users this should easily suffice. 
```

`python3 gen_sa_accounts.py --quick-setup 1 --new-only`

A folder named accounts will be created which will contain keys for the service accounts

**NOTE**: If you have created SAs in past from this script, you can also just re download the keys by running:
```
python3 gen_sa_accounts.py --download-keys project_id
```

## Add all the service accounts to the Team Drive
- Run:
```
python3 add_to_team_drive.py -d SharedTeamDriveSrcID
```

## Youtube-dl authentication using .netrc file
For using your premium accounts in youtube-dl, edit the [.netrc](https://github.com/breakdowns/slam-mirrorbot/blob/master/.netrc) file according to following format:
```
machine host login username password my_youtube_password
```
where host is the name of extractor (eg. youtube, twitch). Multiple accounts of different hosts can be added each separated by a new line

## Credits

Thanks to:
- [out386](https://github.com/out386) heavily inspired from telegram bot which is written in JS
- [Izzy12](https://github.com/lzzy12/) for original repo
- [Dank-del](https://github.com/Dank-del/) for base repo
- [magneto261290](https://github.com/magneto261290/) for some features
- [SVR666](https://github.com/SVR666/) for some fixes
- [4amparaboy](https://github.com/4amparaboy/) for some help
- [WinTenDev](https://github.com/WinTenDev/) for Uptobox support
- [iamLiquidX](https://github.com/iamLiquidX/) for Speedtest module
- [ydner](https://github.com/ydner/) for Usage module
- [breakdowns](https://github.com/breakdowns) for main repo
- [ayush](https://github.com/ayushteke) for nothing

