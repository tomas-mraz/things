# Keycloak

## Installation

- https://www.keycloak.org/docs/latest/server_installation/
- https://medium.com/@hasnat.saeed/setup-keycloak-server-on-ubuntu-18-04-ed8c7c79a2d9
- Ubuntu 18.04, 4 GB RAM
- Pre-requisite: `sudo timedatectl set-timezone Asia/Kolkata`

### Install Java

```bash
sudo apt-get update
sudo apt-get install -y default-jdk
```

### Install Keycloak

```bash
echo 'export KEYCLOAK_VERSION=9.0.3' >> $HOME/.profile
mkdir -p /tmp/keycloak
cd /tmp/keycloak
wget -O keycloak-${KEYCLOAK_VERSION}.tar.gz https://downloads.jboss.org/keycloak/9.0.3/keycloak-${KEYCLOAK_VERSION}.tar.gz
sudo tar -C /opt -zxvf keycloak-${KEYCLOAK_VERSION}.tar.gz
```

#### Keycloak users

```bash
sudo groupadd keycloak
sudo useradd -r -g keycloak -d /opt/keycloak -s /sbin/nologin keycloak
```

#### Keycloak permissions

```bash
sudo chown -R keycloak:keycloak /opt/keycloak-${KEYCLOAK_VERSION}
sudo chmod -R o+rwx /opt/keycloak-${KEYCLOAK_VERSION}/
```

#### `systemd` service

```bash
sudo mkdir -p /etc/keycloak-${KEYCLOAK_VERSION}
sudo cp /opt/keycloak-${KEYCLOAK_VERSION}/docs/contrib/scripts/systemd/wildfly.conf /etc/keycloak-${KEYCLOAK_VERSION}/keycloak-${KEYCLOAK_VERSION}.conf
```

#### Copy launch script

```bash
sudo cp /opt/keycloak-${KEYCLOAK_VERSION}/docs/contrib/scripts/systemd/launch.sh /opt/keycloak-${KEYCLOAK_VERSION}/bin/
sudo chown keycloak:keycloak /opt/keycloak-${KEYCLOAK_VERSION}/bin/launch.sh
```

#### Edit `launch.sh`

```bash
sudo cp /dev/null /opt/keycloak-${KEYCLOAK_VERSION}/bin/launch.sh
sudo nano /opt/keycloak-${KEYCLOAK_VERSION}/bin/launch.sh
```

```sh
#!/bin/bash

if [ "x$WILDFLY_HOME" = "x" ]; then
    WILDFLY_HOME=/opt/keycloak-${KEYCLOAK_VERSION}
fi

if [[ "$1" == "domain" ]]; then
    $WILDFLY_HOME/bin/domain.sh -c $2 -b $3
else
    $WILDFLY_HOME/bin/standalone.sh -c $2 -b $3
fi
```

#### Copy service definition

```bash
sudo cp /opt/keycloak-${KEYCLOAK_VERSION}/docs/contrib/scripts/systemd/wildfly.service /etc/systemd/system/keycloak-${KEYCLOAK_VERSION}.service
```

#### Keycloak service

```bash
sudo cp /dev/null /etc/systemd/system/keycloak-${KEYCLOAK_VERSION}.service
sudo nano /etc/systemd/system/keycloak-${KEYCLOAK_VERSION}.service
```

```ini
[Unit]
Description=The Keycloak Server
After=syslog.target network.target
Before=httpd.service

[Service]
Environment=LAUNCH_JBOSS_IN_BACKGROUND=1
Environment=KEYCLOAK_VERSION=${KEYCLOAK_VERSION}
EnvironmentFile=/etc/keycloak-${KEYCLOAK_VERSION}/keycloak-${KEYCLOAK_VERSION}.conf
User=keycloak
Group=keycloak
LimitNOFILE=102642
PIDFile=/var/run/keycloak-${KEYCLOAK_VERSION}/keycloak-${KEYCLOAK_VERSION}.pid
ExecStart=/opt/keycloak-${KEYCLOAK_VERSION}/bin/launch.sh $WILDFLY_MODE $WILDFLY_CONFIG $WILDFLY_BIND
StandardOutput=null

[Install]
WantedBy=multi-user.target
```

#### Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable keycloak-${KEYCLOAK_VERSION}
sudo systemctl start keycloak-${KEYCLOAK_VERSION}

sudo systemctl restart keycloak-${KEYCLOAK_VERSION}
sudo systemctl status keycloak-${KEYCLOAK_VERSION}
sudo systemctl disable keycloak-${KEYCLOAK_VERSION}
```

#### Administration

```bash
cd /opt/keycloak-${KEYCLOAK_VERSION}/bin
./add-user-keycloak.sh -r master -u masteradmin -p Mast3r.Adm1n
sudo systemctl restart keycloak-${KEYCLOAK_VERSION}

./kcadm.sh config credentials --server http://localhost:8080/auth --realm master --user <admin-username>  –-password <admin-password>
./kcadm.sh update realms/master -s sslRequired=NONE
```

## Realm administration

### Realm management

- Add a new realm - `plant1`.
- Disable SSL for realm.
- _Clearing caches_
- _Email settings_
- _Themes_

### User management

- Add a new group
- Add a new user
