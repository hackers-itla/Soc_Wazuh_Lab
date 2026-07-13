# Configuración de Kali Linux para pruebas del IPS

## Estado

> Completado

## Objetivo

Configurar Kali Linux como máquina de pruebas de seguridad dentro del laboratorio SOC.

Kali será utilizado para generar tráfico controlado, realizar pruebas de conectividad, ejecutar análisis de red y validar el funcionamiento de Suricata en modo IPS.

Para realizar las pruebas de forma segura, Kali fue colocado en una red independiente denominada `KALI_TEST`.

Esta separación permite que el tráfico generado desde Kali tenga que atravesar OPNsense antes de alcanzar los equipos de la LAN.

---

## Información de red del laboratorio

| Componente | Dirección IP |
|---|---:|
| Red LAN | `192.168.126.0/24` |
| OPNsense - LAN | `192.168.126.254/24` |
| Wazuh Server | `192.168.126.10/24` |
| Windows Server | `192.168.126.20/24` |
| Windows 10 | `192.168.126.30/24` |
| Red KALI_TEST | `192.168.127.0/24` |
| OPNsense - KALI_TEST | `192.168.127.254/24` |
| Kali Linux | `192.168.127.10/24` |

---

# 1. Arquitectura utilizada para la prueba

Para validar el funcionamiento del IPS se separó Kali Linux de la red LAN.

La arquitectura utilizada fue:

```text
Kali Linux
192.168.127.10
     |
     v
Red KALI_TEST
192.168.127.0/24
     |
     v
OPNsense
KALI_TEST: 192.168.127.254
LAN: 192.168.126.254
     |
     v
Suricata IPS
     |
     v
Red LAN
192.168.126.0/24
     |
     v
WIN10-SOC
192.168.126.30
