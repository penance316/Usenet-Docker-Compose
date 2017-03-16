# Usenet-Docker-Compose
docker-compose.yml file to quickly setup a usenet box

## Setup host folders
- sudo -i
- mkdir -p /docker/containers/{couchpotato,plex,sonarr,sabnzbd}/config
- chown -R nero:nero /docker

## Notes
I am using linuxserver.io docker images as they are very simple to get up and running.

```yml
  sabnzbd:
    image: linuxserver/sabnzbd
    container_name: sabnzbd
    # keeps servers up and running and starts on boot
    restart: unless-stopped
    ports:
      - "8080:8080"
      - "9090:9090"
    # set user id and group id to avoid permission issues with folders and files
    environment:
      - PUID=1000
      - PGID=1000
      - TZ:Europe/London
    # mount common /docker/containers config folder
    # mount data pool dir
    volumes:
      - /docker/containers/sabnzbd/config:/config
      - /media/Pool/downloads:/downloads
```

##### Watchdog (auto update)
Watchdog container used to keep the other images updated.
```yml
  watchtower:
    image: v2tec/watchtower
    container_name: watchtower
    restart: unless-stopped
    # cleanup flag set to remove out of date images
    command: --cleanup
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
