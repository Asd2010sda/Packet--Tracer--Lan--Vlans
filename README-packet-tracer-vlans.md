# Packet Tracer — LAN con VLANs (Segmentación IT/OT)

Topología de red básica construida en Cisco Packet Tracer, aplicando segmentación por VLANs como base para separación de redes IT y OT.

## Objetivo

Construir y verificar una red LAN funcional, y demostrar cómo las VLANs permiten aislar tráfico entre dispositivos aunque compartan el mismo switch físico — el mecanismo base detrás de la separación de redes corporativas (IT) e industriales (OT).

## Herramientas usadas

- **Cisco Packet Tracer 9.0** — simulador de red
- **CLI de Cisco IOS** — configuración por línea de comandos

## Qué se hizo

### 1. Topología física
- 2 PCs conectados directamente (cable cruzado) — primera prueba de conectividad
- Incorporación de un switch Cisco 2960 para conectar ambos PCs (cable recto PC–switch)
- Verificación de conectividad con `ping` en ambos escenarios

### 2. Direccionamiento IP y subnetting
- Asignación de IPs dentro del mismo rango (`192.168.1.x`, máscara `255.255.255.0` / `/24`)
- Repaso conceptual de subnetting: representación binaria de una IP, cómo se construye una máscara de red, y por qué solo ciertos valores (0, 128, 192, 224, 240, 248, 252, 254, 255) son válidos en una máscara

### 3. Configuración de VLANs por CLI
- Creación de VLAN 10 (`IT_Zone`) y asignación al puerto Fa0/1
- Creación de VLAN 20 (`OT_Zone`) y asignación al puerto Fa0/2
- Verificación de configuración con `show vlan brief`

### 4. Prueba de aislamiento
- Ping entre PC0 (VLAN 10) y PC1 (VLAN 20): **falla** (Request timed out)
- Esto confirma que, aunque ambos dispositivos comparten el mismo switch físico y el mismo rango de IP, las VLANs los aíslan completamente a nivel lógico

## Comandos clave utilizados

```
enable
configure terminal
vlan 10
 name IT_Zone
exit
interface fastEthernet 0/1
 switchport mode access
 switchport access vlan 10
exit
show vlan brief
```

## Hallazgo clave

Las VLANs permiten segmentar una red sin necesidad de hardware adicional ni cambios en el cableado físico. Este es el mecanismo fundamental detrás del modelo Purdue en entornos industriales, donde se separan las redes de oficina (IT) de las redes de control industrial (OT) para reducir superficie de ataque y contener posibles incidentes de seguridad.

## Próximos pasos

- Agregar un router para permitir comunicación controlada entre VLANs (inter-VLAN routing)
- Aplicar ACLs para restringir qué tráfico específico puede cruzar entre IT_Zone y OT_Zone
- Migrar esta topología a GNS3 para mayor realismo, incorporando un PLC simulado (OpenPLC) en la zona OT

---

*Parte de un path de especialización en ICA (Instrumentación, Control y Automatización) + redes + ciberseguridad industrial.*
