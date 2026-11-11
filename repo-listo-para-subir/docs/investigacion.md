# Investigación de la solución

## 1. ¿Qué es Nginx?

Nginx es un servidor web y proxy inverso de código abierto. En este proyecto se usa como servidor web de contenido estático: recibe peticiones HTTP y responde con los archivos (HTML, CSS, JavaScript, imágenes) de la plantilla publicada. En la imagen oficial, el contenido se sirve por defecto desde la carpeta `/usr/share/nginx/html`.

## 2. ¿Qué es Alpine Linux y por qué `nginx:alpine`?

Alpine Linux es una distribución mínima, orientada a la seguridad y al bajo consumo de recursos. La variante `nginx:alpine` de la imagen oficial está construida sobre Alpine, por lo que es mucho más liviana que la versión basada en una distribución completa. Esto reduce el espacio en disco, el tiempo de descarga y la superficie de ataque.

## 3. ¿Por qué usar Docker?

- **Aislamiento:** Nginx corre dentro de un contenedor, sin instalar el servidor directamente en el sistema.
- **Reproducibilidad:** con la misma imagen y el mismo comando (o `docker-compose.yml`), otro equipo obtiene el mismo resultado.
- **Facilidad de cambio:** si algo falla, se elimina el contenedor y se vuelve a crear sin afectar el sistema.

## 4. Conceptos clave que usa el proyecto

| Concepto | Qué hace en este proyecto |
|---|---|
| Imagen (`nginx:alpine`) | Plantilla de solo lectura desde la que se crea el contenedor |
| Contenedor | Instancia en ejecución de la imagen |
| Volumen (bind mount) | Comparte la carpeta de la plantilla del equipo con el contenedor, en `/usr/share/nginx/html` |
| Publicación de puertos (`-p 8080:80`) | Expone el puerto 80 del contenedor como el puerto 8080 del equipo, para acceder desde otros dispositivos |
| Modo solo lectura (`:ro`) | El contenedor no puede modificar los archivos del sitio |

## 5. Requisitos de la actividad (resumen)

- Clonar un repositorio con una página o plantilla **ya construida** (no desarrollar un sitio desde cero).
- Publicarla mediante un contenedor Nginx.
- Comprobar el servicio **desde otro equipo o cliente externo**; no basta con mostrar que el contenedor está corriendo.

## 6. Alternativas consideradas

*Pendiente de completar por el analista e investigador.* Ejemplos de puntos a comparar: ejecutar con `docker run` o con `docker compose`; usar un bind mount o copiar los archivos a una imagen propia con un `Dockerfile`.

## 7. Documentación técnica consultada

| Fuente | Enlace | Fecha de consulta |
|---|---|---|
| Imagen oficial de Nginx en Docker Hub | https://hub.docker.com/_/nginx | [Completar] |
| Documentación de Nginx | https://nginx.org/en/docs/ | [Completar] |
| Documentación de Docker | https://docs.docker.com/ | [Completar] |
| Wiki de Alpine Linux | https://wiki.alpinelinux.org/ | [Completar] |
| Plantilla web utilizada | [Completar] | [Completar] |
