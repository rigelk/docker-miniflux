# rigelk/miniflux
[Miniflux](https://miniflux.net/) is an open source web-based news feed (RSS/Atom) reader and aggregator.
Miniflux is not for everyone, it's for people who want to read their feeds efficiently.

## Usage

```
docker create \
--name=miniflux \
-v /etc/localtime:/etc/localtime:ro \
-v <path to data>:/config \
-e PGID=<gid> -e PUID=<uid> \
-e TZ=<timezone> \
-p 80:80 \
rigelk/miniflux
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side.
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 80` - webui port *see note below*
* `-v /etc/localtime` for timesync - *optional* *omit if using TZ variable*
* `-v /config` - where miniflux should store its config and data files
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e TZ` for setting timezone information, eg Europe/London

It is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it miniflux /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to
specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Info

* To monitor the logs of the container in realtime `docker logs -f miniflux`.

## Differences from miniflux/miniflux

* Based off Alpine with the s6 overlay
* Included cronjob which updates feeds every hour

## Thanks

This template is based off [the tt-rss template](https://github.com/linuxserver/docker-tt-rss) by [LinuxServer](https://www.linuxserver.io/). Cheers! (but don't use tt-rss)
