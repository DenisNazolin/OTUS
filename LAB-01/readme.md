### Схема CLOS
### Цели
1. Собрать топологию CLOS.
2. Распределить адресное пространство для Underlay сети.



### Реализовать схему 

### Таблица адресов
| Device        | Interface | IP Address   | Mask |
| ------------- |:----------| :------------| :----|
| pd01-clf-001  | Ethernet1 | 172.16.1.0   | /31  |
|               | Ethernet2 | 172.16.1.2   | /31  |
|               | Ethernet3 | 172.16.1.4   | /31  |
|               | Ethernet4 | 172.16.1.6   | /31  |
|               | Ethernet5 | 172.16.1.8   | /31  |
|               | Ethernet6 | 172.16.1.10  | /31  |
|               | Ethernet7 | 172.16.1.12  | /31  |
|               | Ethernet8 | 172.16.1.14  | /31  |
| pd01-clf-002  | Ethernet1 | 172.16.2.4   | /31  |
|               | Ethernet2 | 172.16.2.6   | /31  |
|               | Ethernet3 | 172.16.2.0   | /31  |
|               | Ethernet4 | 172.16.2.2   | /31  |
|               | Ethernet5 | 172.16.2.8   | /31  |
|               | Ethernet6 | 172.16.2.10  | /31  |
|               | Ethernet7 | 172.16.2.12  | /31  |
|               | Ethernet8 | 172.16.2.14  | /31  |
|               | Lo0       | 10.0.2.1     | /32  |
| pd01-clf-003  | Ethernet1 | 172.16.3.4   | /31  |
|               | Ethernet2 | 172.16.3.6   | /31  |
|               | Ethernet3 | 172.16.3.8   | /31  |
|               | Ethernet4 | 172.16.3.10  | /31  |
|               | Ethernet5 | 172.16.3.0   | /31  |
|               | Ethernet6 | 172.16.3.2   | /31  |
|               | Ethernet7 | 172.16.3.12  | /31  |
|               | Ethernet8 | 172.16.3.14  | /31  |
|               | Lo0       | 10.0.3.1     | /32  |
| pd01-elf-001  | Ethernet1 | 172.16.1.1   | /31  |
|               | Ethernet2 | 172.16.1.3   | /31  |
|               | Ethernet3 | 172.16.2.1   | /31  |
|               | Ethernet4 | 172.16.2.3   | /31  |
|               | Ethernet5 | 172.16.3.1   | /31  |
|               | Ethernet6 | 172.16.3.3   | /31  |
|               | Lo0       | 10.1.1.1     | /32  |
| pd01-elf-001  | Ethernet1 | 172.16.1.1   | /31  |
|               | Ethernet2 | 172.16.1.3   | /31  |
|               | Ethernet3 | 172.16.2.1   | /31  |
|               | Ethernet4 | 172.16.2.3   | /31  |
|               | Ethernet5 | 172.16.3.1   | /31  |
|               | Ethernet6 | 172.16.3.3   | /31  |
|               | Lo0       | 10.1.1.1     | /32  |
| pd01-elf-002  | Ethernet1 | 172.16.2.5   | /31  |
|               | Ethernet2 | 172.16.2.7   | /31  |
|               | Ethernet3 | 172.16.1.5   | /31  |
|               | Ethernet4 | 172.16.1.7   | /31  |
|               | Ethernet5 | 172.16.3.5   | /31  |
|               | Ethernet6 | 172.16.3.7   | /31  |
|               | Lo0       | 10.2.2.2     | /32  |
| pd01-elf-003  | Ethernet1 | 172.16.2.9   | /31  |
|               | Ethernet2 | 172.16.2.11  | /31  |
|               | Ethernet3 | 172.16.1.9   | /31  |
|               | Ethernet4 | 172.16.1.11  | /31  |
|               | Ethernet5 | 172.16.3.9   | /31  |
|               | Ethernet6 | 172.16.3.11  | /31  |
|               | Lo0       | 10.3.3.3     | /32  |
| pd01-elf-004  | Ethernet1 | 172.16.1.13  | /31  |
|               | Ethernet2 | 172.16.1.15  | /31  |
|               | Ethernet3 | 172.16.2.13  | /31  |
|               | Ethernet4 | 172.16.2.15  | /31  |
|               | Ethernet5 | 172.16.3.13  | /31  |
|               | Ethernet6 | 172.16.3.15  | /31  |
|               | Lo0       | 10.4.4.4     | /32  |
| 

