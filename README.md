# genshin-auto-daily-check-in-docker

Genshin Hoyo Lab automatic attendance check Docker image. You can register multiple accounts at once.

[Github](https://github.com/rezytijo/genshin-auto-daily-check-in-docker)

## How to use

### 1. Get cookie information

1. Access [hoyolab.com](https://www.hoyolab.com/).

2. Log in.

3. Press `F12` to open developer tools.

4. Go to the `Application` tab, enter `Cookies` and then `https://www.hoyolab.com` in that order.

5. Copy `ltuid` and `ltoken` from the corresponding tab. If `ltuid_v2` and `ltoken_v2` exist instead, these are copied instead.

### 2. Using Docker image

The token is an example.

```bash
docker run -d \
     --restart always \
     -e ACCOUNT1=13435465,AbCdEFGhIjKLmnoPQRsTUvWxYZ\
     -e ACCOUNT2=32132132,PQRsTUvWxYZAbCdEFGhIjKLmno\
     -e NO_HONKAI=TRUE \
     ks2515/genshin-auto-daily-check-in
```
### 3. Using Docker Compose File
```Compose
version: '3.3'
services:
    genshinhelper:
        container_name: 'genshin-ks2515'
        environment:
            - 'TZ=Asia/Jakarta'
            - 'ACCOUNT1=<ltuid_v2>,<ltoken_v2>'
            - 'ACCOUNT2=<ltuid_v2>,<ltoken_v2>'
            - 'NO_START=0'
            - 'TIME=23:01'
            - 'SERVER=en-us'
        #volumes:
            #- '/etc/genshin:/app/genshincheckinhelper/config'
        restart: always
        image: 'ks2515/genshin-auto-daily-check-in'
```
All environment variables starting with `ACCOUNT` are recognized.

You must enter ltuid and ltoken separately with `,`.

![Example image](https://i.imgur.com/s8C8cJy.png)

The result appears as above.

| Available environment variables | Description | Example |
| ----------- | -------------------------------------------------- | ----------------------------------- |
| ACCOUNT* | Cookie information. | 13435465,AbCdEFGhIjKLmnoPQRsTUvWxYZ |
| SERVER | Language information to use. Default "en-us" | en-us,zh-cn,zh-tw,de-de,es-es,fr-fr,id-id,ja-jp,ko-kr,pt-pt,ru-ru,th-th,vi-vn|
| TIME | It's time to check attendance every day. Based on CST (UTC+8). Default value "00:00"<br/>The standard attendance check time is 1:00 AM Korean time. (00:00 China time) | 00:00 |
| TZ | The time zone the Docker container will use. <br/>The default value is Asia/Shanghai according to the standard attendance check time. | Asia/Shanghai |
| NO_GENSHIN | Genshin Impact does not check attendance. | true |
| NO_STARRAIL | Starrail does not check attendance. | 1 |
| NO_HONKAI | Collapse 3rd Attendance check is not conducted. | yes |

### 4. Others

```bash
python main.py -o
```

If you run main.py with -o, it will run only once instead of repeating every day.

#### build

```bash
docker buildx create --name genshin-builder --use

docker buildx build --platform linux/amd64,linux/arm64 --tag ks2515/genshin-auto-daily-check-in --push .
```

## Requirements

python>=3.11<br>
[schedule](https://github.com/dbader/schedule)<br>
[genshin](https://github.com/thesadru/genshin.py)<br>
[rich](https://github.com/Textualize/rich)