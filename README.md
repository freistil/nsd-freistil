# nsd cookbook

A Chef cookbook to install nsd master and slaves along with initial zone configuration.

## Recipes

### nsd::master

Install nsd master and configure zones.

### nsd::slave

Install nsd slave and configure zones.

## Configuration

Better to explain it by an extract from node attributes:

```json
    ...
    "nsd": {
      "master": {
        "fqdn": "ns1.example.com",
        "ip_address": "1.1.1.1",
        "contact": "hostmaster@example.com"
      },
      "slaves": {
        "ns2.example.com": "2.2.2.2",
        "ns3.example.com": "3.3.3.3"
      },
      "zones": [
        "example.com",
        "test.com"
      ]
    },
    ...
```

This configuration describes one master (`ns1.example.com`) and two slave servers (`ns2.example.com` and `ns3.example.com`), all of which host two zones - `example.com` itself and `test.com`. SOA records will contain `hostmaster@example.com` as an email address.

### AXFR keys

A zone is modified only on a master server. Then the changes are transferred to slave servers. It is presupposed that each slave server has its own secret key which is stored in an encrypted data bag named `nsd`:

```json
{
    "id": "production",
    "keys": {
        "ns2.example.com": "7DzUnLpx9H...",
        "ns3.example.com": "1ADEn1fqOo..."
    }
}
```

A key may be generated with this command:

```sh
$ dd if=/dev/random count=1 bs=32 2> /dev/null | base64
```

## Limitations
At this juncture, this cookbook only supports IPv4. Also, no firewall rules are set up.

## License
MIT @ [Alexander Pyatkin](https://github.com/aspyatkin)
