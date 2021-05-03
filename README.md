# ejabber

Before you build amazing things with xmpp you need to first setup the server !!!
Ejabberd, Prosody and Openfire are some of the famous xmpp server. All are great here we are going to use ejabberd as our xmpp server.

# Setup

Start the server/Install the server

```
docker-compose up
```

Register User

```
docker-compose exec xmpp-server bin/ejabberdctl register admin localhost passw0rd
User admin@localhost successfully registered
```

Similarly register other users as well (Incase when doing pubub/muc implementation )

```
docker-compose exec xmpp-server bin/ejabberdctl register hermione localhost hermione
docker-compose exec xmpp-server bin/ejabberdctl register albus localhost albus
docker-compose exec xmpp-server bin/ejabberdctl register hari localhost hari
docker-compose exec xmpp-server bin/ejabberdctl register harry localhost harry
```

Get auth token (Needed when interacting with ejabberd rest api)

```
docker-compose exec xmpp-server bin/ejabberdctl oauth_issue_token admin@localhost 33334444 ejabberd:admin
```

# Exploration

Navigate to ejabberd admin panel http://localhost:5443/admin and login with username: admin pwd: passw0rd
Enjoy :)

You can control ejabberd using Rest Api the best way to learn about them is to explore them in postman at https://documenter.getpostman.com/view/11453962/TVCb3Vc7

Also for additional rest commands similar to postman commands above visit https://docs.ejabberd.im/developer/ejabberd-api/admin-api/

Additional Notes:
For configuring ejabberd to control which services are allowed visit https://docs.ejabberd.im/admin/configuration/


---------------------------------------------------------------------------------------------------------------------

# Install Ejabberd
Docker compose file
Create /env/common.env file include :

```

#COMMON
ERLANG_NODE=ejabberd
EJABBERD_HTTPS=false
EJABBERD_PROTOCOL_OPTIONS_TLSV1_1=false

##VHOST AND ACCOUNT
XMPP_DOMAIN=localhost localhost1 localhost2 localhost3
EJABBERD_ADMINS=admin@localhost admin@localhost1 admin@localhost2 admin@localhost3
EJABBERD_USERS=admin@localhost:password admin@localhost1:password admin@localhost2:password admin@localhost3:password bss1@localhost:password bss2@localhost:password bss1@localhost1:password bss2@localhost2:password bss1@localhost3:password bss2@localhost3:password
Create /env/common.module file include :

## MODULES
EJABBERD_MOD_OAUTH_ENABLE=true
EJABBERD_MOD_OAUTH_PREFIX=/oauth
EJABBERD_MOD_HTTP_API_ENABLE=true
EJABBERD_MOD_HTTP_API_PREFIX=/api/v1
EJABBERD_MOD_MUC_ADMIN=true
EJABBERD_MOD_ADMIN_EXTRA=true
EJABBERD_REGISTER_ADMIN_ONLY=true
EJABBERD_MOD_MAM=true
EJABBERD_SKIP_MODULES_UPDATE=false
EJABBERD_RESTART_AFTER_MODULE_INSTALL=true
Create /env/mysql.module file include :

## MYSQL
EJABBERD_AUTH_METHOD=sql
EJABBERD_CONFIGURE_SQL=true
EJABBERD_DEFAULT_DB=sql
EJABBERD_SESSION_DB=sql
EJABBERD_SQL_TYPE=mysql
EJABBERD_SQL_SERVER=128.199.94.240
EJABBERD_SQL_DATABASE=ejabberd
EJABBERD_SQL_USERNAME=root
EJABBERD_SQL_PASSWORD=hoanggia3116
EJABBERD_SQL_POOL_SIZE=10
EJABBERD_SQL_PORT=3306
Create docker-compose.yml file include : * use mysql db * ejabberd admin page * port expose

5222
5269
5280
4560
5443
version: '2'
services:
  ejabberd-service:
    container_name: beesightsoft-ejabberd-service
    image: beesightsoft/debian-ejabberd:16.04
    ports:
       - 5222:5222
       - 5269:5269
       - 5280:5280
       - 4560:4560
       - 5443:5443
    env_file:
      - ./env/common.env
      - ./env/mysql.env
      - ./env/module.env
    external_links:
        - ejabberd-mysql
Run docker compose
docker-composer up -d
Ejabberd Admin
http://$ip:5280/admin
https://docs.ejabberd.im/static/images/admin/webadmin.png
Port
4560 (XMLRPC)
5222 (Client 2 Server)
5269 (Server 2 Server)
5280 (HTTP admin/websocket/http-bind)
5443 (HTTP Upload)

```







