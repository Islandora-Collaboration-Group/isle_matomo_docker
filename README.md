# isle matomo docker

This repository is just a slight deviation from the original [ISLE project](https://github.com/Islandora-Collaboration-Group/ISLE) adding the Matomo FPM container
and a NIGX webserver to the mix.
Main changes are

 * Changed docker-compose version from 3.0 to 3.2 to allow the extended Port mapping definition (to force Traefik to forward client IP headers to Matomo)
 * A matomo 3.9.1 container using FastCGI PHP 7.x with a bind mounted config folder at `./isle-matomo-config`
 * A matomo NGIX container that serves the matomo application files using a bind mounted config folder at `./isle-matomo-nginx`
 * Uses Traefik to route requests to matomo.isle.localdomain (host, external) to internal NGIX port, allowing this way SSL.
 
Of course this adds the need to have a matomo.isle.localdomain entry in your `/etc/hosts`, full entry looks like

File: /etc/hosts
```Shell
127.0.0.1	localhost
127.0.0.1       isle.localdomain
127.0.0.1       matomo.isle.localdomain
127.0.0.1       portainer.isle.localdomain
127.0.0.1       whoami.isle.localdomain
````
Since Matomo does not allow a fully automated deployment (yet) this requires to follow the Installation Process a first time, reusing, for the DB connection the host and credentials 
used for the `isle-mysql-ld` container (see `.env`)

For the actual deployment/delivery we will provide a replacement entry point `.sh` file (dev and production) that will deal with most of the settings.
