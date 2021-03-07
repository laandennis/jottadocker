# jottadocker

Available on docker hub as: [laandennis/jottadocker](https://hub.docker.com/r/laandennis/jottadocker)
fork of [maaximal/jottadocker](https://hub.docker.com/r/maaximal/jottadocker)

In order to persist the config add /var/lib/jottad as a mount or volume.

Add paths to backup as mounts under /backup/...
Each subfolder there is added as a backup path in jotta-cli

Login using env variables:
- JOTTA_TOKEN: Your JottaCloud personal login token
- JOTTA_DEVICE: The Device name for the JottaCloud backups 
- JOTTA_SCANINTERVAL: The scaninterval for the JottaCloud backups
- JOTTA_MAXDOWNLOADS: The amount of concurrent downloads (max=6)
- JOTTA_MAXUPLOADS: The amount of concurrent uploads (max=6)
- JOTTA_DOWNLOADRATE: The download rate (0=unlimited)
- JOTTA_UPLOADRATE: The upload rate (0=unlimited)
- LOCALTIME: The [timezone file](https://packages.debian.org/sid/all/tzdata/filelist) in the docker image (e.g. Europe/Berlin)

Units:
  download and upload rates:
   k,KB,kb = KiloByte ( 1000 bytes )
   m,MB,mb = MegaByte ( 1000 KiloBytes )
   etc

  scaninterval:
   10m       ( every 10 minutes )
   5h30m     ( every 5 hours and 30 minutes )

To add a [ignore file](https://docs.jottacloud.com/en/articles/1437235-ignoring-files-and-folders-from-backup-with-jottacloud-cli) mount it to /config/ignorefile


my example docker-compose

version: '3.9'
services:
    jotta:
        container_name: jotta
        image: laandennis/jottadocker
        volumes:
            - /docker/jotta:/var/lib/jottad
            - /docker:/backup/docker
            - /media:/backup/media
        environment:
            - TZ=$TZ
            - JOTTA_TOKEN=$JOTTA_TOKEN
            - JOTTA_DEVICE='nas'
            - JOTTA_SCANINTERVAL=24h
            - JOTTA_MAXDOWNLOADS=6
            - JOTTA_MAXUPLOADS=6
            - JOTTA_DOWNLOADRATE=0
            - JOTTA_UPLOADRATE=0

