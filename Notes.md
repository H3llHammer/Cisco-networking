---
title: Cisco Networking
---

<h2 style="color:red">Comandos show</h2>

| Command               | Utilidad                                                               |
| --------------------- | ---------------------------------------------------------------------- |
| `show running-config` | Verificar la configuración actual                                      |
| `show interfaces`     | Verificar el estado de la interfaz y ver si hay algún mensaje de error |
| `show ip interface`   | Verificar la información de la capa 3 de una interfaz                  |
| `show arp`            | Verificar la lista de hosts conocidos en las LAN Ethernet locales      |
| `show ip route`       | Verificar la información de enrutamiento de la capa 3                  |
| `show protocols`      | Verificar qué protocolos están operativos                              |
| `show version`        | Verificar la memoria, las interfaces y las licencias del dispositivo   |

<h2 style="color:red">Configuracion router</h2>

### Nombre del dispositivo

- Router(config)# `hostname <hostname>`

### Proteger el modo EXEC (linea de consola)

- Router(config)# `line console 0`
- Router(config-line)# `password <password>`
- Router(config-line)# `login`

### Acceso remoto seguro a Telnet/SSH

- Router(config-line)# `line vty 0 4`
- Router(config-line)# `password <password>`
- Router(config-line)# `login`
- Router(config-line)# `transport input ssh telnet`

### Proteger el modo EXEC con privilegios

- Router(config)# `enable secret <password>`

### Cifrar todas las contraseñas

- Router(config)# `service password-encryption`

### Asignar tamaño minimo a contraseña

- Router(config)# `security passwords min-length 8`

### limitar intentos de inicio de sesión vty durante sierto tiempo

- Router(config)# `login block-for 120 attempts 3 within 60`

### Crear banner

- Router(config)# `banner motd #¡Acceso autorizado únicamente!#`

### Verificación de configuración de interfaz

Proporciona un resumen de la información clave para todas las interfaces de red de un router/switch.

- Router# `show ip interface brief`

### Comando show cdp neighbors

Obtener información de dispositivos vecinos

- Router# `show cdp neighbors`

Revela la dirección IP de un dispositivo vecino

- Router# `show cdp neighbors detail`

Para deshabilitar CDP globalmente

- Router(config)# `no cdp run`

Para deshabilitar CDP en una interfaz

- Router(config)# `int fa0/x`
- Router(config-if)# `no cdp enable`

## IPv6

- Router# `show ipv6 interface brief`

### Guardar la configuracion

- Router# `copy running-config startup-config`

### Mostrar la tabla ARP

- Router# `show ip arp`

### Mostrar estadísticas de todas las interfaces del dispositivo.

- Router# `show interfaces`

### Mostrar las estadísticas de IPv4 correspondientes a todas las interfaces de un router.

- Router# `show ip interfaces`

### Listar servicios

- Router# `show ip ports all`/`show control-plane host open-ports`

### Listas de control de acceso

### Enrutamiento estatico

- Router(config)# `ip route <ip-destino> <mascara> <ip-origen>`

### Dar acceso a todas las redes (enrutamiento estatico con rutas por default)

- Router(config)# `ip route 0.0.0.0 0.0.0.0 <salto>`
- Router(config)# `ip route 0.0.0.0 0.0.0.0 <interfaz>`

## Enrutamiento dinamico

### RIP

- Routing information protocol
- Distancia administrativa = 120
- Utiliza saltos como metrica
- Solo soporta 15 saltos
- Solo se publican las redes directamente conectadas
- Dos versiones: V1 y V2
- RIP V2 soporta VLSM

## RIP v1

- Router(config)# `router rip`
- Router(config-router)# `network <network>`

## RIP v2

- Router(config)# `router rip`
- Router(config)# `ver 2`
- Router(config-router)# `network <network>`
- Router(config-router)# `no auto-summary`

## EIGRP

Enhanced Interior Gateway Routing Protocol [90/ métrica]

- Router(config)# `router eigrp <1-65535>`
- Router(config-router)# `network <red principal>`

## OSPF

Open Shortest Path First
costo(metrica) = 10000 0000/ancho de banda en bps

- Router(config)# `router ospf <#>`
- Router(config-router)# `network x.x.x.x <wildcard x.x.x.x> area <#>`

### Ver tabla de enrutamiento

- Router# `show ip route`
- Router(config)# `do show ip route`

### Configurar subinterfaz

- Router(config)# `int fa0/x`
- Router(config-if)# `no shut`
- Router(config-if)# `int fa0/x.x`
- Router(config-subif)# `encapsulation dot1q <vlan>`
- Router(config-subif)# `ip add <direccion> <mascara>`

### Configurar interfaz serial

- Router(config)# `int sex/x`
- Router(config-if)# `no shut`
- Router(config-if)# `ip address <ip> <mascara>`
- Router(config-if)# `clock rate 64000`

### Configurar interfaz loopback

- Router(config)# `int loopback <num>`
- Router(config-if)# `ip add <direccion> <mascara>`

### Configurar servidor DHCP

- Router(config)# `ip dhcp pool <nombre>`
- Router(dhcp-config)# `network 10.0.0.0 255.0.0.0`
- Router(dhcp-config)# `default-router 10.0.0.1`

---

<h2 style="color:red">Configuracion switch</h2>

Para el acceso a la administración remota de un switch, este se debe configurar
con una dirección IP y una máscara de subred. Recuerde que para administrar un switch
desde una red remota, se lo debe configurar con un gateway predeterminado.

- SVI: interfaz virtual del switch. La SVI es una interfaz virtual, no un puerto físico del switch.

De manera predeterminada, el switch está configurado para que el control de la administración del
switch se realice mediante la VLAN 1.

### Configuracion de la interfaz de administracion de un switch

- switch(config)# `interface vlan 99`
- switch(config-if)# `ip address <ip> <mask>`
- switch(config-if)# `no shutdown`
- switch(config-if)# `end`
- switch# `copy runnig-config startup-config`

### Configuracion del gateway predeterminado de un switch

- switch(config)# `ip default-gateway <ip>`
- switch(config)# `end`
- switch# `copy running-config startup-config

### Verificar la configuracion de la interfaz de administracion de un switch

- switch# `show ip interface brief`

### Configurar puerto como trunk

- switch(config)# `int fa'/*`
- switch(conf-if)# `switchport mode trunk`

---

<h2 style="color:red">Vlan's</h2>

Las VLAN son grupos lógicos numerados a los que se pueden asignar puertos físicos.
Los parámetros de configuración aplicados a una VLAN también se aplican a todos los
puertos asignados a esa VLAN.

### Crear vlan

- Switch(config)# `vlan <number>`
- Switch(config-vlan)# `name <name>`

### Asignar puertos a vlan

- Switch# `configure terminal`
- Switch(config)# `interface <id-interface>`
  > Asignar un rango de puertos:
- Switch(config)# `interface range <puertos>`
- Switch(config-if-range)# `switchport mode access`
- Switch(config-if-range)# `switchport access vlan <id-vlan>`
- Switch(config-if-range)# `end`

### Listar informacin de VLAN

- Switch# `show vlan brief`

### Etherchannel

- Switch(config)# `interface range fax/x - x`
- Switch(config-if-range)# `shutdown`
- Switch(config-if-range)# `channel-group 1 mode desirable`
- Switch(config-if-range)# `no shutdown`
- Switch(config-if-range)# `exit`
- Switch(config)# `interface port-channel #`
- Switch(config)# `switchport mode trunk`
- Switch# `show interface port-channel #`

---
