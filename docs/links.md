Link directory
==============

This is the link-block directory of all people that have open peering policies.

# Africa

All servers located within the African continent.

## South Africa

All servers located within the Republic of South Africa.

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
        options { tls; autoconnect; }
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
        options { tls; autoconnect; }
    }
    password "1OznndE6OIsB2BeYqqvQ58sVhIneN5ap+zcBRhP22gk=" { spkifp; }
    class servers;
}
```

Reachable (replace `braveheart.bnet` with either) via:

1. `300:7232:2b0e:d6e9:216:3eff:fe3c:c82b` (Yggdrasil)

# Middle East

All servers located within the Middle East.

## Lebanon

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
        options { tls; autoconnect; }
    }
    password "6HHBeB3YYKBZ+LIH5mhGfynyzT80Wb7MiVDgKjPbh8A=" { spkifp; }
    class servers;
}
```

Reachable (replace `sparrow.bnet` with either) via:

1. `fdfb:1a20:a9bf::1` (CRXN)
2. `204:208c:fb24:4d76:6162:6b44:9418:2897` (Yggdrasil)