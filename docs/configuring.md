Configuring
===========

This chapter will help you configure your _unrealircd_ daemon for linking into the BonoboNET IRC network.

## Initial configuration

You will need to contact an admin of BNET:

1. `deavmi` (deavmi@redxen.eu)
2. `rany` (ranycrxn@riseup.net)

This will be so you can acquire the following:

1. `sid`
	* This is your server ID which must be unique within the IRC network which is why you need to contact a network leader

### Includes

The _Includes_ section is just to bring in some needed configuration files that make sense to be seperated (for reasons of 
modularity)

```
include "modules.default.conf";

include "help/help.conf";
include "badwords.conf";
//include "spamfilter.conf";
include "operclass.default.conf";
```

### Server information

You must now add the following block to your config:

```
me {
	name "<name>";
	info "<description>";
	sid "<sid>";
}
```

Please fill in the following:

1. `sid "<sid>"`
	* Where `<sid>` is the server Id you got from one of the network leader's earlier
2. `info "<description>"`
	* Where `<description>` can be a short little description of your server
	* Example: `This is my London-based IRC node`
3. `name "<name>"`
	* Where `<name>` is the name of your server
	* This should be something that ends in `.bnet`
	* Example: `myCoolServer.bnet`
	* Save this for later, as you will need it when linking with others

### Administrator information

You must now add the following block to your config:

**TODO: Fgigure out what to do with this configurarion*

```
admin {
	"adam smith";
	"adam smith";
	"capitalismispoggers@cia.gov";
}
```

### Classes

Classes allow one to affect certain attributes of certain types of connections. For example, we may
want to change the amount of servers that can link to us from a certain IP address, this would be
manipulated in the `servers` class. However, we may also want to manioulate, perhaps, how many
clients may connect to the server, this would be manipulated in the `clients` class. We pretty much
go with thr default here for both _servers_ and _clients_.

#### Client and Server classes

Configure the clients and servers classes.

```
/* Client class with good defaults */
class clients {
	pingfreq 90;
	maxclients 1000;
	sendq 200k;
	recvq 8000;
}

/* Server class with good defaults */
class servers {
	pingfreq 60;
	connfreq 15; /* try to connect every 15 seconds */
	maxclients 10; /* max servers */
	sendq 20M;
}
```

#### Operator class

We also give _operators_ higher priveleges (within their own class):

```
/* Special class for IRCOps with higher limits */
class opers {
	pingfreq 90;
	maxclients 50;
	sendq 1M;
	recvq 8000;
}
```

---

### Allow blocks

The _allow blocks_ section is where we configure, depending on the _source IP address_, which class
should apply to those connections and also how many connections per each connection (IP address) are
allowed.

```
allow {
	ip *@*;
	class clients;
	maxperip 30;
}
```

You may change the following:

1. `maxperip <arg>`
	* You ay set `<arg>` to whatever number you want to limit how many connections per each IP which matches the mask may connect

---

### Operator configuration

You may now conifgure the operator for this server:

```
oper <name> {
	class opers;
	mask *@*;
	password "<password>";
	operclass netadmin;
	swhois "<desription>";
	vhost netadmin.bnet;
}
```

Please configure the following:

1. `oper <name>`
	* Here you may set `<name>` to the username (_nick_) to the nickname for the operator
2. TODO: Mask
3. `password "<password>"`
	* Set the `<password>` to the password that will be used to login as operator `<name>`
4. `swhois "<description>"`
	* Set the `<description>` here to what will be your description

---

### Sockets

Here we configure the addresses for the several sockets that will be listening for client connections
and server connections.

#### Clients

Here we configure the client connections. We will allow connections (palintext) on port `6667` and TLS
connections (secured connections) on port `6697`. Both will listen on all IP addresses.

```
listen {
	ip *;
	port 6667;
}

listen {
	ip *;
	port 6697;
	options { tls; }
}
```

#### Servers

