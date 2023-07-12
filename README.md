# 1. hamachi-vpn

Containerized Hamachi VPN

<!-- TOC -->

- [1. hamachi-vpn](#1-hamachi-vpn)
  - [1.1. Purpose](#11-purpose)
  - [1.2. Pinned Hamachi Release Version](#12-pinned-hamachi-release-version)
  - [1.3. Bug Reports](#13-bug-reports)
  - [1.4. Will it Work On](#14-will-it-work-on)
  - [1.5. Examples](#15-examples)

<!-- /TOC -->

## 1.1. Purpose

To create a means of launching a Hamachi VPN client for hub / spoke configurations in a NAS or other environment that does not meet first-class support requirements for Hamachi on Linux but does have:

- Support for tunnels
- Support for Docker

## 1.2. Pinned Hamachi Release Version

As of 05/07/2019 this is pinned to 2.1.0.203.

## 1.3. Bug Reports

If you are submitting a bug report submit logs with it. The logs for the container may contain sensitive information like IP addresses so feel free to redact those. If you feel confident that there's megabytes of information that have absolutely no relevance to a giant glaring stderr critical failure, feel free to omit those. But please at least submit some kind of logs pertaining to the failure so there's something to go on.

Also, keep in mind I am in no way affiliated with vpn.net or LogMeIn so if it's a failure in Hamachi itself all I can do is relay the information and update the container when a new version resolving the issue is released.

Which brings us to...

## 1.4. Will it Work On

- Synology? Yes, provided you are running the latest. It may run on earlier than the latest 6.2 but Synology has a bad habit of lagging behind on Docker installations so I wouldn't go too far back.

- Other NAS solutions? Potentially. I don't own anything other than my Synology swarm so I really couldn't tell you but the criteria are very straightforward. See above under Purpose.

- Non-NAS Linux deployments? Sure, really no reason why not, provided you aren't running a deployment so stripped down it doesn't support the things you need to run a VPN client. See above under Purpose.

- Mac / Windows? No. This container needs to use the 'host' network mode which only works correctly on Linux systems.

## 1.5. Examples

*docker-compose.yml* is provided as an example of how to run the container. It's really straightforward intentionally:

```yaml
version: "3"
services:
  hamachi:
    image: gtfunes/hamachi
    container_name: hamachi
    restart: unless-stopped
    network_mode: host
    cap_add:
      - NET_ADMIN
    volumes:
      - ${ROOT}:/var/lib/logmein-hamachi:rw
      - /etc/localtime:/etc/localtime
    environment:
      - ROOT=<Configuration root folder>
```

When the container comes up it will do the initial Hamachi setup and login to the network. In order to attach this client to your account you will need to do a one time setup. Go into your container and run 'hamachi attach <youremail@address.com>'.

Once you approve the request and add the now logged-in client to a network that's it, the client is active and its IP address can be used to bring up services.
