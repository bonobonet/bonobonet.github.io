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

## Cloaking module

A cloaking module needs to be loaded, I have decided to use the
`cloak_md5` module, which can be loaded by placing this directive
in your config file:

```
/* Need to use _some_ cloaking module */
loadmodule "cloak_md5";
```

## Remote inclusion

This section _used to_ be applicable but we no longer do remote inclusion anumore
and prefer having the server administrators have their configuration files all
based locally.

## Server information

You will now personalize the settings of this server by setting the _name_ of your
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

## Classes

We need to define some classes which are effectively names used to refer to a bunch
of settings pertaining to connection management in terms of queueing. We shall define
these for:

1. `clients`
    * Affects ordinary clients
2. `opers`
    * Affects those logged in as oper
3. `servers`
    * Affects communication for server-to-server (S2S) links

These are defined in order below:

```
/* Client class */
class clients
{
    pingfreq 90;
    maxclients 1000;
    sendq 200k;
    recvq 8000;
};

/* Oper class */
class opers
{
    pingfreq 90;
    maxclients 50;
    sendq 1M;
    recvq 8000;
};

/* Server class */
class servers
{
    pingfreq 60;
    connfreq 30;
    maxclients 100;
    sendq 20M;
};
```

## Operator information

Since this is _your_ server you will be able to configure an operator account that
can only be used to gained privileged control over _your_ server. You should set
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

Lastly we need to create an `allow {}` block. With listeners setup a socket
is bound which will accept connections at a network transport protocol level
however once connected we can decide whether to let someone proceed with
sending us IRC commands, allow blocks control this. We want to let any
user in from any IP (hence the `*@*`), we want it to apply rules from the
`clients` class defined earlier and only allow a maximum of a `100` connections
from the same IP:

```
/*
* Allow blocks
*/
allow
{
    ip *@*;
    class clients;
    maxperip 100;
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
seeing as it provides IRC services to the whole network. We also add another
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

There are some parameters that are specific to your server which are required to be set, these are shown below:

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

The following settings should be set as is. They control the modes a user will have set in certain scenarios, what
modes a channel will have when created also which channel operators (such as you) should join to.

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
2. The `spamfilter` block includes some more in-depth parameters relating to spam
    * We normally don't change any of the settings here and leave them as is

### Restrictions

One can configure the restriction of commands in a time-based manner using tje `restrict-commands` option as shown below.

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

We normally leave these settings as is.

### Connection throttling

One can configure the throttling of connections with the `connthrottle` option as shown below.

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

We normally leave these settings as is.

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
```

TODO: Do this

### Logging

One can configure where logs are to be sent to along with the verbosity of said logs:

```
log
{
    source
    {
        all;
        !debug;
    }

    destination
    {
        syslog;
    }
}
```

1. The `source` block defines the filter for logs
    * We recommend logging `all` - everything but excluding debugging prints (hence the `!debug`)
2. We almost always suggest have the `destination` be `syslog` as that is where one is most likely to look in the event of any errors

### Links

The last thing you will place in your configuration file is an inclusion for a new file that will be created when you move onto the [Linking](/linking/) section, therefore add the following:

```
/* Links */
include "links.conf";
```

## Network configuration

This section configures the server's network information:

```
set
{
    network-name            "BonoboNET";
    default-server          "rany.bnet";
    services-server         "services.bnet";
    sasl-server             "services.bnet";
    stats-server            "stats.example.org";
    help-channel            "#help";
    hiddenhost-prefix       "cloaked";
    prefix-quit             "Quit";
}
```

There is nothing here which you should change.

## Cloak keys

The cloak keys are required to be the same on all server's participating in the BonoboNET IRC network:

```
set
{
    cloak-keys
    {
        "<key1>";
        "<key2>";
        "<key3>";
    }
}
```

1. You must fill in the keys `<key1>`, `<key2>` and `<key3>`
    * You can get these keys once you have applied for a server link by following the [prior steps](/network/join)

## Nickname restrictions

Taking into account the fact that BonoboNET is an IRC network that uses IRC services there are some names which should not be available as nicknames for the average user as they are indicative of IRC services, this section places a restriction such that nobody can `/nick` as any of these.

```
/* Don't allow ChanServ to be used */
ban nick
{
    mask "*C*h*a*n*S*e*r*v*";
    reason "Reserved for Services";
}

/* Don't allow NickServ to be used */
ban nick
{
    mask "*N*i*c*k*S*e*r*v*";
    reason "Reserved for Services";
}

/* Don't allow HostServ to be used */
ban nick
{
    mask "*H*o*s*t*S*e*r*v*";
    reason "Reserved for Services";
}

/* Don't allow MemoServ to be used */
ban nick
{
    mask "*M*e*m*o*S*e*r*v*";
    reason "Reserved for Services";
}
```

---

You should next check out how one can setup monitoring of the node itself with [OpenBNET monitoring](../monitoring/)
