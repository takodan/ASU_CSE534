```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: # yor ip [10.0.0.10/8]
      gateway4: 10.0.0.100
      nameserveres:
        addresses: [8.8.8.8, 8.8.4.4]
```