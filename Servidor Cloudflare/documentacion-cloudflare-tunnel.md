# Publicación del servidor web en Internet mediante Cloudflare Tunnel

## 1. Objetivo

Publicar temporalmente en Internet el simulador web que ya se encontraba funcionando en el servidor Nginx dentro de la máquina virtual Alpine Linux. Para ello se utilizó **Cloudflare Quick Tunnel**, ejecutado como un contenedor Docker.

Esta etapa complementa la publicación local realizada anteriormente. No incluye la instalación de Alpine Linux, Docker o Nginx, ni la copia de los archivos del proyecto al contenedor web.

## 2. Arquitectura utilizada

El acceso público sigue este recorrido:

1. El usuario abre el enlace HTTPS temporal generado por Cloudflare.
2. La solicitud llega a la infraestructura de Cloudflare.
3. Cloudflare Tunnel mantiene una conexión saliente desde el servidor Alpine hacia Cloudflare.
4. `cloudflared` reenvía la solicitud al servicio local `http://127.0.0.1:8080`.
5. Docker entrega la solicitud al contenedor `mi-servidor-web`, donde Nginx sirve los archivos del simulador.

**Contenedores involucrados:**

| Contenedor | Función |
|---|---|
| `mi-servidor-web` | Ejecuta Nginx y sirve el simulador web por el puerto 8080 del servidor. |
| `cloudflare-tunnel` | Establece el túnel que permite acceder al servidor desde Internet. |
| `twingate-docker` | Mantiene el acceso remoto privado utilizado para la administración del servidor. |

## 3. Creación del túnel temporal

Se creó el contenedor de Cloudflare con el siguiente comando:

```bash
docker run -d --name cloudflare-tunnel --restart unless-stopped --network host cloudflare/cloudflared:latest tunnel --no-autoupdate --url http://127.0.0.1:8080
```

### Explicación de los parámetros

- `docker run`: crea y ejecuta un contenedor.
- `-d`: ejecuta el contenedor en segundo plano.
- `--name cloudflare-tunnel`: asigna un nombre identificable al contenedor.
- `--restart unless-stopped`: solicita a Docker que reinicie el contenedor automáticamente, excepto si fue detenido manualmente.
- `--network host`: utiliza la red del servidor Alpine. Así, `cloudflared` puede acceder al servicio local mediante `127.0.0.1:8080`.
- `cloudflare/cloudflared:latest`: imagen oficial utilizada para ejecutar Cloudflare Tunnel.
- `tunnel --no-autoupdate`: inicia el túnel sin ejecutar el mecanismo de actualización automática del binario.
- `--url http://127.0.0.1:8080`: indica el servicio local al que se enviará el tráfico recibido por el túnel.

Al utilizar Quick Tunnel no fue necesario configurar una cuenta de Cloudflare ni disponer de un dominio propio para esta prueba.

## 4. Obtención del enlace público

Para consultar los registros del contenedor se ejecutó:

```bash
docker logs cloudflare-tunnel
```

En la salida se buscó el mensaje `Your quick Tunnel has been created!`, que muestra la dirección pública temporal terminada en `trycloudflare.com`.

En la ejecución documentada se generó el siguiente enlace:

```text
https://karen-wonderful-matters-bios.trycloudflare.com
```

Este enlace corresponde a la sesión de prueba registrada el 11 de noviembre de 2026. Puede dejar de funcionar cuando el túnel se detenga o reinicie; por tanto, no debe considerarse una dirección permanente.

## 5. Verificación de los servicios

Se comprobó el estado de los contenedores con:

```bash
docker ps
```

En la verificación se observaron los siguientes contenedores en ejecución:

- `cloudflare-tunnel`
- `mi-servidor-web`
- `twingate-docker`

También se consultaron los registros de Cloudflare. El mensaje `Registered tunnel connection` confirmó que el túnel había establecido una conexión con la red de Cloudflare.

La prueba final de funcionamiento consiste en abrir el enlace `trycloudflare.com` desde un dispositivo externo, preferiblemente utilizando datos móviles en lugar de la red local, y comprobar que se muestra el simulador.

