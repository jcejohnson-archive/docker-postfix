# tragus/postfix
tragus/ubuntu with postfix installed & configured

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
    -p 0.0.0.0:25:25 \
    -p 0.0.0.0:143:143 \
    -p 0.0.0.0:993:993 \
    -p 0.0.0.0:143:143 \
    -p 0.0.0.0:465:465 \
    -v /usr/local/etc/postfix:/etc/postfix \
    --name postfix tragus/postfix
```

## Persistent Data

postfix will care about these locations:

- /var/log/postfix
- /etc/postfix

You should map those to some out-of-container storage so that they persist across instances.

## Warning

There is no logfile management in the container at this time.
You may want to map /var/log/postfix (or all of /var/log) to
host storage or a data-only container.

```
docker run -d \
    ...
    -v /usr/local/log/postfix:/var/log/postfix \
    --name postfix tragus/postfix
```

