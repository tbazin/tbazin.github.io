# On Google Cloud Engine

## Firewall rules

Source: https://cloud.google.com/vpc/docs/firewalls#default_firewall_rules

Egress rules on instances meant for `deny` type of rules, since all outbound
traffic is enabled by default on GCE instances:

> All networks, whether a project's `default` network or a manually created
network, have the following implied rules. These rules cannot be changed or
deleted, but rules with a higher priority can override them.
>
>
* A default "allow egress" rule.
    Allows all egress connections. Rule has a priority of `65535`.
* A default "deny ingress" rule.
    Deny all ingress connection. Rule has a priority of `65535`

Allowing all egress traffic by default is apparently common practice on
servers, under the assumption that the code running on the server is - to
itself - trustable.

## Drives and instances

*Information given by Nicola Montecchio.*

* Using an SSD over a hard drive does **not** significantly increase costs
* GPU instances cost a lot, _even when idle_, so **turn them off
    as often as possible**
* On throughput:
    - **Much** higher throughput on _large_ drives, thus it is more
        efficient to create the instance with a _small internal drive_ **AND**
        create a (_possibly shared_) large SSD (e.g. 400Gb) and mount it on the
        instance.
    - in that case the drive has to be created as a blank drive, then
        be attached to the instance. see the procedure in the references for
        mounting and formatting the drive
    - the created disk can be used _in read-only mode_ by several instances
        simultaneously
    - also has the benefit of allowing to shutdown the instance and still access
        the files on the drive: an idle drive does not cost much

### Mounting drive

Code available for formatting on this page: https://cloud.google.com/compute/docs/disks/add-persistent-disk

Note: The code given on this Google page omits the creation of the `/mnt/disks`
folder, which could result in bugs regarding the file permissions.

Code to properly create this folder and set the permissions:
```
$ sudo mkdir /mnt/disks
$ sudo chmod -R a+rx /mnt/disks  # `+x` necessary to enter and perform ls on folder
```

### References:

* On disk performance:
    https://cloud.google.com/compute/docs/disks/performance
* Adding and formatting drives:
    https://cloud.google.com/compute/docs/disks/add-persistent-disk
