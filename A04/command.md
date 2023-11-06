# Kumpulan Command

- [Kumpulan Command](#kumpulan-command)
  - [1. Router Subnet](#1-router-subnet)
    - [a. Router-Dave-Server](#a-router-dave-server)
      - [Router-Dave](#router-dave)
      - [Router-Server](#router-server)
    - [b. Router-Krist-Dave](#b-router-krist-dave)
      - [Router-Krist](#router-krist)
      - [Router-Dave](#router-dave-1)
    - [c. Router-Krist-Kurt](#c-router-krist-kurt)
      - [Router-Krist](#router-krist-1)
      - [Router-Kurt](#router-kurt)
    - [d. Router-Kurt-Server](#d-router-kurt-server)
      - [Router-Kurt](#router-kurt-1)
      - [Router-Server](#router-server-1)
  - [2. Default Gateaway Subnet](#2-default-gateaway-subnet)
    - [Router-Kurt](#router-kurt-2)
      - [Subnet Kurt-1](#subnet-kurt-1)
      - [Subnet Kurt-2](#subnet-kurt-2)
    - [Router-Krist](#router-krist-2)
      - [Subnet Krist-1](#subnet-krist-1)
      - [Subnet Krist-2](#subnet-krist-2)
    - [Router-Dave](#router-dave-2)
      - [Subnet Dave-1](#subnet-dave-1)
      - [Subnet Dave-2](#subnet-dave-2)
    - [Router-Server](#router-server-2)
  - [3. Router-Server NAT](#3-router-server-nat)
  - [4. Konfigurasi DHCP](#4-konfigurasi-dhcp)
    - [a. Router-Kurt](#a-router-kurt)
      - [Kurt-1](#kurt-1)
      - [Kurt-2](#kurt-2)
    - [b. Router-Krist](#b-router-krist)
      - [Krist-1](#krist-1)
      - [Krist-2](#krist-2)
    - [a. Router-Dave](#a-router-dave)
      - [Dave-1](#dave-1)
      - [Dave-2](#dave-2)
  - [5. Konfigutasi DNS server pada DHCP](#5-konfigutasi-dns-server-pada-dhcp)
    - [Router-Kurt](#router-kurt-3)
      - [Kurt-1](#kurt-1-1)
      - [Kurt-2](#kurt-2-1)
    - [Router-Krist](#router-krist-3)
      - [Krist-1](#krist-1-1)
      - [Krist-2](#krist-2-1)
    - [Router-Dave](#router-dave-3)
      - [Dave-1](#dave-1-1)
      - [Dave-2](#dave-2-1)
  - [6. Konfigurasi OSPF](#6-konfigurasi-ospf)
    - [Router-Kurt](#router-kurt-4)
    - [Router-Krist](#router-krist-4)
    - [Router-Dave](#router-dave-4)
    - [Router-Server](#router-server-3)

## 1. Router Subnet

### a. Router-Dave-Server

#### Router-Dave

```shell
enable
config terminal
interface se0/1/1
ip address 10.243.0.225 255.255.255.252
no shut
exit
```

#### Router-Server

```shell
enable
config terminal
interface se0/1/1
ip address 10.243.0.226 255.255.255.252
no shut
exit
```

### b. Router-Krist-Dave

#### Router-Krist

```shell
enable
config terminal
interface se0/1/1
ip address 10.243.0.229 255.255.255.252
no shut
exit
```

#### Router-Dave

```shell
enable
config terminal
interface se0/1/0
ip address 10.243.0.230 255.255.255.252
no shut
exit
```

### c. Router-Krist-Kurt

#### Router-Krist

```shell
enable
config terminal
interface se0/1/0
ip address 10.243.0.233 255.255.255.252
no shut
exit
```

#### Router-Kurt

```shell
enable
config terminal
interface se0/1/0
ip address 10.243.0.234 255.255.255.252
no shut
exit
```

### d. Router-Kurt-Server

#### Router-Kurt

```shell
enable
config terminal
interface se0/1/1
ip address 10.243.0.237 255.255.255.252
no shut
exit
```

#### Router-Server

```shell
enable
config terminal
interface se0/1/0
ip address 10.243.0.238 255.255.255.252
no shut
exit
```

## 2. Default Gateaway Subnet

### Router-Kurt

#### Subnet Kurt-1

```shell
enable
config terminal
interface gig0/0/0
ip address 10.243.0.1 255.255.255.192
no shut
exit
```

#### Subnet Kurt-2

```shell
enable
config terminal
interface gig0/0/1
ip address 10.243.0.65 255.255.255.192
no shut
exit
```

### Router-Krist

#### Subnet Krist-1

```shell
enable
config terminal
interface gig0/0/0
ip address 10.243.0.129 255.255.255.224
no shut
exit
```

#### Subnet Krist-2

```shell
enable
config terminal
interface gig0/0/1
ip address 10.243.0.161 255.255.255.224
no shut
exit
```

### Router-Dave

#### Subnet Dave-1

```shell
enable
config terminal
interface gig0/0/0
ip address 10.243.0.193 255.255.255.240
no shut
exit
```

#### Subnet Dave-2

```shell
enable
config terminal
interface gig0/0/1
ip address 10.243.0.209 255.255.255.240
no shut
exit
```

### Router-Server

```shell
enable
config terminal
interface gig0/0/0
ip address 35.243.0.1 255.255.254.0
no shut
exit
```

## 3. Router-Server NAT

```shell
enable
configure terminal
ip nat pool router-server-pool 35.243.0.4 35.243.0.255 netmask 255.255.254.0
access-list 1 permit 10.243.0.0 0.0.1.255
ip nat inside source list 1 pool router-server-pool
interface se0/1/0
ip nat inside
exit
interface se0/1/1
ip nat inside
exit
interface gig0/0/0
ip nat outside
end
```

## 4. Konfigurasi DHCP

### a. Router-Kurt

#### Kurt-1

```shell
enable
configure terminal
ip dhcp pool router-kurt-1-pool
network 10.243.0.0 255.255.255.192
default-router 10.243.0.1
exit

interface gig0/0/0
ip dhcp excluded-address 10.243.0.1
exit
```

#### Kurt-2

```shell
enable
configure terminal
ip dhcp pool router-kurt-2-pool
network 10.243.0.64 255.255.255.192
default-router 10.243.0.65
exit

interface gig0/0/1
ip dhcp excluded-address 10.243.0.65
exit
```

### b. Router-Krist

#### Krist-1

```shell
enable
configure terminal
ip dhcp pool router-krist-1-pool
network 10.243.0.128 255.255.255.224
default-router 10.243.0.129
exit

interface gig0/0/0
ip dhcp excluded-address 10.243.0.129
exit
```

#### Krist-2

```shell
enable
configure terminal
ip dhcp pool router-krist-2-pool
network 10.243.0.160 255.255.255.224
default-router 10.243.0.161
exit

interface gig0/0/1
ip dhcp excluded-address 10.243.0.161
exit
```

### a. Router-Dave

#### Dave-1

```shell
enable
configure terminal
ip dhcp pool router-dave-1-pool
network 10.243.0.192 255.255.255.240
default-router 10.243.0.193
exit

interface gig0/0/0
ip dhcp excluded-address 10.243.0.193
exit
```

#### Dave-2

```shell
enable
configure terminal
ip dhcp pool router-dave-2-pool
network 10.243.0.208 255.255.255.240
default-router 10.243.0.209
exit 

interface gig0/0/1
ip dhcp excluded-address 10.243.0.209
exit
```

## 5. Konfigutasi DNS server pada DHCP

### Router-Kurt

#### Kurt-1

```shell
enable
configure terminal
ip dhcp pool router-kurt-1-pool
dns-server 35.243.0.3
end
```

#### Kurt-2

```shell
enable
configure terminal
ip dhcp pool router-kurt-2-pool
dns-server 35.243.0.3
end
```

### Router-Krist

#### Krist-1

```shell
enable
configure terminal
ip dhcp pool router-krist-1-pool
dns-server 35.243.0.3
end
```

#### Krist-2

```shell
enable
configure terminal
ip dhcp pool router-krist-2-pool
dns-server 35.243.0.3
end
```

### Router-Dave

#### Dave-1

```shell
enable
configure terminal
ip dhcp pool router-dave-1-pool
dns-server 35.243.0.3
end
```

#### Dave-2

```shell
enable
configure terminal
ip dhcp pool router-dave-2-pool
dns-server 35.243.0.3
end
```

## 6. Konfigurasi OSPF

### Router-Kurt

```shell
enable
configure terminal
router ospf 1
network 10.243.0.1 0.0.0.63 area 0
network 10.243.0.65 0.0.0.63 area 0
network 10.243.0.234 0.0.0.3 area 0
network 10.243.0.237 0.0.0.3 area 0
end
```

### Router-Krist

```shell
enable
configure terminal
router ospf 1
network 10.243.0.129 0.0.0.31 area 0
network 10.243.0.161 0.0.0.31 area 0
network 10.243.0.233 0.0.0.3 area 0
network 10.243.0.229 0.0.0.3 area 0
end
```

### Router-Dave

```shell
enable
configure terminal
router ospf 1
network 10.243.0.193 0.0.0.15 area 0
network 10.243.0.209 0.0.0.15 area 0
network 10.243.0.230 0.0.0.3 area 0
network 10.243.0.225 0.0.0.3 area 0
end
```

### Router-Server

```shell
enable
configure terminal
router ospf 1
network 35.243.0.1 0.0.1.255 area 0
network 10.243.0.238 0.0.0.3 area 0
network 10.243.0.226 0.0.0.3 area 0
end
```
