Linking
=======

We will now begin the process of linking your server with someone else. You firstly need to find
a node operator willing to peer with you.

## Link blocks

### Your link block

You will need to generate a link block which you will provide to your node operator, this
can be done via running:

```bash
unrealircd genblock
```

### Adding the link

You wil now need to add your **peer's** link block to your configuration.

---

## Tidying up

Once you have both swapped link blocks, reload your unrealircd daemon with the following
command:

```bash
unrealircd reload
```