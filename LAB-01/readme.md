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
#### pd01-clf-002
```
interface Ethernet1/1
  description pd01-elf-002
  no switchport
  medium p2p
  ip address 172.16.2.4/31

interface Ethernet1/2
  no switchport
  medium p2p
  ip address 172.16.2.6/31

interface Ethernet1/3
  description pd01-elf-001
  no switchport
  medium p2p
  ip address 172.16.2.0/31

interface Ethernet1/4
  description pd01-elf-001
  no switchport
  medium p2p
  ip address 172.16.2.2/31

interface Ethernet1/5
  description pd01-elf-003
  no switchport
  medium p2p
  ip address 172.16.2.8/31

interface Ethernet1/6
  description pd01-elf-003
  no switchport
  medium p2p
  ip address 172.16.2.10/31

interface Ethernet1/7
  description pd01-elf-004
  no switchport
  medium p2p
  ip address 172.16.2.12/31

interface Ethernet1/8
  description pd01-elf-004
  no switchport
  medium p2p
  ip address 172.16.2.14/31

```
#### pd01-elf-001
```
interface Ethernet1/1
  description pd01-clf-001
  no switchport
  medium p2p
  ip address 172.16.1.1/31


interface Ethernet1/2
  description pd01-clf-001
  no switchport
  medium p2p
  ip address 172.16.1.3/31


interface Ethernet1/3
  description pd01-clf-002
  no switchport
  medium p2p
  ip address 172.16.2.1/31


interface Ethernet1/4
  description pd01-clf-002
  no switchport
  medium p2p
  ip address 172.16.2.3/31


interface Ethernet1/5
  description pd01-clf-003
  no switchport
  medium p2p
  ip address 172.16.3.1/31

interface Ethernet1/6
  description pd01-clf-003
  no switchport
  medium p2p
  ip address 172.16.3.3/31



```
#### pd01-elf-002
```
interface Ethernet1/1
  description pd01-clf-002
  no switchport
  medium p2p
  ip address 172.16.2.5/31


interface Ethernet1/2
  description pd01-clf-002
  no switchport
  medium p2p
  ip address 172.16.2.7/31


interface Ethernet1/3
  description p01-cfl-001
  no switchport
  medium p2p
  ip address 172.16.1.5/31


interface Ethernet1/4
  description p01-cfl-001
  no switchport
  medium p2p
  ip address 172.16.1.7/31


interface Ethernet1/5
  description pd01-clf-003
  no switchport
  medium p2p
  ip address 172.16.3.5/31


interface Ethernet1/6
  description pd01-clf-003
  no switchport
  medium p2p
  ip address 172.16.3.7/31


```
#### pd01-elf-003
```
interface Ethernet1/1
  description pd01-clf-002
  no switchport
  medium p2p
  ip address 172.16.2.9/31


interface Ethernet1/2
  description pd01-clf-002
  no switchport
  medium p2p
  ip address 172.16.2.11/31


interface Ethernet1/3
  description pd01-clf-001
  no switchport
  medium p2p
  ip address 172.16.1.9/31


interface Ethernet1/4
  description pd01-clf-001
  no switchport
  medium p2p
  ip address 172.16.1.11/31


interface Ethernet1/5
  description pd01-clf-003
  no switchport
  medium p2p
  ip address 172.16.3.9/31


interface Ethernet1/6
  description pd01-clf-003
  no switchport
  medium p2p
  ip address 172.16.3.11/31



```
#### pd01-elf-004
```
interface Ethernet1/1
  description pd01-clf-001
  no switchport
  medium p2p
  ip address 172.16.1.13/31


interface Ethernet1/2
  description pd01-clf-001
  no switchport
  medium p2p
  ip address 172.16.1.15/31


interface Ethernet1/3
  description pd01-clf-002
  no switchport
  medium p2p
  ip address 172.16.2.13/31


interface Ethernet1/4
  description pd01-clf-002
  no switchport
  medium p2p
  ip address 172.16.2.15/31



interface Ethernet1/5
  description pd01-clf-003
  no switchport
  medium p2p
  ip address 172.16.3.13/31


interface Ethernet1/6
  description pd01-clf-003
  no switchport
  medium p2p
  ip address 172.16.3.15/31



```


### PS 
Решил добавить больше утсройств и резервные линки для эксплуатации в будущих схемах, и с гитом пока на ВЫ так что без картинок прошу простить.