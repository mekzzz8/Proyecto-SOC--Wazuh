# Proyecto-SOC-Wazuh
En este proyecto he realizado el despliegue de un entorno de monitorizción de seguridad con wazuh utilizando docker sobre Ubuntu y Windows 11 en Parallels Desktop

## FASE 1 - Preparación del entorno Virtual en Parallels Desktop

Lo primero que tenemos que hacer al trabajar con Mac con un procesador Apple Silicon es virtualizar los sistemas operativos con arquitectura ARM64. Necesitamos dos máquinas, el cerebro del proyecto (Ubuntu Server 24.04.3) y la víctima (Windows 11).

+ PRIMER PASO: Creación del Servidor Wazuh Manager --> Ubuntu
1. En el centro de control de Parallels, descargamos e instalamos Ubuntu 24.04.3 (Parallels se encarga de gestionar la versión ARM automáticamente).
2. Para los ajustes de recursos hardware, tenemos que saber que Wazuh es un sistema robusto y para que no colapse debemos ir a ajustes (en el engranaje) --> Hardware --> CPU y Memoria y le asignamos 4GB de RAM y al menos 2 procesadores.
   
   <img width="1392" height="960" alt="image" src="https://github.com/user-attachments/assets/ae9984a5-459c-4ec5-8838-e837bb38b58e" />

4. Para la configuración de red, en la misma configuración que en el punto anterior en hardware, vamos al apartado de red y seleccionamos "Red Compartida". Esto lo que nos crea es un router NAT virtual dentro del equipo y permite que la máquina tenga salida a internet y que ambas máquinas virtuales estén en el mismo rango de red y se vean entre sí.
   
   <img width="1392" height="960" alt="image" src="https://github.com/user-attachments/assets/eb1c9986-7ab8-470f-9dcc-60780dd49ed9" />

+ SEGUNDO PASO: Creación de la máquina Víctima --> Windows 11
1. En el centro de control de Parallels, descargamos e instalamos Windows 11.
2. Para la configuración de red es igual que en la parte de Ubuntu, seleccionamos "Red Compartida".

  <img width="1392" height="612" alt="image" src="https://github.com/user-attachments/assets/fa055aae-22db-4cd1-b02c-11602075f4d4" />


## FASE 2 - Configuración de Red y Preparación del Servidor

Una vez hemos configurado y encendido las dos máquinas, necesitamos mapear nuestra red y preparar el servidor ubuntu para alojar los contenedores de Wazuh.

1. Identificar el direccionamiento IP: Necesitamos conocer la IP de la máquina Ubuntu. Para ello abrimos una Terminal y ejecutamos el comando --> `ip a`
2. Anotamos la dirección IP que será la IP central de nuestro SOC.
3. Realizamos el mismo paso en la máquina victima (Windows 11) abriendo Powershell y ejecutando el comando --> `ipconfig`. Anotamos la IPv4 correspondiente.

Lo primero que tenemos que hacer es comprobar la conectividad entre las dos máquinas. Para ello realizamos un Ping desde Powershell con el comando --> `ping + la ip de ubuntu` - Ejemplo: "ping 192.168.x.x"

*Nota: Si ejecutamos el ping desde ubuntu a Windos es propable que falle por el firewalls del propio windows*

## Preparación del Servidor

+ Lo primero que tenemos que hacer es Actualizar los repositorios de Ubuntu para evitar posibles problemas de dependencias:
  Para ello, ejecutaremos el siguiente comando -->  `sudo apt update && sudo apt upgrade -y`

+ Para la **instalación** de Docker y Docker Compose, vamos a tener que instalar el motor de contenedores y la herramienta de orquestación con el comando -->  `sudo apt install docker.io docker-compose -y`
  

