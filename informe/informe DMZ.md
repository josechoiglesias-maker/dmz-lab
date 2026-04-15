Alumno Jose Manuel Calvo
Fecha 15/04/2026


Informe de configuración de DMZ con Cisco Packet Tracer

1. Objetivo del laboratorio

El objetivo de este laboratorio fue diseñar e implementar una arquitectura de red con una zona desmilitarizada (DMZ) utilizando un router Cisco ISR, aplicando NAT y listas de control de acceso (ACL) para controlar y restringir el tráfico entre la red interna (LAN), la DMZ y la red externa (Internet), garantizando así un entorno seguro.


2. Topología implementada
Cantidad de redes: 3
Dispositivos usados:
1 Router Cisco ISR 2911 (Router_FW)
3 Switches Cisco 2960
2 PCs (PC_Internal, PC_External)
1 Servidor (Server-PT Web_DMZ)
Descripción de las zonas
LAN (192.168.1.0/24):
Red interna donde se encuentra el usuario (PC_Internal). Es la red más segura.
DMZ (192.168.2.0/24):
Zona intermedia donde se ubica el servidor web. Permite acceso controlado desde el exterior.
Red Externa (192.168.3.0/24):
Simula Internet (PC_External), desde donde se accede al servidor mediante NAT.


3. Plan de direccionamiento IP
Dispositivo	            IP	            Máscara	Gateway
PC_Internal	            192.168.1.10	255.255.255.0	192.168.1.1
Server_DMZ	            192.168.2.10	255.255.255.0	192.168.2.1
PC_External	            192.168.3.10	255.255.255.0	192.168.3.1
Router_FW Gi0/0 (LAN)	192.168.1.1	    255.255.255.0	
Router_FW Gi0/1 (DMZ)	192.168.2.1	    255.255.255.0	
Router_FW Gi0/2 (Ext)	192.168.3.1	    255.255.255.0	


4. Configuración aplicada (resumen)
Configuración de interfaces
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0

interface GigabitEthernet0/1
 ip address 192.168.2.1 255.255.255.0
 ip nat inside
 ip access-group 101 in

interface GigabitEthernet0/2
 ip address 192.168.3.1 255.255.255.0
 ip nat outside
 ip access-group 100 in
NAT (publicación del servidor DMZ)
ip nat inside source static 192.168.2.10 192.168.3.1
 Permite que el servidor web sea accesible desde la red externa mediante la IP 192.168.3.1.

ACL 100 (Internet → DMZ)
access-list 100 permit tcp any host 192.168.3.1 eq 80

Permite únicamente tráfico HTTP desde Internet hacia el servidor.

ACL 101 (DMZ  LAN)
access-list 101 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 101 permit ip any any  Bloquea completamente el acceso desde la DMZ hacia la red interna.


5. Verificaciones realizadas
Se realizaron pruebas de conectividad y acceso:

ping desde PC_Internal al router  Correcto
Acceso web desde PC_Internal al servidor DMZ  Correcto
Acceso web desde PC_External (mediante NAT) Correcto
Ping desde PC_External hacia el servidor  Bloqueado (correcto)
Comunicación desde DMZ hacia LAN Bloqueada (correcto)

Todas las pruebas cumplieron con los requisitos de seguridad del laboratorio.

6. Conclusiones y recomendaciones
Este laboratorio permitió comprender cómo implementar una arquitectura de red segura utilizando:

Segmentación de red (LAN, DMZ, WAN)
NAT estático para publicar servicios
ACLs extendidas para controlar el tráfico
Aprendizajes clave:
Las ACL deben aplicarse en la interfaz y dirección correctas.
NAT solo afecta tráfico entre redes diferentes (no interno).
El orden de las reglas en ACL es crítico.
Recomendaciones:
Verificar conectividad básica antes de aplicar seguridad.
Evitar usar la misma IP del router como IP pública en NAT.
Documentar siempre la topología antes de configurar.


7. Capturas de evidencia
Se deben incluir:

Resultados de show access-lists
show running-config
Pruebas de ping
Acceso web desde navegador
Resultados de “Connectivity Tests” en Packet Tracer


CONCLUSIÓN FINAL

Este laboratorio representa una implementación básica pero realista de un firewall perimetral con DMZ, aplicando buenas prácticas de seguridad en redes