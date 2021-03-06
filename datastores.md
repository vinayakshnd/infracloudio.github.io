---
title: Datastore & backend layout
---

## Datastore & backend layout

Docker volumes in vSphere are backed by VMDK files. These VMDKs could be backed by any storage backend. For example the backend storage could be one of NFS, SAN, VSAN, vVol. The VMDKs reside on a central location and can be reqested by any container running on any host. Ability to attach volumes from anywhere in cluster coupled with a clustering technology such as Swarm enables a highly available architecture.

## Multiple datastores Support

By default the volume is created in the default datastore for the VM. But there are use cases where you want to create and consume volumes from different datastores for specific performance or functionality. The vDVS allows you to create a volume on any of the datastores available.

## Default datastore
By default when you just specify name of volume - it is created on default datastore. 

```
$docker volume create --driver=vsphere --name=defaultDsVolume -o size=10gb

defaultDsVolume
```
We can check the volume is created in default datastore

```
$esx ls /vmfs/volumes/vsanDatastore/dockvols

defaultDsVolume.vmdk
```

## Custom Datastore

To choose a datastore other than default one, you have to specify name and append the datastore name with a ```@```

```
$docker volume create --driver=vsphere --name=ds1volume@datastore1 -o size=10gb

ds1volume@datastore1
``` 
We can check that volume is created in the datastore1

```
$esx ls /vmfs/volumes/datastore1/dockvols

ds1volume.vmdk
```

## Listing Volumes

When you list the volumes, you can clearly distinguish the volumes on default datastore vs. the volumes in other datastores

```
$ docker volume ls
DRIVER 			VOLUME NAME
vsphere			defaultDsVolume
vsphere			ds1volume@datastore1
photon			ds2volume@datastore2
```
