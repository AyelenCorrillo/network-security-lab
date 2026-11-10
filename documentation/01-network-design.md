# Fase 1.3 — Construcción y verificación de la topología

## Objetivo
Construir una topología de red virtual y verificar el estado operativo de sus enlaces físicos.

## Dispositivos utilizados
- 2 computadoras: PC-A y PC-B.
- 2 switches: SW1 y SW2.
- 1 router Cisco 1941: R1.

## Topología
PC-A → SW1 → R1 → SW2 → PC-B

## Problema detectado
Las interfaces GigabitEthernet0/0 y GigabitEthernet0/1 del router estaban administrativamente deshabilitadas.

## Diagnóstico
Se utilizó `show ip interface brief` para inspeccionar el estado de las interfaces.

## Corrección aplicada
Se habilitaron ambas interfaces mediante `no shutdown`.

## Verificación final
- GigabitEthernet0/0: up/up.
- GigabitEthernet0/1: up/up.
- Los cuatro enlaces de la topología aparecen verdes en Packet Tracer.
- El archivo `network-topology.pkt` se guardó correctamente.

## Evidencias
- `evidence/screenshots/01-topology-connected.png`
- `evidence/screenshots/02-router-interfaces-up.png`

## Resultado
Topología construida y enlaces verificados visualmente. La conectividad entre redes mediante el router todavía no se ha probado, ya que la configuración IPv4 está pendiente.

## Alcance
No se configuraron direcciones IPv4, gateways, DNS ni servicios de red.

---

## Fase 2 — Configuración de direccionamiento IPv4

Se configuraron las direcciones IPv4 de las dos redes locales y las interfaces del router R1.

### Plan de direccionamiento

| Dispositivo | Interfaz | Dirección IPv4 | Máscara | Gateway predeterminado |
|---|---|---|---|---|
| R1 | GigabitEthernet0/0 | 192.168.10.1 | 255.255.255.0 | — |
| PC-A | FastEthernet0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| R1 | GigabitEthernet0/1 | 192.168.20.1 | 255.255.255.0 | — |
| PC-B | FastEthernet0 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |

### Redes utilizadas

- **LAN 1:** `192.168.10.0/24`
- **LAN 2:** `192.168.20.0/24`

Las interfaces del router se encuentran activas (`up/up`).

## Fase 3 — Pruebas de conectividad

Se realizaron pruebas con el comando `ping` para verificar la conectividad con las puertas de enlace y entre los equipos de ambas redes.

| Prueba | Destino | Paquetes recibidos | Pérdida | Resultado |
|---|---|---|---|---|
| PC-A → Gateway LAN 1 | 192.168.10.1 | 4/4 | 0 % | Correcto |
| PC-B → Gateway LAN 2 | 192.168.20.1 | 4/4 | 0 % | Correcto |
| PC-A → PC-B | 192.168.20.10 | 4/4 | 0 % | Correcto |
| PC-B → PC-A | 192.168.10.10 | 4/4 | 0 % | Correcto |

### Conclusión

Las cuatro pruebas finalizaron correctamente, sin pérdida de paquetes. Se verificó la comunicación entre cada equipo y su gateway, así como la conectividad bidireccional entre las dos redes locales mediante el router R1.

Estos resultados confirman que el direccionamiento IPv4 y el enrutamiento básico funcionan correctamente en el laboratorio de Packet Tracer.

### Evidencia

- `evidence/screenshots/03-connectivity-tests.png`: captura de la prueba de conectividad entre las dos redes.

---

## Fase 4 — Reconocimiento de red

### Objetivo

Identificar los dispositivos de la red mediante tablas ARP y analizar la tabla de enrutamiento del router R1 para comprender cómo se comunican los equipos entre diferentes segmentos.

### 1. Reconocimiento ARP desde PC-A

Se utilizó el comando `arp -a` para consultar las asociaciones entre direcciones IPv4 y direcciones MAC.

