## KOMPRISE FOR KVM


### Instructions

Untar the director and Observer files

```
tar -xvf director.tar.gz
tar -xvf observer.tar.gz
```

You will need to modify the network configuration of both the observer and the director to connect the appropriate networks. In the example below, we connect to the VM to the default network. You can check the avaiable networks domains by issuing the command 'virsh net-list' as shown below

```
$virsh net-list
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     yes           yes
```

Modify the director and observer xml to point to appropriate network domain in the interface settings

```
$ vi komprise-centos-20170526.xml

<interface type='network'>
      <source network='default'/>
      <model type='virtio'/>
    </interface>
```

Modify the director and observer xml to point to the path to disk

```
$ cd director
$ ls -al
-rw-r--r--  1 root   root   7314931712 Jun 26 15:38 komprise-centos-2.4-on-premise-sda
-rw-r--r--  1 dasher dasher       1324 Jun 26 15:36 komprise-centos-2.4-on-premise.xml

<devices>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2' cache='none'/>
      <source file='/home/dasher/director/komprise-centos-2.4-on-premise-sda'/>
      <target dev='vda' bus='virtio'/>
    </disk>
```


### For the Observers ONLY

Copy the extracted folder for the observers to create clones of the base image

```
$ cp -r observer/ observer-1
$ cp -r observer/ observer-2
$ cp -r observer/ observer-3
```

modify the name of the observer to reflect the clone number

```
<domain type='kvm'>
  <!-- generated by virt-v2v 1.32.7rhel=7,release=3.el7.centos.2,libvirt -->
  <name>komprise-observer-1</name>
  <memory unit='KiB'>4194304</memory>
  <currentMemory unit='KiB'>4194304</currentMemory>
  <vcpu>1</vcpu>
```

Import the director and observer virtual machines into libvirt my issuing the comand ```virsh open <Path-to-xml-file.xml>```

```
$ virsh open komprise-centos-2.4-on-premise.xml
```
