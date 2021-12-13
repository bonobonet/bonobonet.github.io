OpenBNET
=======

**OpenBNET** is a lightweight daemon that runs a web server that provides an API endpoint for getting information about the global BonoboNET
IRC network as well as providing a cute little status page regarding this information.

TODO: Setup
TODO: grab from github

## Configuring unrealircd

unrealircd will need to be configured for this piece of software to work. This is because OpenBNET requires a UNIX domain socket to
connect to over which the IRC daemon provides the needed global state information.


### Installing the module

We must first install the module such that the shared object file `.so` (the plugin) becomes available for the IRC daemon to dynamically
link in at runtime whenever it appears in the configuration file (seen in the next step):

### Configuring the module

We must now configure both unrealircd to know that the module is available, this is done with the following line:

```
/* Load the module into unreal */
loadmodule "third/wwwstats"; 
```

And then we must configure the module itself:

```
/* 
* Support for OpenBNET 
*/ 
wwwstats { 
        socket-path "/tmp/openbnet.sock";
};
```

It is important that the `loadmodule` call is done first before using the `wwwstats` block.

## Configuring OpenBNET

1. Grab from git
2. maybe docker image?
3. systemd unit if not docker
4. environment variables settings for network parameters