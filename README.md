# demo2024

## №1

## Описание задания.
Выполните базовую настройку всех устройств:

1) Соберите топологию согласно рисунку. Все устройства работают на OC Linux - Debian
2) Присвоить имена в соответствии с топологией
3) Рассчитайте IP-адресацию IPv4 и IPv6. Необходимо заполнить таблицу №1. При необходимости отредактируйте таблицу.
4) Пул адресов для сети офиса BRANCH - не более 16. Для IPv6 пропустите этот пункт.
5) Пул адресов для сети офиса HQ - не более 64. Для IPv6 пропустите этот пункт.
  
## Топология №1.
 ![image](https://github.com/kutluberdinadi/DEMO2024/assets/148868105/91da25bb-955b-43cd-9ddc-ecf6cc8bcb7c)

### Таблица №1 (разбита на подсети).



| Имя утройства | Интерфейс | IPv4/IPv6 | Маска/Префикс | Шлюз | 
| ----------   |    --------  |    --------- | -------- | ------- |
| ISP  | ens256  | 192.168.0.162    |   /33    |          |         
|      | ens224  | 192.168.0.166   |/30  |
|      | ens192 | 10.12.20.10    |/24    | 10.12.20.254 |
| HQ-R |  ens192 | 192.168.0.161    | /30 | 192.168.0.162 |
|      | ens224 | 192.168.0.1   |/25    |  |
| BR-R | ens192 | 192.168.0.129 | /27 |  |
|      | ens224 | 192.168.0.165   |/30    | 192.168.0.166  |
| HQ-SRV | ens192  | 192.168.0.2  | /25 | 192.168.0.1 |
| BR-SRV | ens192  | 192.168.0.130 | /27 | 192.168.0.129 |

### 1. Настройка интерфесов.

#### Просмотр существующих интерфейсов.
```
ip a
```
#### Заходим на файл  конфигурации интерфейсов.
```
nano /etc/network/interfaces
```
#### Назначаем IP адреса в соотвествии с таблицей.
```
ISP
allow-hotplug ens192
iface ens192 inet dhcp
allow-hotplug ens224
iface ens224 inet static
address 192.168.0.166
netmask 255.255.255.252
allow-hotplug ens256
iface ens224 inet static
address 192.168.0.162
netmask 255.255.255.252
HQ-R
allow-hotplug ens224
iface ens224 inet static
address 192.168.0.166
netmask 255.255.255.252
gateway 192.168.0.162



```
#### Сохраняем комбинацией клавиш.
```
Ctrl+S
```
#### Выходим с комбинацией клавиш.
```
Ctrl+X
```
#### Перезагружаем сеть.
```
systemctl restar networking
```
#### Делаем тоже самое на других виртуальных машинах.

### №1.2

### Описание задания.

Настроить внутренюю динамическую маршрутицацию по средствам FRR. Выберите и обоснуйте выбор протокола динамической маршрутицации из расчёта, что в дальнейшем сеть будет масштабиоваться.

#### Установка пакета frr.
```
apt-get install frr   
