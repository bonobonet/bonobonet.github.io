Monitoring
==========

You can setup monitoring of your node with OpenBNET which is our custom written monitoring software for thew `unrealircd` daemon.

## Configuring JSON-RPC

The way in which OpenBNET communicates with the `unrealircd` daemon  is via a JSON-RPC HTTP socket, however this is not enabled by default in unrealircd, therefore one must enable it by including this piece of configuration in a new file named `openbnet.conf`:

```
/* Load the JSON-RPC modules */
include "rpc.modules.default.conf";

/* Setup a block to listen for RPC calls */
listen
{
    ip 127.0.0.1;
    port 8001;
    options { rpc; }
}

/* Set authentication details */
rpc-user apiuser
{
    match { ip *; }
    password "password";
}
```

Once this file has been created you can place the following at the end of your `unrealircd.conf` file:

```
/* Support for OpenBNET NG */
include "openbnet.conf";
```

Now save this file and reload your unrealircd daemon