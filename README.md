# Initial Sever Setup
```
sudo apt update && sudo apt upgrade -y
```

```
sudo apt install -y ca-certificates curl gnupg lsb-release
```

### Install docker
look into official docker documentation to install

- add user to docker group
```
sudo usermod -aG docker $USER
```
```
newgrp docker
```

### Directory Structure
```
sudo mkdir -p /opt/infra
sudo chown -R $USER:$USER /opt/infra
chmod 700 /opt/infra
```

### Create Shared docker network ( proxy )

traefik and all apps must share one external network
```
docker network create proxy
```

### Traefik setup
```
mkdir -p /opt/infra/traefik/letsencrypt
touch /opt/infra/traefik/letsencrypt/acme.json
chmod 600 /opt/infra/traefik/letsencrypt/acme.json
```

### Create dashboard creds
```
htpasswd -nb admin STRONG_PASSWORD > /opt/infra/traefik/secrets/traefik-users.htpasswd
```

### How to make a db in postgres container ( one db per one app )

```
CREATE USER skillsync_user WITH PASSWORD 'STRONG_PASS_SKILLSYNC';
CREATE DATABASE skillsync OWNER skillsync_user;
```

### how the postgres url should be in .env
```
postgresql+psycopg2://<db-user>:<db-password>@postgres:5432/<db-name>
```

### optimising space on 1gb ram vm's

```
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

```