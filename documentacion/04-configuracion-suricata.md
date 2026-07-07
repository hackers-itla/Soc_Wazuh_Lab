---

## 21. Preparación del archivo de documentación

Se abrió el archivo `04-configuracion-suricata.md`, ubicado dentro de la carpeta `documentacion` del repositorio.

Este archivo se utiliza para registrar el proceso completo de configuración de Suricata como IPS en OPNsense.

![Archivo de configuración de Suricata en GitHub](../imagenes/91-archivo-configuracion-suricata-en-github.png)

Posteriormente, se abrió el editor de GitHub para agregar las evidencias, explicaciones y configuraciones realizadas durante el laboratorio.

![Edición de la documentación de Suricata](../imagenes/92-edicion-documentacion-configuracion-suricata.png)

---

## 22. Revisión de categorías ET Open

Además de las listas de indicadores proporcionadas por `abuse.ch`, se revisaron las categorías disponibles de ET Open.

ET Open contiene firmas para detectar diferentes tipos de actividad maliciosa, como:

- Comunicación con botnets.
- Intentos de explotación.
- Malware.
- Phishing.
- Escaneos de red.
- Ataques contra servicios web.
- Ejecución de shellcode.
- Actividad relacionada con servidores de comando y control.

Las categorías fueron seleccionadas de forma controlada para evitar habilitar reglas innecesarias y reducir la posibilidad de falsos positivos.

### Primera parte del listado

En la primera parte se revisaron categorías relacionadas con botnets, respuestas de ataques, eventos recientes y tráfico DNS sospechoso.

![Primera parte del listado de categorías ET Open](../imagenes/93-listado-categorias-et-open-parte-1.png)

Entre las categorías seleccionadas se encuentran:

```text
ET open/botcc.portgrouped
ET open/emerging-attack_response
ET open/emerging-current_events
ET open/emerging-dns
```

Se utilizó `botcc.portgrouped` en lugar de habilitar simultáneamente `botcc`, con el objetivo de evitar reglas duplicadas relacionadas con comunicaciones de botnets.

---

## 23. Categorías de explotación y malware

En la segunda parte del listado se revisaron categorías relacionadas con explotación de vulnerabilidades, malware y actividad sospechosa en diferentes protocolos.

![Segunda parte del listado de categorías ET Open](../imagenes/94-listado-categorias-et-open-parte-2.png)

Entre las categorías seleccionadas se encuentran:

```text
ET open/emerging-exploit
ET open/emerging-malware
```

La categoría `emerging-exploit` permite detectar intentos de explotación contra sistemas y servicios vulnerables.

La categoría `emerging-malware` contiene firmas relacionadas con comunicaciones, descargas y comportamientos asociados con diferentes familias de malware.

---

## 24. Categorías de phishing, escaneo y shellcode

En la tercera parte del listado se revisaron las categorías relacionadas con phishing, reconocimiento de red, escaneos y ejecución de código malicioso.

![Tercera parte del listado de categorías ET Open](../imagenes/95-listado-categorias-et-open-parte-3.png)

Entre las categorías seleccionadas se encuentran:

```text
ET open/emerging-phishing
ET open/emerging-scan
ET open/emerging-shellcode
```

Estas categorías permiten detectar:

- Enlaces y comunicaciones relacionadas con phishing.
- Escaneos de puertos y reconocimiento de red.
- Intentos de ejecución o transferencia de shellcode.
- Actividad previa a posibles intentos de explotación.

---

## 25. Categorías de ataques web

En la última parte del listado se revisaron las reglas relacionadas con clientes web, servidores web y aplicaciones web específicas.

![Cuarta parte del listado de categorías ET Open](../imagenes/96-listado-categorias-et-open-parte-4.png)

Entre las categorías seleccionadas se encuentran:

```text
ET open/emerging-web_client
ET open/emerging-web_server
ET open/emerging-web_specific_apps
```

Estas reglas permiten detectar actividad como:

- Intentos de explotación contra servidores web.
- Solicitudes HTTP sospechosas.
- Ataques contra aplicaciones web.
- Inyección de comandos.
- Intentos de explotación contra navegadores y clientes web.
- Patrones relacionados con SQL Injection y otras vulnerabilidades.