## 6. Reinicio automático de los contenedores

Para que Nginx se recupere automáticamente cuando Docker vuelva a iniciarse, se configuró previamente la política de reinicio del contenedor web:

```bash
docker update --restart unless-stopped mi-servidor-web
```

La política se verificó con:

```bash
docker inspect -f '{{.HostConfig.RestartPolicy.Name}}' mi-servidor-web
```

El resultado confirmado fue:

```text
unless-stopped
```

El contenedor de Cloudflare se creó directamente con esa misma política mediante `--restart unless-stopped`.

Además, se verificó que Docker estuviera iniciado y habilitado en el arranque de Alpine Linux:

```bash
rc-service docker status
rc-update show | grep docker
```

Los resultados confirmados fueron `status: started` y `docker | default`.

Estas configuraciones permiten que Docker inicie con Alpine y que los contenedores se recuperen automáticamente, siempre que no hayan sido detenidos manualmente.

## 7. Procedimiento de recuperación después de una interrupción

Si el servidor se apaga y posteriormente vuelve a estar disponible, el encargado de la recuperación debe:

1. Comprobar que la máquina virtual Alpine Linux esté encendida.
2. Comprobar que Docker esté funcionando:

   ```bash
   rc-service docker status
   ```

3. Comprobar los contenedores activos:

   ```bash
   docker ps
   ```

4. Si alguno de los contenedores no está ejecutándose, iniciarlo manualmente:

   ```bash
   docker start mi-servidor-web
   docker start cloudflare-tunnel
   ```

5. Consultar los registros del túnel para encontrar el enlace público vigente:

   ```bash
   docker logs cloudflare-tunnel
   ```

6. Identificar la dirección más reciente que termina en `trycloudflare.com`.
7. Compartir el nuevo enlace con el docente y los usuarios.

**Importante:** Quick Tunnel genera una dirección temporal. Después de reiniciar el túnel puede generarse una dirección diferente. Por eso, no basta con conservar el enlace anterior: el encargado debe consultar los registros y comunicar la dirección actualizada.

La recuperación de la máquina virtual también depende de que el servidor físico Proxmox esté encendido y de que la máquina virtual pueda iniciarse. La configuración de encendido automático de la VM, si se requiere, corresponde al administrador de Proxmox.

## 8. Consideraciones y limitaciones

- Quick Tunnel está destinado a pruebas y experimentación, no ofrece una garantía de disponibilidad.
- El enlace `trycloudflare.com` es temporal y puede cambiar cuando se crea un nuevo túnel.
- No se configuró un dominio propio ni un túnel permanente asociado a una cuenta.
- El enlace debe compartirse nuevamente cuando cambie.
- El túnel necesita que el contenedor Nginx esté activo y que el servicio local responda en `127.0.0.1:8080`.
- Las advertencias relacionadas con el proxy ICMP que aparecieron en los registros no impidieron establecer la conexión HTTP del túnel. La confirmación relevante fue `Registered tunnel connection` y la prueba de acceso al simulador.

## 9. Evidencias

Añadir las capturas de pantalla tomadas durante la práctica en los siguientes espacios:

1. **Creación del contenedor:** salida del comando `docker run` que crea `cloudflare-tunnel`.
2. **Enlace generado:** registros de `docker logs cloudflare-tunnel` donde aparece la dirección `trycloudflare.com`.
3. **Contenedores activos:** salida de `docker ps` mostrando Nginx, Cloudflare y Twingate.
4. **Política de reinicio:** resultado de `docker inspect` mostrando `unless-stopped`.
5. **Acceso público:** navegador mostrando el simulador mediante el enlace de Cloudflare.

## 10. Conclusión

Se consiguió publicar temporalmente en Internet el simulador web que ya estaba alojado en Nginx dentro de Alpine Linux. Cloudflare Tunnel permitió realizar esta publicación sin abrir puertos entrantes en el router ni contratar un dominio propio. También se configuraron políticas de reinicio automático para los contenedores y se documentó el procedimiento para recuperar el servicio y compartir el nuevo enlace cuando sea necesario.
