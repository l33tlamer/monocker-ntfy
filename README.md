# Monocker
Monitors Docker (MONitors dOCKER) containers and alerts on 'state' change

![Telegram Alerts](https://raw.githubusercontent.com/petersem/monocker/master/doco/telegram.PNG)

## Features
- Telegram integration
- Pushbullet integration
- Pushover integration
- Discord integration (via webhooks)
- [ntfy.sh](https://ntfy.sh/) integration
- Monitors 'state' changes for all containers (every 10 seconds)
- Specific inclusions or exclusions of containers to monitor
- Optionally, only alert on state changes to (paused, exited, running (unhealthy), or dead)
- In-built Docker healthcheck

## Future Considerations
- Additional messaging platform support

## Installation
```ya
version: '2.4'

services:
  monocker:
    container_name: monocker
    image: petersem/monocker
    environment:
      # Optional label to preface messages. Handy if you are running multiple versions of Monocker
      SERVER_LABEL: 'Dev'
      # Specify the messaging platform and details, or leave blank if only wanting container logs (pick one only)
      MESSAGE_PLATFORM: 'telegram@your_bot_id@your_chat_id'
      # MESSAGE_PLATFORM: 'pushbullet@your_api_key@your_device_id'
      # MESSAGE_PLATFORM: 'pushover@your_user_key@your_app_api_token'
      # MESSAGE_PLATFORM: 'discord@webhook_url'
      # MESSAGE_PLATFORM: 'ntfy.sh@ntfy_url@topic@optional_token'
      # MESSAGE_PLATFORM: ''
      # Optional - includes or excludes specified containers - default behaviour is false
      LABEL_ENABLE: 'false'
      # Optional - only show when container state changes to being offline (paused, exited, running (unhealthy), or dead) - default is false
      ONLY_OFFLINE_STATES: 'false'
      # [Optional] - Regardless of any other settings, you can ignore or include 'exited'
      EXCLUDE_EXITED: 'false'      
      # [Optional] - Set the poll period in seconds. Defaults to 10 seconds, which is also the minimum. 
      PERIOD: 10
      # [Optional] - Supress startup messages from being sent. Default is false
      DISABLE_STARTUP_MSG: 'false'
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    restart: unless-stopped
```
- For Telegram: See documentation for how to obtain ID values
- For Pushbullet: Open Pushbullet in a browser and get device ID from URL [Example](https://raw.githubusercontent.com/petersem/monocker/master/doco/pbdeviceid.PNG)
- For Pushover: See pushover doco for user key and app token
- For Discord: See Discord doco for how to create a webhook and get the url
- For ntfy.sh: Set ntfy_url (=https://ntfy.sh for public server, else your own instance), topic (=channel) and optionally the access token (in case the topic is secured)

*(ntfy.sh was added based on [this pull request](https://github.com/petersem/monocker/pull/62/commits/0143e94dd5467b10519c51ce4ff66135d5816eed) by codebude to the original monocker repo.)*

#### LABEL_ENABLE
This is an optional value, and defaults to false if it is not specified. This feature allows you to specify (with labels) 'either' specific containers to monitor or exclude from monitoring. 
- If it is set to false, then all containers will be monitored `except` for ones with the following label in their YAML.
```ya
    labels:
      monocker.enable: 'false'
```
- If it is set to true, `only` containers with the following label will be monitored
```ya
    labels:
      monocker.enable: 'true'
```
- If you just want to monitor everything, then set `LABEL_ENABLE: 'false'` or just leave it out altogether.


> If you like my work, you can make a dontation to say thanks! [Buy me a coffee](https://www.paypal.com/paypalme/thanksmp)


> Discuss issues or feature requests in the monocker channel on [Discord](https://discord.gg/NcKJTKN9yP)

This application uses *semantic* versioning. See [here](https://semver.org/) for more details. 

Link to code on [GitHub](https://github.com/petersem/monocker)
