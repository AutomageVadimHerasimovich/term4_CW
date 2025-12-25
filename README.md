# term4_CW

# Конфигурирование EtherChannel

## Введение
EtherChannel — технология агрегации портов Cisco для повышения bandwidth и resiliency. Соответствует IEEE 802.3ad.

## Режимы
- **on**: Статический (SLA), без negotiation.
- **passive/active**: Динамический LACP (IEEE).
- **auto/desirable**: PAgP (Cisco proprietary).

## Требования
- Идентичные параметры: speed, duplex, VLAN, trunk mode.
- Несоответствие → suspended (down, orange LED).

## Конфигурирование
### L2 EtherChannel
Switch(config)# interface range gi0/1-2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# channel-group 1 mode on
Switch(config-if-range)# exit
Switch(config)# interface port-channel 1
Switch(config-if)# spanning-tree portfast
Switch(config-if)# exit
text### L3 EtherChannel
Switch(config)# interface range gi0/3-4
Switch(config-if-range)# no switchport
Switch(config-if-range)# no ip address
Switch(config-if-range)# channel-group 2 mode active
Switch(config-if-range)# exit
Switch(config)# interface port-channel 2
Switch(config-if)# no switchport
Switch(config-if)# ip address 192.168.0.11 255.255.255.0
Switch(config-if)# exit
text## Балансировка
`Switch(config)# port-channel load-balance dst-mac`  
Варианты: src-mac (default), dst-mac, src-ip, etc.

## Проверка
`Switch# show etherchannel`  
Вывод: Group state, ports, protocol.

## Дефолт конфиг
| Feature | Default |
|---------|---------|
| Channel groups | None |
| PAgP mode | No default |
| LACP priority | 32768 |
| Load balancing | src-mac |

## Расширения
- vPC на Nexus для multi-switch.

[Диаграмма 1](https://linuxtiwary.com/wp-content/uploads/2017/02/etherchannel-configuration.png)  
[Диаграмма 2](https://www.flackbox.com/wp-content/uploads/2021/01/26-1-EtherChannel-Configuration.png)
