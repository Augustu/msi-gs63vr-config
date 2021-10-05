### Application

#### input

```bash
yay -S ibus-rime

# use setting to add ibus input source
```

#### office

```bash
yay -S libreoffice-fresh libreoffice-fresh-zh-cn libreoffice-extension-texmaths
```

#### wechat

```bash
docker run -d --name wechat --device /dev/snd --ipc="host" \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v $HOME/WeChatFiles:/WeChatFiles \
    -e DISPLAY=unix$DISPLAY \
    -e XMODIFIERS=@im=ibus \
    -e QT_IM_MODULE=ibus \
    -e GTK_IM_MODULE=ibus \
    -e AUDIO_GID=`getent group audio | cut -d: -f3` \
    -e GID=`id -g` \
    -e UID=`id -u` \
    bestwu/wechat

# change scale
docker exec -it wechat /bin/bash
cd /home/wechat/.deepinwine/Deepin-WeChat
vi system.reg
"LogPixels"=dword:00000100

# double check if save successful

docker restart wechat
```

#### pdf view

```bash
evince
```

#### computer overview

```bash
neofetch
```

#### start audio microphone speaker loopback with

```bash
# start audio microphone speaker loopback with:
pacmd load-module module-loopback latency_msec=1

# add a second:
pacmd load-module module-loopback latency_msec=3000

# add a third:
pacmd load-module module-loopback latency_msec=5000

# stop the fun afterwards:
pacmd unload-module module-loopback

#if the software is missing: apt install pavucontrol (for Debian Linuxes)
```

ref: https://superuser.com/questions/460739/how-to-listen-to-microphone-output-on-linux

#### netease 4k

```bash
sudo vi /usr/share/applications/netease-cloud-music.desktop

--force-device-scale-factor=2.0

```

