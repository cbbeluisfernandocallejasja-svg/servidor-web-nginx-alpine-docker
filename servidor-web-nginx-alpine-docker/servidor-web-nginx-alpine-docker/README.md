# Servidor web con Nginx (Alpine Linux + Docker)

Misión: Implementación y Recuperación de Servicios Tecnológicos
Materia: Sistemas Operativos | Docente: Ing. Diego Cárdenas | Universidad Franz Tamayo

Este repositorio documenta cómo nuestra cuadrilla instala, configura, prueba y entrega un **servidor web Nginx** que publica una **plantilla web ya construida** (no se desarrolló ningún sitio desde cero), usando **Alpine Linux** y **Docker**.

> **Estado del repositorio:** en construcción. Las secciones marcadas como *Pendiente* se completan a medida que el equipo ejecuta y verifica cada paso. Ver [Estado actual y tareas pendientes](#9-conclusiones-estado-actual-y-tareas-pendientes).

---

## Índice

1. [Integrantes y roles](#1-integrantes-y-roles)
2. [Proyecto seleccionado y objetivo](#2-proyecto-seleccionado-y-objetivo)
3. [Investigación de la solución](#3-investigación-de-la-solución)
4. [Estado inicial y requerimientos](#4-estado-inicial-y-requerimientos)
5. [Procedimiento de instalación, configuración y puesta en marcha](#5-procedimiento-de-instalación-configuración-y-puesta-en-marcha)
6. [Pruebas y evidencias](#6-pruebas-y-evidencias)
7. [Bitácora, problemas y soluciones](#7-bitácora-problemas-y-soluciones)
8. [Uso de IA (Prompt Espejo) y ActivityWatch](#8-uso-de-ia-prompt-espejo-y-activitywatch)
9. [Conclusiones, estado actual y tareas pendientes](#9-conclusiones-estado-actual-y-tareas-pendientes)

---

## 1. Integrantes y roles

| Rol | Responsabilidad | Integrante |
|---|---|---|
| Coordinador | Organiza las tareas, distribuye el trabajo y controla los tiempos | [Completar nombre completo] |
| Operador técnico | Ejecuta instalaciones, configuraciones y pruebas | [Completar nombre completo] |
| Analista e investigador | Investiga alternativas y contrasta las recomendaciones técnicas | [Completar nombre completo] |
| Documentación y evidencias | Registra procedimientos, configuraciones y resultados en GitHub | Luis Fernando Callejas Jaime |
| Continuidad y relevo | Mantiene la bitácora, registra avances y deja claras las tareas pendientes | [Completar nombre completo] |

---

## 2. Proyecto seleccionado y objetivo

- **Proyecto:** Servidor web, Nginx con sitio prediseñado (Alpine Linux + Docker).
- **Imagen utilizada:** `nginx:alpine`
- **Validación del docente:** [Completar: fecha y forma de validación]

**Objetivo:** clonar un repositorio que contenga una página o plantilla web ya construida y publicarla mediante un contenedor Nginx, comprobando su funcionamiento **desde otro equipo o dispositivo** de la red, y dejando todo documentado para que otro equipo pueda reproducirlo.

**Plantilla utilizada:** [Completar: nombre y enlace del repositorio de la plantilla clonada]

---

## 3. Investigación de la solución

Resumen breve. El detalle está en [`docs/investigacion.md`](docs/investigacion.md).

- **Nginx** es un servidor web que entrega archivos estáticos (HTML, CSS, JS, imágenes) y también puede funcionar como proxy inverso.
- **Alpine Linux** es una distribución muy liviana, por eso la imagen `nginx:alpine` ocupa poco espacio.
- **Docker** permite ejecutar Nginx aislado en un contenedor. La carpeta del sitio se comparte con el contenedor mediante un volumen y el puerto del contenedor se publica hacia el equipo anfitrión.

---

## 4. Estado inicial y requerimientos

**Requerimientos identificados**

- Equipo con Alpine Linux y acceso a internet.
- Docker instalado y servicio activo.
- Git instalado para clonar la plantilla.
- Una plantilla web estática ya construida.
- Un puerto libre en el equipo (se propone el 8080).
- Un segundo equipo o dispositivo en la misma red para la prueba externa.

**Estado inicial del equipo** (versión de Alpine, IP, si Docker y Git estaban instalados): *Pendiente de completar con los datos del operador técnico.*

---

## 5. Procedimiento de instalación, configuración y puesta en marcha

> **Procedimiento previsto.** Se confirmará y corregirá con los comandos que el operador técnico ejecute realmente. Cada paso real tendrá su captura en [`evidencias/`](evidencias/).

1. Verificar el repositorio de paquetes (Docker está en el repositorio `community` de Alpine; si está comentado en `/etc/apk/repositories`, habilitarlo) y actualizar con `apk update`.
2. Instalar Docker y Git: `apk add docker git`.
3. Activar Docker al arranque e iniciarlo: `rc-update add docker boot` y `service docker start`.
4. Comprobar la instalación: `docker --version`.
5. Clonar la plantilla en una carpeta de trabajo: `git clone <URL_de_la_plantilla> sitio`.
6. Levantar Nginx con la plantilla (ver [`configuracion/`](configuracion/)).
7. Comprobar el contenedor con `docker ps`.
8. Probar el sitio en el mismo equipo y **desde otro dispositivo** con `http://<IP_del_servidor>:8080`.

Los archivos y comandos de configuración están en [`configuracion/`](configuracion/).

---

## 6. Pruebas y evidencias

Las capturas están en [`evidencias/`](evidencias/). La tabla se actualiza conforme llegan las evidencias.

| ID | Evidencia | Estado |
|---|---|---|
| EVID-01 | Estado inicial del equipo (versión de Alpine y dirección IP) | Pendiente |
| EVID-02 | Docker instalado (`docker --version`) | Pendiente |
| EVID-03 | Plantilla clonada con `git clone` | Pendiente |
| EVID-04 | Contenedor Nginx en ejecución (`docker ps`) | Pendiente |
| EVID-05 | Sitio abierto en el navegador del mismo equipo | Pendiente |
| EVID-06 | **Sitio abierto desde otro dispositivo** (con la IP visible) | Pendiente |
| EVID-07 | Archivos de configuración utilizados | Pendiente |
| EVID-08 | Prueba tras reiniciar el contenedor | Pendiente |

---

## 7. Bitácora, problemas y soluciones

La bitácora completa está en [`docs/bitacora.md`](docs/bitacora.md).

---

## 8. Uso de IA (Prompt Espejo) y ActivityWatch

- **Uso de IA:** una sola conversación por cuadrilla con el prompt entregado por el docente. Las consultas relevantes y la decisión del equipo sobre cada recomendación (aceptada, modificada o descartada) están en [`docs/uso-ia.md`](docs/uso-ia.md).
- **ActivityWatch:** el reporte exportado, el periodo registrado y sus limitaciones estarán en [`activitywatch/`](activitywatch/), si la estación asignada lo tiene disponible.

---

## 9. Conclusiones, estado actual y tareas pendientes

**Estado actual:** en construcción. Repositorio creado y estructurado; falta ejecutar y documentar la implementación.

**Tareas pendientes**

- [ ] Completar nombres de los integrantes y la validación del docente.
- [ ] Elegir y registrar la plantilla web a publicar.
- [ ] Instalar y configurar Docker y Nginx en Alpine Linux.
- [ ] Publicar la plantilla y probarla en el mismo equipo.
- [ ] Probar el sitio desde otro dispositivo y capturarlo.
- [ ] Registrar los comandos y archivos reales utilizados.
- [ ] Completar la bitácora, los problemas encontrados y sus soluciones.
- [ ] Completar el uso de IA y el reporte de ActivityWatch.
- [ ] Redactar las conclusiones finales.
- [ ] Revisar que no haya contraseñas ni datos sensibles y entregar el enlace en Moodle.

**Conclusión final:** pendiente de completar al terminar las pruebas.

---

> **Importante:** este repositorio no contiene contraseñas, tokens, claves privadas ni datos personales o sensibles. Los archivos de configuración usan valores de ejemplo.
