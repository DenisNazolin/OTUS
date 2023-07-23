### Схема CLOS
### Цели

### Конфигурация устройств
#### pd01-clf-001
```
router isis 1
  net 49.0001.0100.0000.1001.00
  int eth1/1-8
  ip router isis 1  
  int lo0
  ip router isis 0
```
#### pd01-clf-002
```
router isis 0
  net 49.0001.0100.0000.2001.00
  int eth1/1-8
  ip router isis 0
  int lo0
  ip router isis 0
```
#### pd01-clf-003
```
router isis 0
  net 49.0001.0100.0000.3001.00
  int eth1/1-8
  ip router isis 0 
  int lo0
  ip router isis 0  
```
#### pd01-elf-001
```
router isis 0
  net 49.0001.0100.0100.1001.00
  int eth1/1-6
  ip router isis 0
  int lo0
  ip router isis 0
```
#### pd01-elf-002
```
conf t 
feature isis
router isis 0
  net 49.0001.0200.0200.2002.00
  int eth1/1-6
  ip router isis 0
  int lo0
  ip router isis 0
```

#### pd01-elf-003
```
conf t 
feature isis
router isis 0
  net 49.0001.0300.0300.3003.00
  int eth1/1-6
  ip router isis 0
  int lo0
  ip router isis 0
```

#### pd01-elf-004
```
conf t 
feature isis
router isis 0
  net 49.0001.0400.0400.4004.00
  int eth1/1-6
  ip router isis 0
  int lo0
  ip router isis 0
```



### Вывод  команд проверки 
#### pd01-clf-001
```
pd01-clf-001# sh isis adjacency
IS-IS process: 1 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
pd01-elf-001    N/A             1-2    UP     00:00:24   Ethernet1/1
pd01-elf-001    N/A             1-2    UP     00:00:27   Ethernet1/2
pd01-elf-002    N/A             1-2    UP     00:00:32   Ethernet1/3
pd01-elf-002    N/A             1-2    UP     00:00:30   Ethernet1/4
pd01-elf-003    N/A             1-2    UP     00:00:29   Ethernet1/5
pd01-elf-003    N/A             1-2    UP     00:00:29   Ethernet1/6
pd01-elf-004    N/A             1-2    UP     00:00:28   Ethernet1/7
pd01-elf-004    N/A             1-2    UP     00:00:22   Ethernet1/8
```

#### pd01-elf-001
```
pd01-elf-002# sh isis adjacency
IS-IS process: 0 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
pd01-clf-002    N/A             1-2    UP     00:00:21   Ethernet1/1
pd01-clf-002    N/A             1-2    UP     00:00:26   Ethernet1/2
pd01-clf-001    N/A             1-2    UP     00:00:25   Ethernet1/3
pd01-clf-001    N/A             1-2    UP     00:00:26   Ethernet1/4
pd01-clf-003    N/A             1-2    UP     00:00:33   Ethernet1/5
pd01-clf-003    N/A             1-2    UP     00:00:22   Ethernet1/6
```

### Выводы по проверки 
так как сеседтво видно со всеми спайнами есть от лифа, а от лифа со всеми спайнами связность есть.
Так же доступны все lo0