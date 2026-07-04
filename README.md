# Laboratorio SOC con Wazuh

![Estado](https://img.shields.io/badge/Estado-En%20construcción-yellow)
![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-blue)
![Suricata](https://img.shields.io/badge/IDS%2FIPS-Suricata-red)

## Descripción

Este proyecto documenta la implementación de un laboratorio SOC virtualizado
para monitoreo, detección y análisis de eventos de seguridad.

El laboratorio utiliza Wazuh como plataforma SIEM/XDR, Windows Server y
Windows 10 como endpoints, y OPNsense con Suricata como IDS/IPS.

## Objetivos

- Instalar Wazuh en Ubuntu Server.
- Registrar Windows Server y Windows 10 como agentes.
- Instalar Sysmon en los endpoints Windows.
- Implementar OPNsense como firewall.
- Configurar Suricata como IDS/IPS.
- Centralizar y analizar eventos de seguridad.
- Realizar pruebas controladas para validar las detecciones.
- Documentar alertas, evidencias y resultados.

## Arquitectura del laboratorio

| Máquina virtual | Sistema operativo | Función |
|---|---|---|
| Wazuh Server | Ubuntu Server | Manager, Indexer y Dashboard |
| Firewall | OPNsense | Firewall, router y Suricata IDS/IPS |
| Servidor | Windows Server | Active Directory, DNS y endpoint monitoreado |
| Cliente | Windows 10 | Estación de trabajo monitoreada |
| Atacante | Kali Linux | Generación de tráfico y pruebas controladas |

## Topología

La topología del laboratorio estará compuesta por una red externa y una red
interna protegida por OPNsense.

```text
Internet / NAT
      |
   OPNsense
 Suricata IDS/IPS
      |
 Red interna del laboratorio
      |
 -------------------------------------------------
 |                 |                |             |
Ubuntu Server  Windows Server   Windows 10    Kali Linux
   Wazuh          Agente           Agente       Pruebas
