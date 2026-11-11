# Configuración y comandos

Aquí van los archivos y comandos necesarios para **reproducir** el despliegue.

> Los archivos de esta carpeta son una **base**. Se ajustarán a lo que el operador técnico realmente ejecute. **No se publican contraseñas ni datos sensibles.**

## Estructura esperada en el servidor

```
~/nginx-proyecto/
├── docker-compose.yml
└── sitio/          <- aquí se clona la plantilla web
```

## Opción A: `docker run`

```bash
docker run -d \
  --name nginx-plantilla \
  -p 8080:80 \
  -v "$(pwd)/sitio:/usr/share/nginx/html:ro" \
  --restart unless-stopped \
  nginx:alpine
```

## Opción B: `docker compose`

Ver [`docker-compose.yml`](docker-compose.yml) y ejecutar:

```bash
docker compose up -d
```

## Comandos de verificación

```bash
docker ps                          # el contenedor debe aparecer como "Up"
curl http://localhost:8080         # respuesta del sitio en el mismo equipo
docker logs nginx-plantilla        # registros del contenedor
docker restart nginx-plantilla     # prueba de reinicio
```

Prueba desde otro dispositivo: abrir `http://<IP_del_servidor>:8080` en el navegador.

## Comandos realmente ejecutados

*Pendiente: el operador técnico entrega la lista y se copia aquí.*
