# tragus/postfix
tragus/ubuntu with postfix installed & configured

This is a work in progress. Do not use it yet.

## Building the image

```
git clone https://github.com/jcejohnson/docker-postfix.git tragus-postfix
cd tragus-postfix
docker build -t tragus/postfix .
```

## Running the container

Modify & use postfix.launch to start the container.

```
docker run -d -t \
    ... TBD ... \
    -v /usr/local/etc/postfix:/etc/postfix \
    -v /usr/local/log/postfix:/var/log/postfix \
    --name postfix tragus/postfix
```

The container ships with postfix unconfigured. You will need to launch the container
with a shell, reconfigure postfix and extract the configuration to a host path.
See docker run -i -t --entrypoint /bin/bash --name postfix tragus/postfix

```
host> docker run -i -t --entrypoint /bin/bash --name postfix tragus/postfix
container> dpkg-reconfigure postfix
    ...
container> dpkg-reconfigure postfix
container> postconf -e ...
container> ...
container> tar czf /postfix-configured.tgz /etc/postfix
container> exit
host> docker cp postfix:/postfix-configured.tgz /usr/local
host> docker rm postfix
host> tar xzf -C /usr/local /usr/local/postfix-configured.tgz
```

## Persistent Data

postfix will care about these locations:

- /var/log/postfix -- TBD
- /etc/postfix

You should map those to some out-of-container storage so that they persist across instances.

## Warning

There is no logfile management in the container at this time.
You may want to map /var/log/postfix (or all of /var/log) to
host storage or a data-only container.

```
docker run -d \
    ...
    -v /usr/local/etc/postfix:/etc/postfix \
    -v /usr/local/log/postfix:/var/log/postfix \
    --name postfix tragus/postfix
```

