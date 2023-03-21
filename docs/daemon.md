Configuration
=============

This will provide you the steps in configuring your _unrealircd_ server to be able
to link into the BonoboNET IRC network.

After building unrealircd you should have a sub-directory named `conf/`, you should
create a new blank file named `unrealircd.conf` within it. If a file with such name
already exists then you will need to delete it and create an empty one form scratch.

You are to add the following lines of code in the order that they appear in this tutorial.

## Default modules

There should be some configuration files created for you by the build process, and
they should be included by pasting the following into your config:

```
/* Load modules */
include "modules.default.conf";
include "modules.optional.conf";
include "snomasks.default.conf";
include "operclass.default.conf";
```

## Remote inclusion

Recall the line you got sent in the e-mail referred to as the _"remote include"_? Well,
now place that line here:

```
/* Remote configuration base */
include "<remote inclusion url>";
```

## Server information

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

## Operator information

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

## Listening configuration

We now need to configure what ports your server will listen on.

### Clients

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

### Servers

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

## Services configuration

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

## Server parametrs

There are some parameters that are specific to your server whch are required to be set, these are shown below:

### k-line address

The _k-line address_ is the e-mail address a user should use in order to get in contact with the admin of a server regarding a k-line they received on said server. This is set as follows:

```
/* k-line address */
set
{
        kline-address "<admin email address>";
}
```

1. Set the `<admin email address>` here to the same one you used in the `admin {}` block earlier

### Unchanging parameters

The following needs to be set as is:

The following settings should be set as is. They control the modes a user will have set in certain scenarios, what
modes a channel will have when created also which channel operators (such as you) should join to.

> **TODO**: Rany I suppose these should be in the common config actually?

```
/* Modes and oper auto-join */
set
{
        modes-on-connect "+ixw";
        modes-on-oper "+xws";
        modes-on-join "+nt";
        oper-auto-join "#opers";

        options
        {
                hide-ulines;
                show-connect-info;
        }
}
```

### Spam protection

The following settings relate to controlling potential spam-like activity on your server.

```
set
{
    /* Minimum connection time before valid use of `/QUIT` */
    anti-spam-quit-message-time 10s;

    /* Settings for spam filter */
    spamfilter
    {
        ban-time 1d; /* default duration of a *LINE ban set by spamfilter */
        ban-reason "Spam/Advertising"; /* default reason */
        virus-help-channel "#help"; /* channel to use for 'viruschan' action */
        /* except "#help"; channel to exempt from Spamfilter */
    }
}
```

1. The first option is `anti-spam-quit-message-time`
    * This is to be set to the number of seconds that someone must be connected before using the `/quit` command
    * If the user uses the command below the bound then it is considered spammy and they will be marked as spammy users
    * We recommend the default of `10s` (10 seconds)
2. 

1. What we can change

Probably should be common maybe not sure

### Restrictions stuff

TODO: Describe

```
set
{
    restrict-commands
	{
		list
		{
            connect-delay 60;
            exempt-identified yes;
            exempt-reputation-score 24;
        }

		invite
		{
            connect-delay 120;
            exempt-identified yes;
            exempt-reputation-score 24;
        }
	}
}
```

1. What we can change

Probably should be common maybe not sure


### COnn thrttoe stuff

TODO: desribe

```
set
{
	connthrottle
	{
		known-users
		{
            		minimum-reputation-score 24;
            		sasl-bypass yes;
        	}

		disabled-when
		{
            		reputation-gathering 1w;
        		start-delay 3m;
	        }
	}
}
```

1. what can we change

I don't know if this can be common config, there was a reason I kept it

### yolo

```

/*
* Misc.
*/

drpass
{
        restart "restart";
        die "die";
}

log {
        source {
                all;
                !debug;
        }
        destination { syslog; }
}
```



### Maximum channels

You can configure how many channels a single user may be a member of with this parameter:

```
/* Maximum number of channels a user can be a member of */
set
{
        maxchannelsperuser <number>;
}
```

1. Set `<number>` to a positive number
        * It is recommended you make this reasonably large enough; nobody wants to join a server where they cannot maintain multiple channel memberships
        * `100` is a good number

### Links

The last thing you will place in your configuration file is an inclusion for a new file that will be created when you move onto the [Linking](/linking/) section, therefore add the following:

```
/* Links */
include "links.conf";
```