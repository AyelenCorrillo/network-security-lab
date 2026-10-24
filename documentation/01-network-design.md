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