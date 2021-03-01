docker-no-ip (FORK)
============

This is a simple Docker container for running the [No-IP2](http://www.noip.com/) dynamic DNS update script. It will keep
your domain.ddns.net DNS alias up-to-date as your home IP changes. 

This docker image is available as a [docker container](https://hub.docker.com/repository/docker/uping/no-ip/general/).

I created this fork because the original version has a problem with loading environment variables. Original can be found [here](https://github.com/coppit/docker-no-ip) image. 

## Configuration
`USERNAME` username of your noip account  
`PASSWORD` password of your noip account  
`DOMAINS` A comma-delimited list of domains and/or groups you want to update.  
`INTERVAL` Interval you want to update the domains (examples: 1d,  2h, 30m) 

## Volumes  
`/etc/localtime` Needed so the container clock is running correctly  
`/config` Location of the (generated) config file

## Running modes
There are two modes of running this container. Check the example to find the one that is best suited for your situation.

### Environment variables
```
sudo docker run 
    --name=noip -d
    -v /etc/localtime:/etc/localtime   
    -v /config/dir/path:/config 
    -e USERNAME=<username> 
    -e PASSWORD=<password> 
    -e DOMAINS=<domains> 
    -e INTERVAL=<interval> 
    uping/no-ip
```

### Config files
The second mode is with a config file. To create a template config file, run:

```
sudo docker run 
    --name=noip -d 
    -v /etc/localtime:/etc/localtime 
    -v /config/dir/path:/config 
    uping/no-ip
```

When run for the first time, a file named noip.conf will be created in the config dir, and the container will exit. Edit this file, adding your username (email), password, domains, and update interval. Then rerun the command to start the container.

## Warning
In both modes, two config files (`no-ip2.generated.conf` and `no-ip2.generated.conf.breadcrumb`) will be generated. Please do not edit these files, as they are used by the noip2 agent

## Docker Compose example
```
version: "3"
services:
  noip:
    image: uping/no-ip
    container_name: noip
    restart: unless-stopped
    environment:
      - USERNAME=<USERNAME>
      - PASSWORD=<PASSWORD>
      - DOMAINS=<DOMAIN1>,<DOMAIN2>
      - INTERVAL=<INTERVAL>
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /path/to/config:/config
```