Here we configure the server connections. We will allow servers to only connect on port `6900` and using TLS
only.

```
listen {
	ip *;
	port 6900;
	options { tls; serversonly; }
}
```

---

## Network

We now need to configure some network parameters.

```
/* Network configuration */
set {
	network-name 		"BonoboNET";
	default-server 		"reddawn648.bnet";
	services-server 	"services.bnet";
	//stats-server 		"";
	help-channel 		"#Help";
	hiddenhost-prefix	"Clk";
	prefix-quit 		"Quit";

	/* Cloak keys should be the same at all servers on the network.
	 * They are used for generating masked hosts and should be kept secret.
	 * The keys should be 3 random strings of 50-100 characters
	 * and must consist of lowcase (a-z), upcase (A-Z) and digits (0-9).
	 * HINT: On *NIX, you can run './unrealircd gencloak' in your shell to let
	 *       UnrealIRCd generate 3 random strings for you.
	 */
	cloak-keys {
		"<cloak1>";
		"<cloak2>";
		"<cloak3>";
	}
}
```

You need to update the following parts:

1. `<cloak1>`
	* Set `<cloak1>` to the first cloak key
2. `<cloak2>`
	* Set `<cloak2>` to the first cloak key
3. `<cloak3>`
	* Set `<cloak3>` to the first cloak key

You would have obtained these from one of the network leaders.

---

## Connection management

We may want to throttle users who reconnect to fast and so forth. Unrealircd provides a nice mechanism for this based on
the user's reputation.

```
/*
 * The following will configure connection throttling of "unknown users".
 *
 * When UnrealIRCd detects a high number of users connecting from IP addresses
 * that have not been seen before, then connections from new IP's are rejected
 * above the set rate. For example at 10:60 only 10 users per minute can connect
 * that have not been seen before. Known IP addresses can always get in,
 * regardless of the set rate. Same for users who login using SASL.
 *
 * See also https://www.unrealircd.org/docs/Connthrottle for details.
 * Or just keep reading the default configuration settings below:
 */

set {
	connthrottle {
		/* First we must configure what we call "known users".
		 * By default these are users on IP addresses that have
		 * a score of 24 or higher. A score of 24 means that the
		 * IP was connected to this network for at least 2 hours
		 * in the past month (or minimum 1 hour if registered).
		 * The sasl-bypass option is another setting. It means
		 * that users who authenticate to services via SASL
		 * are considered known users as well.
		 * Users in the "known-users" group (either by reputation
		 * or by SASL) are always allowed in by this module.
		 */
		known-users {
			minimum-reputation-score 24;
			sasl-bypass yes;
		}

		/* New users are all users that do not belong in the
		 * known-users group. They are considered "new" and in
		 * case of a high number of such new users connecting
		 * they are subject to connection rate limiting.
		 * By default the rate is 20 new local users per minute
		 * and 30 new global users per minute.
		 */
		new-users {
			local-throttle 20:60;
			global-throttle 30:60;
		}

		/* This configures when this module will NOT be active.
		 * The default settings will disable the module when:
		 * - The reputation module has been running for less than
		 *   a week. If running less than 1 week then there is
		 *   insufficient data to consider who is a "known user".
		 * - The server has just been booted up (first 3 minutes).
		 */
		disabled-when {
			reputation-gathering 1w;
			start-delay 3m;
		}
	}
}
```

## Server-specific settings

Please add the following block for server-specific tweaks:

