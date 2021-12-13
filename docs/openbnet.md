![](open_bnet_banner.png)

OpenBNET
=======

**OpenBNET** is a lightweight daemon that runs a web server that provides an API endpoint for getting information about the global BonoboNET
IRC network as well as providing a cute little status page regarding this information.

![](openbnet_home.png)

## Configuring unrealircd

unrealircd will need to be configured for this piece of software to work. This is because OpenBNET requires a UNIX domain socket to
connect to over which the IRC daemon provides the needed global state information.


### Installing the module

We must first install the module such that the shared object file `.so` (the plugin) becomes available for the IRC daemon to dynamically
link in at runtime whenever it appears in the configuration file (seen in the next step):

To do this you will need to be in your unrealircd source directory where the `unrealircd` binary is present, then run:

```
./unrealircd module install third/wwwstats
```

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

This is taken directly from the [OpenBNET GitHub repository](https://github.com/bonobonet/OpenBNET).

### Setting up

You will need the following and can install them easily:

1. `python3`
2. `flask`
3. `pip`

```
apt install python3 python3-pip
pip3 install flask
```

You will need to configure the `third/wwwstats` module as well, information on doing so can be found [here](http://deavmi.assigned.network/projects/bonobonet/openbnet/).

### Usage

Firstly grab all the files in this repository, then:

```
chmod +x obnet.py
```

The next thing to do will be to set the following environment variables:

* `OPENBNET_BIND_ADDR`
  *  The addresses to listen on (for the web server)
* `OPENBNET_BIND_PORT`
  * The port to listen on (for the web server)
* `UNREAL_SOCKET_PATH`
  * This is the path to the unrealircd `third/wwwstats` UNIX domain socket

You can then run it like such:

```
OPENBNET_BIND_ADDR="::" OPENBNET_BIND_PORT=8081 UNREAL_SOCKET_PATH=/tmp/openbnet.sock ./obnet.py
```

#### Systemd-unit

There is an example systemd unit file included in the repository as `openbnet.service`

### Custom branding

You can adjust the branding in `obnet.py` by taking a look at the following:

```python
# Network information
NET_INFO = {
    "networkName": "OpenBonobo",
    "description": "Network statistics for the BonoboNET network",
    "networkLogo": "open_bnet_banner.png",
}
```