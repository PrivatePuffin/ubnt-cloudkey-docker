# Ubiquiti CloudKey in Docker

The quarantine boredom has taken over and I decided to mess with ubiquiti's protect software.

After a long journey of digging, experimenting and testing, I found that the cloudkey software can be run in a docker container!

It might not be perfect, or something ubiquiti would ever release, but its a fun little project to show how well ubiquiti sofware works in containers! (As they now proved themselfes on the latest UDM firmware)

I will warn you: **This is and always will be just for training and my personal entertainment** ... and because my UDM had issues with dnsmasq.

Unless Ubiquiti is fancy to take over the project haha.

The repository does not contain any ubiquiti sourcecode, all files are downloaded with APT form their public APT repository.

Please also note that this repo may break on updates as the versions I replace may change. But for you tech-savy people it should be easy to figure out where to change the version strings :P

## Disclaimer
I am not liable for anything you do with this. I dont recommend using it, this is just what I found while tinkering. If ubiquiti comes knocking on the door it wont be my fault.

Ubiquiti: If you want this taken down, message me at team@infinytum.co and I will set the repo to private. I have no interest in fighting with you guys.

## How to use it:
Change the necessary fields in rootfs/usr/local/bin/ubnt-tools.content. You can spot them easily:
- UUID
- Serial
- BOM maybe. Not sure. 

Building the docker image is a *big* part of using the thing. I recommend using docker build :P

```
docker build -t awonderful/image .
```

If that works, the biggest problems have been faced already.

How do you run it? Well docker run. But you might wanna throw in some options too!

```
docker run -it --tmpfs /run --tmpfs /run/lock -v /sys/fs/cgroup:/sys/fs/cgroup:ro --tmpfs /tmp \
	--network=host \
	-v <SAFESPACE>/postgres:/var/lib/postgresql/9.6/main \
	-v <SAFESPACE>/sdcard:/srv \
	-v <SAFESPACE>/emmc:/srv-internal \
	-v <SAFESPACE>/hdd:/data \
	awonderful/image
```

or

```
docker run -it --tmpfs /run --tmpfs /run/lock -v /sys/fs/cgroup:/sys/fs/cgroup:ro --tmpfs /tmp \
	-p 7442:7442 \
    -p 7444:7444 \
    -p 7447:7447 \
    -p 7550:7550 \
	-p 443:443 \
	-v <SAFESPACE>/postgres:/var/lib/postgresql/9.6/main \
	-v <SAFESPACE>/sdcard:/srv \
	-v <SAFESPACE>/emmc:/srv-internal \
	-v <SAFESPACE>/hdd:/data \
	awonderful/image
```

This will give the image all it needs to be persistent and usable. I recommend --network=host to avoid discovery issues.

I highly recommend to not run this image in production too. You can try it to learn how ubiquiti's software is made.