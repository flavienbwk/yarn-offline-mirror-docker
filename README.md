# NPM Offline Registry

<p align="center">
<img src="https://travis-ci.org/flavienbwk/npm-yarn-offline-mirror-docker.svg?branch=master"/>
</p>

Dockerized Yarn / NPM / PNPM offline registry to host your own dependencies online or offline.

This repository contains tools that will allow you to download dependencies you need **online** and push them **offline**.

## Getting started

### Setting up credentials for registry (offline :electric_plug:)

In order for you to be able to **push** packages to the registry, you need to create a user account.  

The default credentials are : 

- Username `default`
- Password `default`

Credentials are stored in `registry/conf/htpasswd`

To add a user :

```
apt install apache2-utils -y
htpasswd ./registry/conf/htpasswd myuser
docker-compose restart registry
```

To remove a user, just remove its line in `registry/conf/htpasswd` and restart the `registry` container

### Starting the registry !

Just execute :

```
docker-compose up -d registry
```

### Downloading your dependencies (online :zap:)

You just have to **place the `/package.json` of the project** you want to get the packages from, **at the root of the directory**.

The script that downloads the packages downloads as well the dependencies of each package (and their dependencies... and so on).

Then, just run :

```
docker-compose up --build download
```

### Pushing your dependencies to your local registry (offline :electric_plug:)

This script will look into the `/packages` directory for `.tgz` package files downloaded from the _downloader_ script or from the archives you've added.

Just run :

```
docker-compose up --build push
```

### Testing if everything works (offline :electric_plug:)

You can check your registry installation with the _test_ script that will `yarn add` a package from the registry.

You can choose which package(s) to `yarn add` inside `/tester/entrypoint.sh`.

Then, just run :

```
docker-compose up --build test
```

### Installing your offline client (offline :electric_plug:)

This one is not about Docker. Here is the command to configure any NPM/Yarn client on your network to download packages from your local registry only :

- `yarn config set registry http://localhost:4873`

OR

- `npm config set registry http://localhost:4873`
