### Схема CLOS
### Цели
1. Собрать топологию CLOS.
2. Распределить адресное пространство для Underlay сети.



### Реализовать схему 

### Таблица адресов
| Device        | Interface | IP Address   | Mask |
| ------------- |:----------| :------------| :----|
| Spine01       | Ethernet1 | 172.16.0.11  | /31  |
|               | Ethernet2 | 172.16.0.15  | /31  |
|               | Ethernet3 | 172.16.0.19  | /31  |
| Spine02       | Ethernet1 | 172.16.0.13  | /31  |
|               | Ethernet2 | 172.16.0.17  | /31  |
|               | Ethernet3 | 172.16.0.21  | /31  |
| Leaf01        | Ethernet1 | 172.16.0.10  | /31  |
|               | Ethernet2 | 172.16.0.12  | /31  |
|               | vlan10    | 10.1.10.1    | /24  |
| Leaf02        | Ethernet1 | 172.16.0.14  | /31  |
|               | Ethernet2 | 172.16.0.16  | /31  |
|               | vlan20    | 10.2.20.1    | /24  |
| Leaf03        | Ethernet1 | 172.16.0.18  | /31  |
|               | Ethernet2 | 172.16.0.20  | /31  |
|               | vlan30    | 10.3.30.1    | /24  |
| PC1           | Ethernet0 | 10.1.10.2    | /24  |
| PC2           | Ethernet0 | 10.2.20.2    | /24  |
| PC3           | Ethernet0 | 10.3.30.2    | /24  |
| PC4           | Ethernet0 | 10.3.30.3    | /24  | 

### Конфигурация устройств
#### Spine01
```
interface Ethernet1  
   no switchport  
   ip address 172.16.0.11/31  
interface Ethernet2  
   no switchport  
   ip address 172.16.0.15/31
interface Ethernet3  
   no switchport  
   ip address 172.16.0.19/31  
interface Loopback1  
   ip address 10.0.1.1/32 
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
