# Proxy Inverso para MarMon

## ¿Qué es un proxy inverso?

Un proxy inverso es un servidor que se sitúa entre los clientes y uno o más servidores backend, recibiendo las solicitudes de los clientes y redirigiéndolas a los servidores apropiados. A diferencia de un proxy tradicional (que protege a los clientes), el proxy inverso protege y optimiza los servidores backend.

## Funciones principales de un proxy inverso

1. **Balanceo de carga**: Distribuye tráfico entre múltiples servidores
2. **Terminación SSL**: Maneja el cifrado/descifrado HTTPS
3. **Caché**: Almacena contenido estático para mejorar rendimiento
4. **Seguridad**: Oculta la infraestructura backend
5. **Compresión**: Reduce el tamaño de las respuestas
6. **Enrutamiento**: Dirige solicitudes a diferentes backends según reglas

## Implementación actual

Este proyecto implementa un proxy inverso usando Nginx que:
- Sirve dos aplicaciones backend diferentes:
  - `/tienda` en el puerto 8080 (servido por Nginx)
  - `/news` en el puerto 9090 (servido por Python HTTP Server)
- Implementa HTTPS con certificados autofirmados
- Redirige automáticamente HTTP a HTTPS

## Arquitectura del sistema

Clientes
↓
[Proxy Inverso Nginx] (HTTPS, puertos 80/443)
├─→ [Backend1 Nginx] (/tienda, puerto 8080)
└─→ [Backend2 Python] (/news, puerto 9090)

## Configuración detallada

### 1. Proxy Inverso (Nginx)

- **Puertos expuestos**: 80 (HTTP) y 443 (HTTPS)
- **Configuración**:
  - Terminación SSL con certificados autofirmados
  - Redirección automática HTTP→HTTPS
  - Reglas de enrutamiento:
    - `/tienda/` → backend1:8080
    - `/news/` → backend2:9090
  - Cabeceras personalizadas para mantener información del cliente original

### 2. Backend1 - Tienda (Nginx)

- **Puerto**: 8080
- **Función**: Sirve contenido estático HTML para la tienda
- **Configuración mínima** optimizada para servir archivos

### 3. Backend2 - News (Python HTTP Server)

- **Puerto**: 9090
- **Función**: Servidor simple para demostración
- **Nota**: En producción se recomendaría usar Nginx también

## Certificados SSL

Se utilizan certificados autofirmados para demostración.

## Cómo usar este proyecto

1. Clona el repositorio
2. Ejecuta `docker-compose up -d`
3. Accede a:
   - https://localhost/tienda/ (para la tienda)
   - https://localhost/news/ (para noticias)