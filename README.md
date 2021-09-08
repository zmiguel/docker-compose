
# Docker-compose Files
This repository hosts a public version of the docker-compose files I use in my homelab.
They are compose 3.8 compatible and run on docker v20+

## Requirements
- [Docker](https://www.docker.com/)
- [Docker-Compose](https://docs.docker.com/compose/install/)

## Docker Network
Each stack has a few networks to connect to eachother.

Network | Purpose
----|----
LabNetwork | Connects containers to the traefik reverse proxy
Adminer | Connect databases to the adminer container
Default | default network of each stack for intra stack comunication

-----

## Stacks
I organize my containers in stacks.

Each stack has containers that either rely on eachother or are part of a group of apps.
for example, the plex stack has all apps related to plex such as: sonarr, radarr, jackett, etc.

### Auth Stack
Provides a source of truth for my services using Authelia with openLDAP as a backend;
Includes password manager.

Container | Purpose
----|----
Authelia | Authentication provider for traefik
redis | Used by Authelia
postgres | Storage for Authelia
openldap | User database for Authelia
ldap-user-manager | Web interface to manage LDAP database
bitwarden | Password manager

### Comms Stack
Provides communication services for me and my friends. 

Container | Purpose
----|----
teamspeak | TeamSpeak server for voice chat
sinusbot | Bot for TeamSpeak, manages idle users and allow you to play music
xmpp | Ejabberd XMPP server
xmpp_postgres | Database for XMPP server
certdumper | Dumps traefik SSL certificates so they can be used in XMPP

### Dev Stack
Stack with a few useful development tools

Container | Purpose
----|----
code-server | VSCode in your browser
jetbrains-projector | JetBrains IDEs in your browser
gitea | Git server
postgres | database for Gitea

### Games Stack
Pterodacyl game hosting panel

Container | Purpose
----|----
ptero-certdumper | Dumps traefik SSL certificates so they can be used in pterodacyl
panel | the main pterodacyl panel container
worker | worker for pterodacyl
cron | cron for pterodacyl
cache | redis for pterodacyl
mysql | database for pterodacyl

### Geth Stack
Ethereum node

Container | Purpose
----|----
geth | Ethereum node

### Invoice Stack
Invoicing for services I run

Container | Purpose
----|----
server | web server for invoiceninja
invoiceninja | invoiceninja invoicing software
db | mysql server for invoiceninja

### ISEC Stack
This stack hosts nextcloud so we can share class notes and exams from our university with our classmates, whitout relying on the university. We got tired of them deleting stuff every year #DataHoarder.

Container | Purpose
----|----
nextcloud-isec | Nextcloud
postgres-isec | Nextcloud Database

### Main Stack
This stack has all the essential services that run on my server

Container | Purpose
----|----
Traefik | Reverse proxy for all other services
portainer | Web interface to manage your docker containers
adguard | Network-wide ad blocker
duplicati | Backup manager
samba | Share folder in the home network
watchtower | Automatic docker container updates
adminer | Web interface to manage your databases
wg-easy | VPN do securely connect to your home network outside of your home
syncthing | sync data between devices
languagetool | gramarly, but better

### Metrics Stack
Everyone loves graphs and stats from their server

Container | Purpose
----|----
influxdb | Time-based databse for time related metrics
influxdb2 | Time-based databse for time related metrics V2
chronograph | View and manage InfluxDB data
grafana | Dashboard to view all the data you are collecting
shynet | Alternative to google analytics 
shynet_db | Database for shynet
prometheus | Metrics collector
speedtest | metrics about your internet speed
scrutiny | disk SMART data on a neat website

### Plex Stack
Everything related to plex and media management

Uses Nvidia GPU on some containers

Container | Purpose
----|----
prowlarr | BitTorrent site aggregator
sonarr | TV Show manager
radarr | Movie manager
lidarr | Music manager
bazarr | Subtittle manager
tautulli | Plex stats and newsletters
petio | Allow user to request new media
mongo-petio | Database for petio
qbittorrent | BitTorrent client
plex | Plex media server itself
tdarr | Transcode manager, h264 -> h265 to save space on disk
tdarr-node | Tdarr node that does the actual transcoding
varken | plex metrics in grafana

### Tracker Stack
I decided to run my own open BitTorrent tracker
> Please note: this is just a tracker, it's a public tracker, all this does is share peer information between other peers. I have no idea what files are being shared, nor I care about that. I also have no control over what people share.
> Use at your own risk, HIGLY RECOMMEND Blocking chinese, russian, and other shady countries at a DNS level for safety.

Container | Purpose
----|----
tracker-redis | Redis to keep tracker data
chihaya | Open source bittorrent tracker
tracker-prometheus | Prometheus for metris on tracker

### Unifi Stack
Unifi controller and related

Container | Purpose
----|----
certdumper | Dumps traefik certificates so they can be using in the unifi controller
unifi-controller | Unifi network controller
unvr | Caddy proxy for UNVR appliance, traefik doesn't play nice proxying it directly

### Web Stack
Mostly website or web utilities

Container | Purpose
----|----
xbb | ShareX upload server, share your screenshots, files, etc with people
xbb-db | Database for xbb
ghost | My personal blog
ghost_db | Database for my personal blog
grocy | Manage your groceries and household tasks
web_mysql | mysql database for bookstack
bookstack | documentation
planka | kanboard done right
postgres-board | database for planka
flame | a dashboard
