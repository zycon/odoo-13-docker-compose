# Quick install

Installing Odoo 13 with one command.

(Supports multiple Odoo instances on one server)

Install [docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/) yourself, then run:

``` bash
curl -s https://raw.githubusercontent.com/zycon/odoo-13-docker-compose/master/run.sh | sudo bash -s odoo-one 10013 20013
```

to set up first Odoo instance @ `localhost:10013` (default master password: `minhng.info`)

and

``` bash
curl -s https://raw.githubusercontent.com/zycon/odoo-13-docker-compose/master/run.sh | sudo bash -s odoo-two 11013 21013
```

to set up another Odoo instance @ `localhost:11013` (default master password: `minhng.info`)

Some arguments:
* First argument (**odoo-one**): Odoo deploy folder
* Second argument (**10013**): Odoo port
* Third argument (**20013**): live chat port

If `curl` is not found, install it:

``` bash
$ sudo apt-get install curl
# or
$ sudo yum install curl
```

# Usage

Start the container:
``` sh
docker-compose up
```

* Then open `localhost:10013` to access Odoo 13.0. If you want to start the server with a different port, change **10013** to another value in **docker-compose.yml**:

```
ports:
 - "10013:8069"
```

Run Odoo container in detached mode (be able to close terminal without stopping Odoo):

```
docker-compose up -d
```

**If you get the permission issue**, change the folder permission to make sure that the container is able to access the directory:

``` sh
$ git clone https://github.com/zycon/odoo-13-docker-compose
$ sudo chmod -R 777 addons
$ sudo chmod -R 777 etc
$ mkdir -p postgresql
$ sudo chmod -R 777 postgresql
```

Increase maximum number of files watching from 8192 (default) to **524288**. In order to avoid error when we run multiple Odoo instances. This is an *optional step*. These commands are for Ubuntu user:

```
$ if grep -qF "fs.inotify.max_user_watches" /etc/sysctl.conf; then echo $(grep -F "fs.inotify.max_user_watches" /etc/sysctl.conf); else echo "fs.inotify.max_user_watches = 524288" | sudo tee -a /etc/sysctl.conf; fi
$ sudo sysctl -p    # apply new config immediately
```

# Custom addons

The **addons/** folder contains custom addons. Just put your custom addons if you have any.

# Odoo configuration & log

* To change Odoo configuration, edit file: **etc/odoo.conf**.
* Log file: **etc/odoo-server.log**
* Default database password (**admin_passwd**) is `minhng.info`, please change it @ [etc/odoo.conf#L60](/etc/odoo.conf#L60)

# Odoo container management

**Run Odoo**:

``` bash
docker-compose up -d
```

**Restart Odoo**:

``` bash
docker-compose restart
```

**Stop Odoo**:

``` bash
docker-compose down
```

# Live chat

In [docker-compose.yml#L21](docker-compose.yml#L21), we exposed port **20013** for live-chat on host.

Configuring **nginx** to activate live chat feature (in production):

``` conf
#...
server {
    #...
    location /longpolling/ {
        proxy_pass http://0.0.0.0:20013/longpolling/;
    }
    #...
}
#...
```

# docker-compose.yml

* odoo:13.0
* postgres:11

# Odoo 13 screenshots

![odoo-13-welcome-docker](screenshots/odoo-13-welcome-screenshot.png)

![odoo-13-apps-docker](screenshots/odoo-13-apps-screenshot.png)

![odoo-13-sales](screenshots/odoo-13-sales-screen.png)

![odoo-13-form](screenshots/odoo-13-sales-form.png)
