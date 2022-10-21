---
title: State of the Network 2022
author: Tristan B. Kildaire
description: Update on the state of the BonoboNET IRC network for 2022
tags: [stateofthenetwork, sotn, 2022, general]
---

State of the Network 2022
=========================

I hope to write these updates very often and not just every single year. It aims to recap all the accomplishments of the past year along wit some notable events as well!

## Looking back

### Stability

If we go back all the way to when I, [`deavmi`](../../people/#rany), founded BNET way back when in 2020 round about April I believe, we were in a _way_ different position to now than we were then. This was a period of major instability for the network. We were still running [ngircd](https://ngircd.barton.de/) which still has a good place in my heart but we needed space to grow and some had suggested [unrealircd](https://www.unrealircd.org/) which would be the server software we are using today.

Back then we also had rather unstable peers. I recall a user by the name of `kaotisk`, now old users of BNET will recall that this was a Greek man that was going through some hard times from what I last remember. He, however, had the bright idea to run a node, that we _gladly accepted_ to peer with have you, from his laptop. Little did we know this laptop was not a make shift fixed machine but a machine he would _"DJay with"_ and _"Take to the beach"_. Needless to say, we now only peer with people who will have a relatively stable network or atleast will not be experimenting with their host by running `routef` whenever it suites them.

Along with this, the internet connection for `lockdown.bnet` has been upgraded to 100mbit doown and 50mbit up therefore improving stability from the previous 10mbit line which was nowhere near that since it is a residential network. CRXN also benefitted form this upgrade which means that peering for nodes such as `lockdown.bnet` and `reddawn648.bnet` (which both peer over CRXN) is improved.

### IRC services

We never had IRC services back in the original BonoboNET IRC network which ran on NGIRCD, however I believe near the end of NGIRCD deployment was when [`rany`](../../people/#rany) joined us and started experimenting with running IRC services via [Atheme](https://atheme.github.io/atheme.html), we quickly decided that using unrealircd would be better.

Having services has greatly improved the professionalism of the network and given the operators the ability to enforce the policies (the [Rules](../../rules)) that we are mandated to.

### Lokinet sucesses

The deployment of Lokinet for both server peering and client access has been a fun experience especially seeing that it is most likely one of the newest networks out there in this space (Yggdrasil, CJDNS etc.). The former however (client access) has lead to a rather large number of new users, many returning whilst others stopped by. The spike is most likely due to `gayballs.bnet`'s server attracting a lot of new users due to this being the server operated by the lead developer of Lokinet, secondly because the main Lokinet developer channel now seems to be hosted on BonoboNET as `#lokinet`. This is obviously a great win for the network and we loom forward to welcoming many more users to the network over Lokinet!

### Peering

We have also updated the peering system such that people can more easily peer. [`rany`](../../people/#rany) has setup a base configuration hosted on Yggdrasil that node operators can pull from (and cache locally) such that the common configuration never needs to be setup again, however along with this peering only requires adding a peer to your side and to the central config (then all other machines can potentially peer with you if you choose to connect with them).

### New website

The new website, which is finally on [Github](https://github.com/bonobonet/website) and open to pull requests, is nearing completion and finally looks professional. I think a lot of this has helped new users easily find us. In terms of I2P I recall a user mentioning _"I stumbled across it whilst browsing I2P"_ which probably means it was easy enough to navigate - although not to forget that we do have a little advertisement block on the i2pd homepage at `i2pd.i2p`.

### Botty

We cannot forget our great bot written by [`rany`](../../people/#rany), [**botty**](../../botty/)!

## Future

Some things definately need to be worked on, sucha s making sure OpenBNET is deployed to as many nodes as possible such that we can get a greater view of each node operating in the network.

Amongst other things more features to our in-house IRC bot written by [`rany`](../../people/#rany), [_botty_](../../botty/)