```
/* Server specific configuration */
set {
	kline-address "<email>"; /* e-mail or URL shown when a user is banned */
	modes-on-connect "+iw"; /* when users connect, they will get these user modes */
	modes-on-oper "+xws"; /* when someone becomes IRCOp they'll get these modes */
	modes-on-join "+nt"; /* default channel modes when a new channel is created */
	oper-auto-join "#opers"; /* IRCOps are auto-joined to this channel */
	options {
		hide-ulines; /* hide U-lines in /MAP and /LINKS */
		show-connect-info; /* show "looking up your hostname" messages on connect */
	}

	maxchannelsperuser 255; /* maximum number of channels a user may /JOIN */

	/* The minimum time a user must be connected before being allowed to
	 * use a QUIT message. This will hopefully help stop spam.
	 */
	anti-spam-quit-message-time 10s;

	/* Or simply set a static quit, meaning any /QUIT reason is ignored */
	/* static-quit "Client quit";	*/

	/* static-part does the same for /PART */
	/* static-part yes; */

	/* Flood protection */
	anti-flood {
		nick-flood 3:60;	/* 3 nick changes per 60 seconds (the default) */
		connect-flood 3:60; /* 3 connection attempts per 60 seconds (the default) */
		away-flood 4:120;	/* 4 times per 2 minutes you may use /AWAY (default) */
	}

	/* Settings for spam filter */
	spamfilter {
		ban-time 1d; /* default duration of a *LINE ban set by spamfilter */
		ban-reason "Spam/Advertising"; /* default reason */
		virus-help-channel "#help"; /* channel to use for 'viruschan' action */
		/* except "#help"; channel to exempt from Spamfilter */
	}

	/* Restrict certain commands.
	 * See https://www.unrealircd.org/docs/Set_block#set::restrict-commands
	 */
	restrict-commands {
		list {
			connect-delay 60;
			exempt-identified yes;
			exempt-reputation-score 24;
		}
		invite {
			connect-delay 120;
			exempt-identified yes;
			exempt-reputation-score 24;
		}
		/* In addition to the ability to restrict any command,
		 * such as shown above. There are also 4 special types
		 * that you can restrict. These are "private-message",
		 * "private-notice", "channel-message" and "channel-notice".
		 * They are commented out (disabled) in this example:
		 */
		//private-message {
		//	connect-delay 10;
		//}
		//private-notice {
		//	connect-delay 10;
		//}
	}
}
```

Please set the following fields:

1. `kline-address "<email>";`
	* Set `<email>` to your e-mail address.
	* This will be used in k-line information messages to those who have been k-lined

---

## Services

We need to add some exceptional rules for the IRC network services run by rany:

```
/* U-lines give other servers (even) more power/commands.
 * If you use services you must add them here.
 * NEVER put the name of an UnrealIRCd server here!!!
 */
ulines {
	services.bnet;
}

/* With "aliases" you can create an alias like /SOMETHING to send a message to
 * some user or bot. They are usually used for services.
 *
 * We have a number of pre-set alias files, check out the alias/ directory.
 * As an example, here we include all aliases used for anope services.
 */
include "aliases/atheme.conf";

/* Ban nick names so they cannot be used by regular users */
ban nick {
	mask "*C*h*a*n*S*e*r*v*";
	reason "Reserved for Services";
}
```

---

## Misc.

### Logging

You may want to enable sane defaults for the logging.

```
/* This is a good default, it logs almost everything */
log "ircd.log" {
	flags {
		oper;
		connects;
		server-connects;
		kills;
		errors;
		sadmin-commands;
		chg-commands;
		oper-override;
		tkl;
		spamfilter;
	}
}
```

---

### Remaining things

Just a few more things to add:

```
/* Here you can add a password for the IRCOp-only /DIE and /RESTART commands.
 * This is mainly meant to provide a little protection against accidental
 * restarts and server kills.
 */
drpass {
	restart "restart";
	die "die";
}
```

---

## WebIRC (TODO: Move to own section)

If you wish to configure your server for access via a web-based IRC client then you will want to
add the following blocks to your configuration file.

```
// webirc
webirc {
	mask 127.*.*.*;
	password "penisssssssIsCooooooolChangeThisLOL";
};

except ban {
        mask 127.*.*.*;
        type { connect-flood; }; //blacklist; }; //unknown-data-flood; };
};
```