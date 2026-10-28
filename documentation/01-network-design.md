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


