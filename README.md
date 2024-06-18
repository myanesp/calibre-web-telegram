## Script for uploading books to Calibre Web directly from Telegram

This repository will give you the needed scripts to automatically upload books to Calibre-Web (Docker installation) when forwarding an ebook file to a Telegram bot. 

The scripts allow Calibre-Web to monitor a given directory in which the books we want to upload will be placed. Along with [Telethon Downloader](https://github.com/jsavargas/telethon_downloader), every time you send a book file to your bot, it will download the file in the folder of convenience, and Calibre-Web will add it to your library. 

### How it works
The Calibre-Web developer offers on its wiki a [Linux script](https://github.com/janeczku/calibre-web/wiki/Automatically-import-new-books-(Linux)) for monitoring directories and uploading its content to Calibre-Web, but this tool is missing on the Docker images.

Besides this, the [flexibility of the Docker images provided by LinuxServer](https://docs.linuxserver.io/general/container-customization/) allows us to attach the script and install the needed packages to our Calibre-web container.

Basically, with the [custom-init/install-inotify.sh](custom-init/install-inotify.sh) script we install the inotify-tools package, which is mandatory to watch a folder, and with [custom-services/calibre-watch.sh](custom-services/calibre-watch.sh) we execute the script that will watch the directory and upload the ebook files found in that folder.

### How to use
Basically, you have to clone this repository and mount into your Calibre-web container three folders: the custom-init and custom-services directories from here, and also the path of the folder where Telethon will download the ebook files. You have an example in this [docker-compose.yml](docker-compose.yml). It is also mandatory to have the [universal Calibre docker mod](https://github.com/linuxserver/docker-mods/tree/universal-calibre) set in the environment.

Before launching the container, we need to fix permissions. As stated on the LinuxServer docs, both custom folders have to be owned by root:

```bash
sudo chown -R root: custom-*
```

Lastly, we have to modify the `config.ini` file from Telethon to download ebook files in a given directory instead of the general completed one.

```ini
[DEFAULT_PATH]
...
epub = /download/ebooks
mobi = /download/ebooks
azw3 = /download/ebooks
``` 