### Конфигурация устройств
#### pd01-clf-001
```
interface Ethernet1/1
  description pd01-elf-001
  no switchport
  medium p2p
  ip address 172.16.1.0/31

interface Ethernet1/2
  description pd01-elf-001
  no switchport
  medium p2p
  ip address 172.16.1.2/31

interface Ethernet1/3
  description pd01-elf-002
  no switchport
  medium p2p
  ip address 172.16.1.4/31

interface Ethernet1/4
  description pd01-elf-002
  no switchport
  medium p2p
  ip address 172.16.1.6/31

interface Ethernet1/5
  description pd01-elf-003
  no switchport
  medium p2p
  ip address 172.16.1.8/31

interface Ethernet1/6
  description pd01-elf-003
  no switchport
  medium p2p
  ip address 172.16.1.10/31

interface Ethernet1/7
  description pd01-elf-004
  no switchport
  medium p2p
  ip address 172.16.1.12/31

interface Ethernet1/8
  description pd01-elf-004
  no switchport
  medium p2p
  ip address 172.16.1.14/31
 
 
``` 
#### Spine02
```
interface Ethernet1  
   no switchport  
   ip address 172.16.0.13/31 
interface Ethernet2  
   no switchport  
   ip address 172.16.0.17/31  
interface Ethernet3  
   no switchport  
   ip address 172.16.0.21/31  
interface Loopback1  
   ip address 10.0.2.1/32 
```
#### Leaf01
```
vlan 10  
interface Ethernet1  
   no switchport  
   ip address 172.16.0.10/31  
interface Ethernet2  
   no switchport  
   ip address 172.16.0.12/31  
interface Ethernet3  
   switchport access vlan 10  
   spanning-tree portfast  
interface Loopback1  
   ip address 10.1.1.1/32  
interface Vlan10  
   ip address 10.1.10.1/24  
```
#### Leaf02
```
vlan 20  
interface Ethernet1  
   no switchport  
   ip address 172.16.0.14/31  
interface Ethernet2  
   no switchport  
   ip address 172.16.0.16/31  
interface Ethernet3  
   switchport access vlan 20  
   spanning-tree portfast  
interface Loopback1  
   ip address 10.1.2.1/32 
interface Vlan20  
   ip address 10.2.20.1/24  
```
#### Leaf03
```
vlan 30  
interface Ethernet1  
   no switchport  
   ip address 172.16.0.18/31  
interface Ethernet2  
   no switchport  
   ip address 172.16.0.20/31  
interface Ethernet3  
   switchport access vlan 30  
   spanning-tree portfast  
interface Ethernet4  
   switchport access vlan 30  
   spanning-tree portfast  
interface Vlan30  
   ip address 10.3.30.1/24 
``` 
#### PC1
```
IP/MASK: 10.1.10.2/24  
GATEWAY: 10.1.10.1
```  
#### PC2
```
IP/MASK: 10.2.20.2/24  
GATEWAY: 10.2.20.1
```  
#### PC3
```
IP/MASK: 10.3.30.2/24  
GATEWAY: 10.3.30.1  
```
#### PC4
```
IP/MASK: 10.3.30.3/24  
GATEWAY: 10.3.30.1 
``` 
#### PC5
#### PC6
123245
#### PC7
#### PC8
#### PC8