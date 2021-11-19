Link directory
==============

This is the link-block directory of all people that have open peering policies. Remember to sort out who will `autoconnect` to who. It's not recommended
that **both** parties `autoconnect`, only one should.

# Africa

All servers located within the African continent.

## South Africa

All servers located within the Republic of South Africa.

### lockdown.bnet

Link block:

```
link lockdown.bnet
{
        incoming {
                mask *;
        }
        outgoing {
                hostname lockdown.bnet;
                port 6901;
                options { tls; }
        }
        password "J8ZR1mNEal0yHYfoB4hEiE2Y3xrOVlYJrcXIt4M90rM=" { spkifp; }
        class servers;
}
```

Reachable (replace `lockdown.bnet` with either) via:

1. `fd08:8441:e254::2` (CRXN)
2. `200:ad55:3db7:b8cf:3aa8:664b:bca3:bd81` (Yggdrasil)
3. `fc54:203d:baa7:5c08:152a:317c:4a2e:8c13` (CJDNS)
4. `2a04:5b81:2010::10` (Clearnet)

### reddawn.bnet

Link block:

```
link reddawn648.bnet {
    incoming {
        mask *;
    }
    outgoing {
        hostname reddawn648.bnet;
        port 6901;
        options { tls; }
    }
    password "rde+PDmB9C24x7gFNRI5JLUlxSfSnMiefDUgL0aEkpY=" { spkifp; }
    class servers;
}
```

Reachable (replace `reddawn648.bnet` with either) via:

1. `fd96:21ef:a9ba::1` (CRXN)
2. `204:fb3e:d9e:9f20:7af1:27ab:6aed:df32` (Yggdrasil)
3. `2a04:5b81:2011::2` (Clearnet)

### columbus.bnet

Link block:

```
link columbus.bnet {
    incoming {
        mask *;
    }
    outgoing {
        hostname columbus.bnet;
        port 6900;
        options { tls; }
    }
    password "h0apxxjND4gvaRmWSfVy/MfY9LcRlqEpiDpksijEO70=" { spkifp; }
    class servers;
}
```

Reachable (replace `columbus.bnet` with either) via:

1. `fd06:3184:2093:0:eff0:c986:ee2a:61b7` (CRXN)
2. `200:e1ad:d2cf:7580:3d78:fce:4ff4:b618` (Yggdrasil)

# Europe

All servers located within the European continent.

## UK (Scotland)

### braveheart.bnet

Link block:

```
link braveheart.bnet {
    incoming {
        mask *;
    }
    outgoing {
        hostname braveheart.bnet;
        port 6900;
        options { tls; }
    }
    password "1OznndE6OIsB2BeYqqvQ58sVhIneN5ap+zcBRhP22gk=" { spkifp; }
    class servers;
}
```

Reachable (replace `braveheart.bnet` with either) via:

1. `300:7232:2b0e:d6e9:216:3eff:fe3c:c82b` (Yggdrasil)

## Germany

### sparrow.bnet

Link block:

```
link sparrow.bnet {
    incoming {
        mask *;
    }
    outgoing {
        hostname sparrow.bnet;
        port 6900;
        options { tls; }
    }
    password "6HHBeB3YYKBZ+LIH5mhGfynyzT80Wb7MiVDgKjPbh8A=" { spkifp; }
    class servers;
}
```

Reachable (replace `sparrow.bnet` with either) via:

1. `fdfb:1a20:a9bf::1` (CRXN)
2. `204:208c:fb24:4d76:6162:6b44:9418:2897` (Yggdrasil)