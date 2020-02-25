# Traefik setup

## What is traefik ?

Traefik is a loadbalancer and reverse proxy with lots of majik inside. In my use case I chose it because of it's Docker integration.

See https://containo.us/traefik/ for more info.

## Setup

This is for setting up traefik itself.

Create a `.env` file in the same dir and setup the following environment variables inside:
```bash
OWNER_EMAIL=<youremail@yourdomain>
VOLUME_ROOT=<the directory where configs should be saved>
```
The email address is used for Letsencrypt certificates.
The volume root is used to set the base folder where configs are saved.

The `.env` file is used to hold things as credentials and it should *never* be pushed to git.

In my repo it's added to .gitignore.

Once this is done you can start the container with:

```bash
docker-compose up
```

To start it in background:
```bash
docker-compose up -d
```

Connect to port 8080 on your machine and you should get the traefik dashboard.

## What now?

Now if you want to expose a service simply add the following in docker-compose.yaml :

```yaml
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.drone.rule=Host(`my.nice.domain`)"
      - "traefik.http.routers.drone.entrypoints=websecure"
      - "traefik.http.routers.drone.tls.certresolver=myhttpchallenge"
```
This will tell traefik to use this one as a backend, sending over all traffic for "my.nice.domain" on port 443. 
This is a very simple setup you are going to need to change things for your own setup.
