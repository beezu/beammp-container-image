<p align="center">
  <img src="https://raw.githubusercontent.com/RouHim/beammp-container-image/main/logo.svg" width="250">
</p>


<p align="center">
    This project provides a container image for the <a href="https://beammp.com">BeamMP</a> 
    game server and shows its usage in a docker-compose environment.
</p>

## Motivation

This is a fork of Rouhim's BeamMP repo that's been lightly modified. The goal of this fork is to make it easier for people with existing BeamMP servers to move to Docker. It does away with environment variables in favor of passing through a Serverconfig.toml file.

## Variants

Rouhim publishes a [stable](https://github.com/BeamMP/BeamMP-Server/releases/latest) and [unstable](https://github.com/BeamMP/BeamMP-Server) version of BeamMP Server; this repo will only be publishing stable releases. Rouhim is (currently) only publishing amd64 images on Docker Hub, but my repo will be publishing both amd64 and arm64. [My stable releases can be found here](https://hub.docker.com/r/beezu/beammp-server).

The server is lightweight enough that two server instances can be run on the same nano AWS ARM instance (which are cheaper than the amd64 equivalents) with plenty of resources to spare. I personally am using a t4g.nano instance.

## Usage

The sections below provides use cases for docker and docker-compose.

### docker

Quick start:

```bash
docker run --name beammp-server \
           -p 30814:30814/tcp -p 30814:30814/udp \
           -v ./ServerConfig.toml:/beammp/ServerConfig.toml \
           beezu/beammp-server
```

### docker-compose

This is the recommended method, instead of a docker run command.

First, clone this repository and check `docker-compose.yml`

To get started, create the mod folders:

```bash
mkdir client-mods server-mods
```

Once you've downloaded and added the mods you want, run:
```bash
docker-compose pull && docker-compose up -d
```

## Client mods

In the first place you should consider
reading [the official mods guide](https://wiki.beammp.com/en/home/server-installation#how-to-add-mods-to-your-server).
Mods can be downloaded from the [official BeamNG resources website](https://www.beamng.com/resources/). Just copy the
downloaded zip file into the `client-mods` folder.

### Custom maps

Copy the downloaded zip file into the `client-mods` folder.

Then have to find out the custom map path name (e.g.: `/levels/car_jump_arena/info.json`), to set it later as the map to
load. To do so:

1. Execute the shell command below, or open the zip file manually.
2. Copy the absolute path to the `info.json` location (`/levels/{map-name}/info.json`).
3. Set in .env file: `MAP=/levels/{map-name}/info.json`. Example: `MAP=/levels/car_jump_arena/info.json`

A simple way to print the full map path including info.json (_unzip_, _grep_ and _awk_ is required):

```shell
unzip -l PATH/TO/MAP.zip \
  | grep 'levels/.*/info.json' \
  | awk '{split($0,a," "); print "/"a[4]}'
```

## Server mods

Server mods can be found in the [BeamMP forum](https://forum.beammp.com/c/resource-plugin-area/server-resources).
Installation and configuration instructions are provided by each mod.

## Resources

- Rouhim's repository: https://github.com/RouHim/beammp-container-image
- BeamMP server repository: https://github.com/BeamMP/BeamMP-Server
- Official server maintenance guide: https://wiki.beammp.com/en/home/server-maintenance
- Official server installation guide: https://wiki.beammp.com/en/home/server-installation
- Inspired by: https://github.com/mastamic-ian/BeamMP_docker
- Built from: https://github.com/beezu/beammp-container-image
- Built to: https://hub.docker.com/r/beezu/beammp-server
