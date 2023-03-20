Configuration
=============

This will provide you the steps in configuring your _unrealircd_ server to be able
to link into the BonoboNET IRC network.

## Contacting the admins

Before you are able to link in you will need to acquire some information from the
network administrators, you can contact one of the two network administrators
with a request to join BonoboNET and the server's name that you have chosen
(this must end with `.bnet`, so `<myName>.bnet`)

1. `deavmi` (deavmi@redxen.eu)
2. `rany` (ranycrxn@riseup.net)

After contacting a network administrator you will be given two pieces of information,
one of which will be your `sid` (server identifier) and another will be a so-called
_"remote include"_ which will be used later in your configuration file.

## Configuring the files

After building unrealircd you should have a sub-directory named `conf/`, you should
create a new blank file named `unrealircd.conf` within it. If a file with such name
already exists then you will need to delete it and create an empty one form scratch.

You are to add the following lines of code in the order that they appear in this tutorial.

### Default modules

There should be some configuration files created for you by the build process, and
they should be included by pasting the following into your config:

```
/* Load modules */
include "modules.default.conf";
include "modules.optional.conf";
include "snomasks.default.conf";
include "operclass.default.conf";
```

### Remote inclusion

Recall the line you got sent in the e-mail referred to as the _"remote include"_? Well,
now place that line here:

```
/* Remote configuration base */
include "<remote inclusion url>";
```

### Server information

You will now personalise the settings of this server by setting the _name_ of your
server to the one you chose in the e-mail. Along with this a custom **short** description
of your server, followed by the `sid` you were sent in the e-mail.

```
/* IRC node information */
me
{
        name "<server name>.bnet";
        info "<short description>";
        sid <sid>;
}
```

Following this we will want to add some contact information for your server which will
allow a user of your server to find contact information for your server.

```
/* Administrator contact information */
admin
{
	"<full name>";
	"<screen name>";
	"<email address>";
}
```

### Operator information

Since this is _your_ server you will be able to configure an operator account that
can only be used to gained priveleged control over _your_ server. You should set
the nickname for your operator user and put that in `<username>`. Also, set a strong
password that will be used for gaining access to operator status.

```
/*
* Operator information
*/
oper <username>
{
        class opers;
        mask *@*;
        password "<operator password>";
        operclass netadmin;
        swhois "is a Network Administrator";
        vhost administrator.bnet;
}
```

1. It is important you keep the `vhost` set as `administrator.bnet`

### Listening configuration

We now need to configure what ports your server will listen on.

#### Clients

For client connections we will let the server bind to all addresses
and listen on port `6667`:

```
/* Client (plaintext) */
listen
{
        ip *;
        port 6667;
}
```

We will also have a socket listen on `6697` which has TLS enabled on
it such that users can connect ina  secure manner if they so choose to:

```
/* Client (TLS) */
listen
{
        ip *;
        port 6697;
        options { tls; }
}
```

#### Servers

The servers need to be able to link to each other and they do so over TLS,
so add the following:

```
/* Server (TLS) */
listen
{
        ip *;
        port 6900;
        options { tls; serversonly; }
}
```

### Services configuration

There are some restraints we need to include some configuration to allow
a certain server, namely `services.bnet`, to be able to have special rights
aseeing as it provides IRC services to the whole network. We also add another
rules banning the use of nicknames that are used for services.

```
/* Give special rights to the IRC services */
ulines
{
        services.bnet;
}
```

TODO: the above should be added into remote config?