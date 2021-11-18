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