---

## 26. Instalación y habilitación de las categorías ET Open

Después de seleccionar las categorías necesarias, se utilizó la opción para habilitar los conjuntos de reglas seleccionados.

Posteriormente, se presionó el botón:

```text
Download & Update Rules
```

La columna **Last updated** mostró la fecha y hora de actualización, mientras que la columna **Enabled** mostró una marca de verificación para los conjuntos habilitados.

![Categorías ET Open instaladas y habilitadas](../imagenes/97-categorias-et-open-instaladas-y-habilitadas.png)

Las principales categorías instaladas fueron:

```text
ET open/botcc.portgrouped
ET open/emerging-attack_response
ET open/emerging-current_events
ET open/emerging-dns
ET open/emerging-exploit
ET open/emerging-malware
ET open/emerging-phishing
ET open/emerging-scan
ET open/emerging-shellcode
ET open/emerging-web_client
ET open/emerging-web_server
ET open/emerging-web_specific_apps
```

No se habilitaron inicialmente todas las categorías disponibles, ya que algunas pueden generar una gran cantidad de eventos, falsos positivos o un consumo innecesario de recursos.

Entre las categorías que se dejaron deshabilitadas se encuentran:

```text
ET open/emerging-games
ET open/emerging-chat
ET open/emerging-p2p
ET open/emerging-inappropriate
ET open/emerging-info
ET open/emerging-hunting
ET open/emerging-policy
```

---

## 27. Verificación de las reglas ET Open

Se accedió a la pestaña **Rules** para confirmar que las reglas pertenecientes a las categorías ET Open fueran importadas correctamente.

En la columna **Source** se observaron reglas procedentes de conjuntos como:

```text
emerging-exploit.rules
emerging-web_server.rules
emerging-malware.rules
```

La columna **Action** mostró el valor:

```text
alert
```

![Verificación de reglas ET Open en modo alerta](../imagenes/98-verificacion-reglas-et-open-en-modo-alerta.png)

Las reglas ET Open se mantuvieron inicialmente con la acción `Alert`.

Esto significa que Suricata detectará y registrará el tráfico que coincida con estas firmas, pero no lo bloqueará automáticamente.

La estrategia aplicada quedó de la siguiente manera:

| Fuente de reglas | Acción |
|---|---|
| Abuse.ch | `Drop` |
| ET Open | `Alert` |
| Regla personalizada de prueba | `Drop`, posteriormente deshabilitada |

Las reglas de `abuse.ch` bloquean comunicaciones relacionadas con indicadores maliciosos conocidos.

Las reglas ET Open permanecen en modo de alerta para permitir su análisis antes de cambiar firmas específicas a la acción `Drop`.

No todas las firmas individuales aparecen necesariamente habilitadas dentro de la pestaña **Rules**, ya que cada conjunto puede incluir reglas activas y reglas desactivadas por defecto. Sin embargo, los conjuntos seleccionados quedaron instalados y disponibles para Suricata.

---

## Resultado final

Suricata quedó configurado correctamente como IPS en OPNsense.

El sistema quedó preparado para:

- Inspeccionar el tráfico de la interfaz LAN.
- Detectar tráfico relacionado con indicadores de compromiso.
- Utilizar reglas externas proporcionadas por `abuse.ch`.
- Bloquear indicadores maliciosos conocidos mediante la acción `Drop`.
- Utilizar categorías de ET Open para detectar exploits, malware, phishing y escaneos.
- Mantener las reglas ET Open inicialmente con la acción `Alert`.
- Convertir selectivamente reglas de alerta en reglas de bloqueo.
- Crear reglas personalizadas por dirección IP.
- Bloquear tráfico en tiempo real.
- Registrar los paquetes bloqueados en la sección de alertas.
- Restaurar la conectividad al desactivar una regla.
- Ampliar posteriormente la configuración mediante nuevas políticas de detección y bloqueo.

La estrategia final quedó configurada de la siguiente manera:

```text
Abuse.ch → Drop
ET Open → Alert
Reglas personalizadas → Según la prueba o necesidad
```

Esta configuración permite que el laboratorio detecte una amplia variedad de actividades sospechosas mientras limita el bloqueo automático a indicadores considerados de alta confianza.
