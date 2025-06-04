# Proxmox Webhook Notification to Discord

Due to the character limit of discord webhooks (2000 characters) you can't send full backup logs into discord so this program will act as a middle man and store the log file and make it accessble via a link that will be posted in the discord instead of the wall of text being posted in the channel directly.

```
// Method/URL:
POST http://url-to-webserver/webhook

// Headers:
Content-Type: application/json

// Body:
{
    "discordWebhook": "https://discord.com/api/webhooks/{{ secrets.token }}",
    "messageContent": "{{ escape message }}",
    "messageTitle": "{{ title }}",
    "urlLogAccessable": "http://url-to-webserver/logs/",
    "severity": "{{ severity }}"
}

//Secrets:
token: 70043953045676504/5E8qbiWN4o-sdagsdgfgdfgdfhfghjrrety_adadsdadsadasdasds-izpAdw
```

You will need to compile it on your prefered os (You will need to install go aswell):
```
git clone https://github.com/MrPvTDagger/pve-to-discord.git
go build -o /pvetodiscord
mkdir logs/
./pvetodiscord
```
or run it in a docker container
```
git clone https://github.com/MrPvTDagger/pve-to-discord.git
docker build . -t pvetodiscord:latest
docker run -d -p 8880:80 --restart always pvetodiscord:latest
```

or via the prebuilt image:
```ghcr.io/mrpvtdagger/pve-to-discord:latest```

```
services:
 pvetodiscord:
    image: ghcr.io/mrpvtdagger/pve-to-discord:latest
    volumes:
      - "/path-to-logs/logs:/app/logs"
    ports:
      - "8087:80"
    restart: unless-stopped
```
![example-in-discord](https://cdn.oki.cx/files/r3p4gxa1y7kg4gei5wvhq2p03.png)
