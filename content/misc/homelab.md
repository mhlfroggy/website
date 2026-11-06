---
date: '2026-04-17T00:00:00-05:00'
draft: false
title: 'Homelab'
---

Modern services typically make you choose between having either the convience of
a subscription and cloud services, or being able to have control over your
personal data. They also don't mention how you're at their mercy if they decide
to increase the prices of a service. Because of this, I decided to build a home
lab to serve as a secure, controlled environment for the data which I don't want
on the cloud.

Additionally, it also serves as a data host for music, movies and TV shows,
Pictures and Videos
- File backups
- Zettelkasten backup
- Minecraft Server

# Hardware
- Intel N100 for router (will run PfSense, OpenVPN, etc.)
- Unifi 8-port 2.5Gbps network switch
- Home Assistant Green
- USB Blu-Ray Drive
- Host node I prefer simplicity over complexity in my homelab, which is why I
  choose to use a single node as my host system.
    - Dell Precision 3360
    - Dell Precision 3680
    - HP Z2 Tower G9
    - Lenovo ThinkStation P360
    - Lenovo ThinkStation P3 Tower
    - What really matters for the system is that there's at least 32GB of memory
      (64 is better) and at least a 12th gen i5/i7 CPU for encoding on Jellyfin.

For the hard drives, I intentionally chose to use 3 8-12TB hard drives for my
setup as this means I can run RAIDZ1 to allow performance while also having some
disk parity/security.

# Software

- **OpnSense**
    - Hosted on the N100 to serve as my router and firewall
    - Also serves as a network-wide ad-blocker
- **Tailscale**
    - Root node is hosted on the N100 which is what everything connects to
- **Immich**
    - High performance self-hosted photo and video management which is designed
      to be an open-source version of Google/iCloud Photos. Includes automated
      backups, machine learning processing, facial recognition, etc.
    - Use [Immich-Go](https://github.com/simulot/immich-go) for merging
      metadata from photos on Google Photos with their corresponding pictures
      and automating the uploading of all data to Immich instance.
      ```
      zsh ./immich-go upload from-google-photos
      --server=http://my-website.com:8888 --api-key=*IMMICH_API_KEY*
      --concurrent-tasks=4 --client-timeout=60m --pause-immich-jobs=true
      --on-errors=continue --session-tag takeout-*.zip ```
- **Jellyfin**
    - My media server of choice. I host movies, television shows, animes, and
      audiobooks here.
    - Jellyfin isn't ideal for serving music as it doesn't tag some attributes
      properly, so instead we should utilize Navidrome instead.
- **Navidrome**
    - Navidrome is the better option for music because it includes explicit tags
      as well run as podman commands.
    - Command to rescan music library for Navidrome: `sudo podman exec
      systemd-navidrome ./navidrome scan -f`
- **Forgejo**
    - FOSS fork of Gitea designed for self-hosting Git instances.
    - Primarily used to house personal projects as well as my Zettelkasten
      database, as I don't want GitHub/Codeberg having access to that data.
- **Minecraft Server**
    - Use either [Paper](https://papermc.io/) or [Fabric](https://fabricmc.net/)
      for an optimized JAR file.
- **Open WebUI**
    - Open-source front end for Large Language Models
    - I use it as a front for OpenRouter instances for access to Chinese models
- **Filebrowser**
- **Homepage**
- **FreshRSS** 
- **Traefik**
- **Syncthing**
- **ddclient**

# Container Deployment
The base operating system is Proxmox VM running on the boot SSD, with all
containers being hosted elsewhere on the system.

| Service      | Type   | vCPU | RAM         | Notes                                                     |
| ------------ | ------ | ---- | ----------- | --------------------------------------------------------- |
| Jellyvin     | LXC    | 4    | 2-4GiB      | Share host iGPU via `/dev/dri` for Quick Sync transcoding |
| Navidrome    | LXC    | 2    | 512MiB-1GiB | Featherweight; bind-mount music library                   |
| Immich       | Docker | 4    | 8GiB        |                                                           |
| Forgejo      | LXC    | 2    | 1GiB        | Go binary, tiny footprint                                 |
| Paper/Fabric | LXC    | 4    | 4-8GiB      |                                                           |
| Filebrowser  | LXC    | 1    | 512MiB      | Bind-mount the data pool directly                         |
| Homepage     | LXC    | 1    | 512MiB-1GiB | Dashboard; trivial                                        |
| FreshRSS     | LXC    | 2    | 512-1GiB    | PHP + Postgres/SQLite; community script exists            |
| Syncthing    | LXC    | 2    | 1GiB        | Bind-mount whatever datasets it syncs                     |
| Open WebUI   | LXC    | 1    | 2GiB        |                                                           |
