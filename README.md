# Tarea 03 - Modos de red VMs

1 - Asignación de hostname: carlos-vela

# Escenario 1: DHCP

> IP Asignada por DHCP: 192.168.1.59/24

## Configuración de red y ping a 8.8.8.8

![](/images/dhcp-bridge.png)

# Escenario 2: IP Estatica en subred

## Configuración en netplan

> IP Asignada manualmente: 192.168.1.40/24

![](/images/config-static-ip.png)

## Configuración de red y ping a 8.8.8.8

![](/images/static-ip.png)

# Escenario 3: IP Estatica fuera de la subred

## Configuración en netplan

> IP Asignada manualmente: 192.168.2.250/24

![](/images/static-external-ip-config.png)

## Configuración de red y ping a 8.8.8.8 (FALLIDO)

![](/images/static-external-ip.png)

> Nota: Al asignar una IP fuera del segmento de la subred del hipervisor (192.168.1.1/24), la maquina virtual no tiene acceso a la red ni a internet porque es incapaz de conocer como enrutar hacia la puerta de enlace predeterminada (termina con .1.1)

# Especificación de subred del Hipervisor

Subred del hipervisor:

Hosts utilizables: 192.168.1.1 - 192.168.1.254

Mascara de red: 255.255.255.0

![](/images/hipervisor-subnet.png)