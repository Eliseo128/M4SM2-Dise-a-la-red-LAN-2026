Esta sesión didáctica de 3 horas combina la fundamentación teórica de redes LAN con una práctica guiada paso a paso en Cisco Packet Tracer.

---

## Parte 1: Conceptos Fundamentales (Teoría Didáctica)

Una **Red de Área Local (LAN)** interconecta equipos dentro de una extensión geográfica reducida (como una aula o laboratorio). Para diseñar una topología funcional en Cisco Packet Tracer, identificamos cuatro componentes esenciales:
<img width="602" height="320" alt="lan-act11" src="https://github.com/user-attachments/assets/d46b9384-0942-4485-9fe3-c24c2c252261" />

* **Área de Trabajo y Entorno:** Es el lienzo digital donde se construyen, simulan y analizan las redes mediante dos modos principales: *Real-time* (comportamiento en tiempo real) y *Simulation* (inspección paso a paso de paquetes PDU).
* **Dispositivos Finales (End Devices):** Equipos origen o destino de tráfico de datos (PC, Laptops, Servidores). Requieren una dirección IP única dentro de la subred y una máscara de red para comunicarse.
* **Dispositivos Intermedios (Switches y Routers):**
* *Switch (Capa 2):* Conecta múltiples dispositivos en la misma red local procesando tramas Ethernet mediante sus direcciones MAC.
* *Router (Capa 3):* Interconecta diferentes subredes y determina la mejor ruta para enviar paquetes usando direcciones IP.


* **Medios de Conexión (Cableado):**
* *Cable Directo (Copper Straight-Through):* Para conectar dispositivos de distinta capa (ej. PC a Switch, o Switch a Router).
* *Cable Cruzado (Copper Crossover):* Para conectar dispositivos del mismo nivel (ej. PC a PC, o Switch a Switch).



---

## Parte 2: Comandos y Ejemplos de Configuración (CLI Cisco IOS)

En redes profesionales, la interfaz gráfica (GUI) se sustituye por la línea de comandos (**CLI**). A continuación se presentan los comandos clave organizados por su nivel de ejecución en los dispositivos Cisco.
<img width="384" height="260" alt="cisco act11" src="https://github.com/user-attachments/assets/cd81245a-3bb9-494f-aab2-1c730332072f" />

### 1. Configuración Básica de un Switch (2960)

```cisco
! Acceso al modo privilegiado y de configuración global
Switch> enable
Switch# configure terminal

! Asignar nombre al dispositivo
Switch(config)# hostname SW-Principal

! Configurar mensaje de advertencia al ingresar
SW-Principal(config)# banner motd #Acceso restringido solo a personal autorizado.#

! Asignar IP a la interfaz de administración (VLAN 1)
SW-Principal(config)# interface vlan 1
SW-Principal(config-if)# ip address 192.168.1.2 255.255.255.0
SW-Principal(config-if)# no shutdown
SW-Principal(config-if)# exit

! Guardar cambios en la memoria NVRAM
SW-Principal# copy running-config startup-config

```

* **Descripción breve:** Activa el modo de administración, asigna un identificador único (`SW-Principal`), define un mensaje de seguridad del sistema y asigna una IP lógica a la interfaz virtual para permitir la gestión remota del Switch.

---

### 2. Configuración de Interfaz en un Router (2911)

```cisco
! Configuración de la interfaz GigabitEthernet conectada a la LAN
Router> enable
Router# configure terminal
Router(config)# hostname RTR-Gateway

! Configuración de la puerta de enlace (Default Gateway)
RTR-Gateway(config)# interface GigabitEthernet0/0/0
RTR-Gateway(config-if)# ip address 192.168.1.1 255.255.255.0
RTR-Gateway(config-if)# description Interfaz conectada a LAN Principal
RTR-Gateway(config-if)# no shutdown
RTR-Gateway(config-if)# exit

```

* **Descripción breve:** Configura la interfaz física del Router con la dirección IP `192.168.1.1` (puerta de enlace predeterminada) y ejecuta `no shutdown` para encender físicamente el puerto (los puertos del router vienen apagados por defecto).

---

### 3. Configuración de Parámetros IP en la Terminal del Host (PC)

Aunque en Packet Tracer las PCs se configuran visualmente en la pestaña **Desktop > IP Configuration**, el equivalente por comandos ejecutados dentro del **Command Prompt** de la PC es:

```cmd
! Verificación de configuración IP actual
C:\> ipconfig /all

! Prueba de conectividad con la puerta de enlace
C:\> ping 192.168.1.1

! Rastreo de ruta de datos hacia un nodo destino
C:\> tracert 192.168.1.1

```

* **Descripción breve:** Diagnostica la red interna enviando paquetes ICMP (`ping`) y verificando los saltos que realiza el paquete (`tracert`) para confirmar conectividad extremo a extremo.

---

## Parte 3: Guía de la Práctica Guiada (Paso a Paso)

1. **Diseño de la Topología Física:** Colocación de nodos en el lienzo de Packet Tracer.
1. Arrastra desde el panel inferior 2 PCs (`PC-0` y `PC-1`).
2. Arrastra 1 Switch modelo `2960`.
3. Arrastra 1 Router modelo `2911`.


2. **Cableado e Interconexión:** Uso de cables directos Ethernet.
1. Conecta `PC-0` (FastEthernet0) al `Switch` (FastEthernet0/1).
2. Conecta `PC-1` (FastEthernet0) al `Switch` (FastEthernet0/2).
3. Conecta el `Switch` (GigabitEthernet0/1) al `Router` (GigabitEthernet0/0/0).


3. **Direccionamiento IP de las Workstations:** Asignación lógica en la subred 192.168.1.0/24.
1. Abre `PC-0` -> **Desktop** -> **IP Configuration**:
* **IP Address:** `192.168.1.10`
* **Subnet Mask:** `255.255.255.0`
* **Default Gateway:** `192.168.1.1`


2. Abre `PC-1` -> **Desktop** -> **IP Configuration**:
* **IP Address:** `192.168.1.11`
* **Subnet Mask:** `255.255.255.0`
* **Default Gateway:** `192.168.1.1`




4. **Prueba de Conectividad y Modos de Simulación:** Validación en tiempo real y flujo de datos.
1. Entra al **Command Prompt** de `PC-0` y ejecuta: `ping 192.168.1.11` para probar enlace entre hosts.
2. Cambia al modo **Simulation** en la esquina inferior derecha.
3. Envía una PDU de prueba entre `PC-0` y `PC-1` y observa cómo el Switch conmuta las tramas analizando el encabezado Ethernet.
