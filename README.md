# openstack-netvirt
Openstack and Opendaylight NetVirt Integration Stuff

Network and Subnet
------------------
```
openstack network create net01 --provider-network-type vxlan --provider-segment 100
openstack network create net02 --provider-network-type vxlan --provider-segment 200

openstack subnet create --network net01 --subnet-range 10.15.1.0/24 subnet01
openstack subnet create --network net02 --subnet-range 10.16.2.0/24 subnet02

```

Router and floating IP
----------------------

```
openstack network create pubnet --external --provider-network-type flat --provider-physical-network public

openstack subnet create --network pubnet --subnet-range 192.168.100.0/24 --allocation-pool start=192.168.100.200,end=192.168.100.250 --gateway 192.168.100.1 --no-dhcp pubsubnet

openstack router create extvr
openstack router add subnet extvr subnet01
openstack router add subnet extvr subnet02
openstack router set --external-gateway pubnet extvr

openstack floating ip create --port ba993162-ee1c-4db2-8979-4868410ab66e pubnet

$ openstack server list
+--------------------------------------+------+--------+----------------------------------+--------------------------+---------+
| ID                                   | Name | Status | Networks                         | Image                    | Flavor  |
+--------------------------------------+------+--------+----------------------------------+--------------------------+---------+
| 8a58dbb1-c926-4e6d-b0ca-190c6e1f0ff1 | vm4  | ACTIVE | net02=10.16.2.12                 | cirros-0.3.5-x86_64-disk | m1.tiny |
| 82122a0b-a518-4d4e-b820-df96d7c94c41 | vm3  | ACTIVE | net02=10.16.2.8                  | cirros-0.3.5-x86_64-disk | m1.tiny |
| 2f0d0fd0-a2c3-45c6-bc5c-d1bb740bbe20 | vm2  | ACTIVE | net01=10.15.1.5                  | cirros-0.3.5-x86_64-disk | m1.tiny |
| 3f1e15fe-9aab-4093-a864-93aa9cbc9033 | vm1  | ACTIVE | net01=10.15.1.3, 192.168.100.215 | cirros-0.3.5-x86_64-disk | m1.tiny |
+--------------------------------------+------+--------+----------------------------------+--------------------------+---------+
stack@control:~/devstack$
```

Integration Topo
----------------
![Interation Topo](https://github.com/yyang13/openstack-netvirt/raw/master/netvirt-topo.jpg)
