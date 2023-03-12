## Contents

   1. [Инструмент ipcalc](#part-1-инструмент-ipcalc)
   2. [Статическая маршрутизация между двумя машинами](#part-2-статическая-маршрутизация-между-двумя-машинами)
   3. [Утилита iperf3](#part-3-утилита-iperf3)
   4. [Сетевой экран](#part-4-сетевой-экран)
   5. [Статическая маршрутизация сети](#part-5-статическая-маршрутизация-сети)
   6. [Динамическая настройка IP с помощью DHCP](#part-6-динамическая-настройка-ip-с-помощью-dhcp)
   7. [NAT](#part-7-nat)
   8. [Допополнительно. Знакомство с SSH Tunnels](#part-8-дополнительно-знакомство-с-ssh-tunnels)
   
## Part 1. Инструмент **ipcalc**

**ipcalc** предоставляет простой метод для вычисления IP-информации хоста.

![/cat/issue](/screenshots/ipcalc1.png)


<img src="/src/screenshots/ipcalc1.png" alt="ipcalc1" width="600"/>

### 1.1. Сети и маски

##### 1) Адрес сети 192.167.38.54/13

<img src="/src/screenshots/address.png" alt="ipcalc1" width="700"/>

##### 2) Перевод маски 255.255.255.0 в префиксную и двоичную запись, /15 в обычную и двоичную, 11111111.11111111.11111111.11110000 в обычную и префиксную

<img src="/src/screenshots/netmask.png" alt="ipcalc1" width="700"/>

##### 3) Минимальный и максимальный хост в сети 12.167.38.4 при масках; /8, 11111111.11111111.00000000.00000000, 255.255.254.0 и /4

<img src="/src/screenshots/host.png" alt="ipcalc1" width="700"/>

### 1.2. localhost

##### Определяем, можно ли обратиться к приложению, работающему на localhost, со следующими IP: *194.34.23.100/16*, *127.0.0.2/24*, *127.1.0.1/8*, *128.0.0.1/8*

<img src="/src/screenshots/194.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/127.0.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/127.1.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/128.png" alt="ipcalc1" width="600"/>

Обратиться можно только к приложениям с IP-адресом 127.0.0.2/24 и 127.1.0.1/8.

### 1.3. Диапазоны и сегменты сетей

##### Определяем
##### 1) какие из перечисленных IP можно использовать в качестве публичного, а какие только в качестве частных: *10.0.0.45/8*, *134.43.0.2/16*, *192.168.4.2/16*, *172.20.250.4/12*, *172.0.2.1/12*, *192.172.0.1/12*, *172.68.0.2/12*, *172.16.255.255/12*, *10.10.10.10/8*, *192.169.168.1/16*
##### 2) какие из перечисленных IP адресов шлюза возможны у сети *10.10.0.0/18*: *10.0.0.1*, *10.10.0.2*, *10.10.10.10*, *10.10.100.1*, *10.10.1.255*

<img src="/src/screenshots/10.0.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/134.43.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/192.168.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/172.20.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/172.0.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/192.172.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/172.68.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/172.16.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/10.10.png" alt="ipcalc1" width="600"/>
<img src="/src/screenshots/192.169.png" alt="ipcalc1" width="600"/>

В качестве частных IP можно использовать 10.0.0.45/8, 192.168.4.2/16, 172.20.250.4/12, 192.172.0.1/12, 172.16.255.255/12, 10.10.10.10/8.

В качестве публичных 134.43.0.2/16, 172.0.2.1/12, 172.68.0.2/12, 192.169.168.1/16.

<img src="../src/screenshots/1.1.3.png" alt="ipcalc1" width="600"/>

Из перечисленных IP адресов шлюза возможны у сети 10.10.0.0/18: 10.10.0.2, 10.10.10.10, 10.10.1.255.

## Part 2. Статическая маршрутизация между двумя машинами

##### Поднять две виртуальные машины (ws1 и ws2)

##### С помощью команды `ip a` смотрим существующие сетевые интерфейсы

<img src="../src/screenshots/2.1.png" alt="ip a" width="600"/>

##### Задаём следующие адреса и маски: ws1 - *192.168.100.10*, маска */16*, ws2 - *172.24.116.8*, маска */12*

<img src="../src/screenshots/2.2.1.png" alt="ws1" width="600"/>
<img src="../src/screenshots/2.2.2.png" alt="ws2" width="600"/>

##### Выполняем команду `netplan apply` для перезапуска сервиса сети

<img src="../src/screenshots/2apply.png" alt="net apply" width="600"/>

#### 2.1. Добавление статического маршрута вручную
##### Добавляем статический маршрут от одной машины до другой и обратно при помощи команды вида `ip r add`

<img src="../src/screenshots/2.1.1.png" alt="ip r add" width="600"/>
<img src="../src/screenshots/2.1.2.png" alt="ip r add" width="600"/>

##### Пингуем соединение между машинами

<img src="../src/screenshots/2.1.3.png" alt="ping" width="600"/>
<img src="../src/screenshots/2.1.4.png" alt="ping" width="600"/>

#### 2.2. Добавление статического маршрута с сохранением
##### Перезапускаем машины
##### Добавляем статический маршрут от одной машины до другой с помощью файла *etc/netplan/00-installer-config.yaml*

<img src="../src/screenshots/2.1.5.png" alt="yaml" width="600"/>
<img src="../src/screenshots/2.1.6.png" alt="yaml" width="600"/>

##### Пингуем соединение между машинами

<img src="../src/screenshots/2.1.7.png" alt="ping" width="600"/>
<img src="../src/screenshots/2.1.8.png" alt="ping" width="600"/>

## Part 3. Утилита **iperf3**

#### 3.1. Скорость соединения
##### Перевести и записать в отчёт: 8 Mbps в MB/s, 100 MB/s в Kbps, 1 Gbps в Mbps

8 Mbps to MB/s = 1 MB/s

100 MB/s to Kbps = 819 200 Kbps

1 Gbps to Mbps = 1000 Mbps

#### 3.2. Утилита **iperf3**
##### Измерить скорость соединения между ws1 и ws2

<img src="../src/screenshots/3.2.png" alt="iperf3" width="600"/>

## Part 4. Сетевой экран

#### 4.1. Утилита **iptables**
##### Создаем файл */etc/firewall.sh*, имитирующий фаерволл, на ws1 и ws2

##### Добавляем в файл подряд следующие правила:
##### 1) на ws1 применить стратегию когда в начале пишется запрещающее правило, а в конце пишется разрешающее правило (это касается пунктов 4 и 5)
##### 2) на ws2 применить стратегию когда в начале пишется разрешающее правило, а в конце пишется запрещающее правило (это касается пунктов 4 и 5)
##### 3) открыть на машинах доступ для порта 22 (ssh) и порта 80 (http)
##### 4) запретить *echo reply* (машина не должна "пинговаться”)
##### 5) разрешить *echo reply* (машина должна "пинговаться")

<img src="../src/screenshots/4.1.1.png" alt="ws1 firewall"/>
<img src="../src/screenshots/4.1.2.png" alt="ws2 firewall"/>

##### Запустим файлы на обеих машинах командами `chmod +x /etc/firewall.sh` и `/etc/firewall.sh`

<img src="../src/screenshots/4.1.3.png" alt="chmod ws1 firewall"/>
<img src="../src/screenshots/4.1.4.png" alt="chmod ws2 firewall"/>

В первом файле мы сначала применяем запрещающее правило, затем разрешающее, а во втором разрешающее, затем запрещающее для того, чтобы на первой машине не принимать пакеты, а на второй принимать. Это демонстрирует нам то, что выполняется последнее записанное правило.

#### 4.2. Утилита **nmap**
##### Командой **ping** находим машину, которая не "пингуется"

<img src="../src/screenshots/4.2.1.png" alt="ping"/>

##### Утилитой **nmap** показываем, что хост машины запущен

<img src="../src/screenshots/4.2.2.png" alt="nmap"/>

## Part 5. Статическая маршрутизация сети

##### Поднимаем пять виртуальных машин (3 рабочие станции (ws11, ws21, ws22) и 2 роутера (r1, r2))

#### 5.1. Настройка адресов машин
##### Настроим конфигурации машин в *etc/netplan/00-installer-config.yaml* согласно сети на рисунке.

<img src="../src/screenshots/5.png" alt="part 5" width="900"/>

<img src="../src/screenshots/5.1.png" alt="netplan" width="900"/>

##### Перезапускаем сервис сети. Командой `ip -4 a` проверяем, что адрес машины задан верно. Также пингуем ws22 с ws21. Аналогично пингуем r1 с ws11.

<img src="../src/screenshots/5.1.1.png" alt="ip -4 a & ping"/>
<img src="../src/screenshots/5.1.2.png" alt="ping"/>

#### 5.2. Включение переадресации IP-адресов.
##### Для включения переадресации IP, выполняем команду на роутерах:
`sysctl -w net.ipv4.ip_forward=1`

<img src="../src/screenshots/5.2.1.png" alt="r1 ip forward"/>
<img src="../src/screenshots/5.2.2.png" alt="r2 ip forward"/>

##### В файле */etc/sysctl.conf* добавляем строку:
`net.ipv4.ip_forward = 1`

<img src="../src/screenshots/5.2.3.png" alt="sysctl.conf"/>

#### 5.3. Установка маршрута по-умолчанию

##### Настроим маршрут по-умолчанию (шлюз) для рабочих станций. Для этого добавим gateway4 \[ip роутера\] в файле конфигураций

<img src="../src/screenshots/5.3.1.png" alt="gateway" width="900"/>

##### Командой `ip r` показываем, что добавился маршрут в таблицу маршрутизации

<img src="../src/screenshots/5.3.2.png" alt="ip r" width="900"/>

##### Пингуем с ws11 роутер r2 и показать на r2, что пинг доходит. Для этого используем команду:
`tcpdump -tn -i enp0s9`

<img src="../src/screenshots/5.3.3.png" alt="ping"/>
<img src="../src/screenshots/5.3.4.png" alt="tcpdump"/>

#### 5.4. Добавление статических маршрутов
##### Добавим в роутеры r1 и r2 статические маршруты в файле конфигураций. Пример для r1 маршрута в сетку 10.20.0.0/26:

<img src="../src/screenshots/5.4.1.png" alt="r1 netplan"/>
<img src="../src/screenshots/5.4.2.png" alt="r2 netplan"/>

##### Вызовем `ip r` и посмотрим таблицы с маршрутами на обоих роутерах.

<img src="../src/screenshots/5.4.3.png" alt="r1 ip r"/>
<img src="../src/screenshots/5.4.4.png" alt="r2 ip r"/>

##### Запустим команды на ws11:
`ip r list 10.10.0.0/[маска сети]` и `ip r list 0.0.0.0/0`

<img src="../src/screenshots/5.4.5.png" alt="ip r list"/>

Для интерфейса 10.10.0.0 не нужна маршрутизация. 0.0.0.0 - это любой интерфейс, но для того, чтобы связаться с другой сетью машиной используется адрес 10.10.0.2

#### 5.5. Построение списка маршрутизаторов

##### Запустим на r1 команду дампа:
`tcpdump -tnv -i enp0s8`

<img src="../src/screenshots/5.5.1.png" alt="tcpdump"/>

##### При помощи утилиты **traceroute** построим список маршрутизаторов на пути от ws11 до ws21

<img src="../src/screenshots/5.5.2.png" alt="traceroute"/>

Путь строится от узла к узлу, пока не достигнет своей цели. Каждый передаваемый пакет проходит определенное количество узлов, пока не достигнет целевого узла.

#### 5.6. Использование протокола **ICMP** при маршрутизации
##### Запускаем на r1 перехват сетевого трафика, проходящего через enp0s8 с помощью команды:
`tcpdump -n -i enp0s8 icmp`

<img src="../src/screenshots/5.6.1.png" alt="tcpdump"/>

##### Пингуем с ws11 несуществующий IP

<img src="../src/screenshots/5.6.2.png" alt="ping"/>

## Part 6. Динамическая настройка IP с помощью **DHCP**

##### Укажем MAC адрес у ws11, для этого в *etc/netplan/00-installer-config.yaml* добавим строки:

<img src="../src/screenshots/6.1.png" alt="tcpdump"/>

##### Для r2 настроим в файле */etc/dhcp/dhcpd.conf* конфигурацию службы **DHCP**:
##### 1) укажем адрес маршрутизатора по-умолчанию, DNS-сервер и адрес внутренней сети

<img src="../src/screenshots/6.2.png" alt="dhcp.conf"/>

##### 2) в файле *resolv.conf* пропишем `nameserver 8.8.8.8`
<img src="../src/screenshots/6.3.png" alt="resolv"/>

##### Перезагрузим службу **DHCP** командой `systemctl restart isc-dhcp-server`

<img src="../src/screenshots/6.4.png" alt="dhcp restart" width="700"/>

##### Машину ws21 перезагрузим при помощи `reboot` и через `ip a` проверим, получила ли она адрес

<img src="../src/screenshots/6.5.png" alt="old ip"/>
<img src="../src/screenshots/6.6.png" alt="new ip"/>

##### Также пропингуем ws22 с ws21

<img src="../src/screenshots/6.7.png" alt="ping"/>

##### Для r1 настроим аналогично, но сделаем выдачу адресов с жесткой привязкой к MAC-адресу (ws11)

<img src="../src/screenshots/6.8.png" alt="dhcp.conf"/>
<img src="../src/screenshots/6.9.png" alt="resolv"/>
<img src="../src/screenshots/6.10.png" alt="dhcp restart"/>
<img src="../src/screenshots/6.11.png" alt="old ip"/>
<img src="../src/screenshots/6.12.png" alt="new ip" width="900"/>

##### Запросим с ws21 обновление ip адреса

<img src="../src/screenshots/6.13.png" alt="old ip"/>
<img src="../src/screenshots/6.14.png" alt="new ip"/>

- Опции **DHCP** сервера, которые использовались в этом пункте: ip адреса, адреса серверов DNS.

## Part 7. **NAT**

##### В файле */etc/apache2/ports.conf* на ws22 и r2 изменим строку `Listen 80` на `Listen 0.0.0.0:80`, то есть сделаем сервер Apache2 общедоступным

<img src="../src/screenshots/7.1.png" alt="ws22 ports.conf"/>

##### Запустим веб-сервер Apache командой `service apache2 start` на ws22 и r1

<img src="../src/screenshots/7.2.png" alt="apache start"/>

##### Добавим в фаервол, созданный по аналогии с фаерволом из Части 4, на r2 следующие правила:
##### 1) Удаление правил в таблице filter - `iptables -F`
##### 2) Удаление правил в таблице "NAT" - `iptables -F -t nat`
##### 3) Отбрасывать все маршрутизируемые пакеты - `iptables --policy FORWARD DROP`

<img src="../src/screenshots/7.3.png" alt="firewall"/>

##### Запустим файл также, как в Части 4

<img src="../src/screenshots/7.4.png" alt="sh firewall"/>

##### Проверим соединение между ws22 и r1 командой ping

*При запуске файла с этими правилами, ws22 должна "пинговаться" с r1*

<img src="../src/screenshots/7.5.png" alt="ping"/>

##### Добавить в файл ещё одно правило:
##### 4) Разрешить маршрутизацию всех пакетов протокола **ICMP**

<img src="../src/screenshots/7.6.png" alt="icmp"/>

##### Запустим файл также, как в Части 4 командой `sudo /etc/firewall.sh` и проверим соединение между ws22 и r1 командой `ping`

<img src="../src/screenshots/7.7.png" alt="ping"/>

##### Добавим в файл ещё два правила:
##### 5) Включим **SNAT**, а именно маскирование всех локальных ip из локальной сети, находящейся за r2 (по обозначениям из Части 5 - сеть 10.20.0.0)
##### 6) Включим **DNAT** на 8080 порт машины r2 и добавить к веб-серверу Apache, запущенному на ws22, доступ извне сети

<img src="../src/screenshots/7.8.png" alt="snat"/>

##### Запустим файл также, как в Части 4 и проверим соединение по TCP для **SNAT**, для этого с ws21 подключимся к серверу Apache на r1 командой:
`telnet [адрес] [порт]`

<img src="../src/screenshots/7.9.png" alt="telnet" width="500"/>

##### Проверим соединение по TCP для **DNAT**, для этого с r1 подключимся к серверу Apache на ws22 командой `telnet` (по адресу r2 и порту 8080)

<img src="../src/screenshots/7.10.png" alt="telnet" width="500"/>

## Part 8. Дополнительно. Знакомство с **SSH Tunnels**

##### Запустим веб-сервер **Apache** на ws22 только на localhost

<img src="../src/screenshots/8.1.png" alt="apache" width="500"/>

##### Воспользуемся *Local TCP forwarding* с ws21 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws21, выполнив команду:
`ssh -L 8888:localhost:80 matinish@10.20.0.20`

<img src="../src/screenshots/8.2.png" alt="local tcp"/>

##### Воспользуемся *Remote TCP forwarding* c ws11 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws11, выполнив команду:
`ssh -R 8888:localhost:80 matinish@10.20.0.20`

<img src="../src/screenshots/8.3.png" alt="remote tcp"/>

##### Для проверки, сработало ли подключение в обоих предыдущих пунктах, перейдем во второй терминал и выполним команду:
`telnet 127.0.0.1 8888`

<img src="../src/screenshots/8.4.png" alt="remote tcp"/>
<img src="../src/screenshots/8.5.png" alt="remote tcp"/>