| Dirección IPv4 | Dirección MAC | Tipo |
|---|---|---|
| 192.168.10.1 | 0001.6339.7101 | dynamic |

La entrada corresponde a la interfaz GigabitEthernet0/0 del router R1, que funciona como gateway de PC-A.

### 2. Reconocimiento ARP desde PC-B

Se ejecutó `arp -a` en PC-B y se obtuvo el siguiente resultado:

| Dirección IPv4 | Dirección MAC | Tipo |
|---|---|---|
| 192.168.20.1 | 0001.6339.7102 | dynamic |

La entrada corresponde a la interfaz GigabitEthernet0/1 del router R1, que funciona como gateway de PC-B.

### 3. Tabla ARP del router R1

Se utilizó el comando `show ip arp` para identificar las direcciones IP y MAC conocidas por el router.

| Dispositivo | Dirección IPv4 | Dirección MAC | Interfaz |
|---|---|---|---|
| R1 | 192.168.10.1 | 0001.6339.7101 | GigabitEthernet0/0 |
| PC-A | 192.168.10.10 | 0030.A3A6.CAE9 | GigabitEthernet0/0 |
| R1 | 192.168.20.1 | 0001.6339.7102 | GigabitEthernet0/1 |
| PC-B | 192.168.20.10 | 00D0.5866.DAA7 | GigabitEthernet0/1 |

Se comprobó que R1 aprendió dinámicamente las direcciones MAC de ambos equipos después de generar tráfico hacia ellos.

### 4. Análisis de la tabla de enrutamiento

Se utilizó el comando `show ip route` para identificar las redes conocidas por R1.

Se encontraron las siguientes rutas:

| Código | Red o dirección | Interfaz |
|---|---|---|
| C | 192.168.10.0/24 | GigabitEthernet0/0 |
| L | 192.168.10.1/32 | GigabitEthernet0/0 |
| C | 192.168.20.0/24 | GigabitEthernet0/1 |
| L | 192.168.20.1/32 | GigabitEthernet0/1 |

- **C (Connected):** red directamente conectada al router.
- **L (Local):** dirección IP asignada a una interfaz del propio router.
- **Gateway of last resort is not set:** no existe una ruta predeterminada configurada.

R1 conoce ambas redes directamente conectadas y puede encaminar tráfico entre ellas.

### 5. Pruebas de conectividad desde R1

Se ejecutaron pruebas ICMP desde el router hacia ambos equipos.

| Prueba | Destino | Paquetes recibidos | Pérdida |
|---|---|---|---|
| R1 → PC-A | 192.168.10.10 | 4/5 | 20 % |
| R1 → PC-B | 192.168.20.10 | 4/5 | 20 % |

En ambas pruebas, el primer paquete no recibió respuesta y los cuatro siguientes fueron exitosos. Este comportamiento puede producirse durante la resolución ARP inicial.

### 6. Prueba de conectividad entre redes

Desde PC-B se ejecutó `ping 192.168.10.10`.

Resultado: 4 paquetes enviados, 4 recibidos y 0 % de pérdida.

También se comprobó que PC-B conserva en su tabla ARP únicamente la dirección MAC de su gateway, ya que PC-A pertenece a otra subred.

### Conclusión

Se identificaron las direcciones IPv4 y MAC de los dos equipos y las interfaces del router asociadas a cada segmento.

El análisis de las tablas ARP permitió comprender cómo se resuelven las direcciones MAC dentro de una red local. La tabla de enrutamiento confirmó que R1 conoce las dos redes directamente conectadas y puede encaminar tráfico entre ellas.

Estos procedimientos constituyen una base del reconocimiento de infraestructura de red utilizado durante las primeras etapas de una auditoría de seguridad o pentesting.

### Evidencias

- `evidence/screenshots/04-pc-a-arp.png`
- `evidence/screenshots/05-pc-b-arp.png`
- `evidence/screenshots/06-router-arp.png`
- `evidence/screenshots/07-router-routing-table.png`


