# Dockerizar Aplicación con Multi-Stage Build

## 1. Descripción de la aplicacion

**Lenguaje:** Node.js
**Framework:** Express
**Descripción:** API REST para gestión de usuarios

**Endpoints:**
- GET / - Mensaje de bienvenida
- GET /api/users - Lista de usuarios
- GET /api/users/:id - Obtención de usuario segun ID

**Funcionalidad:**
- Muestra un mensaje de bienvenida
- Extrae la lista de todos los usuarios registrados
- Extrae a un usuario por el ID

## 2. Dockerfile
- Se genera el archivo [Dockerfile](./Dockerfile)
```bash
# Stage 1: Build
FROM node:18-alpine AS build

# Establecer directorio de trabajo
WORKDIR /app

# Copiar archivos de dependencias
COPY package*.json ./

# Instalar todas las dependencias (incluyendo devDependencies)
RUN npm install

# Copiar el código de la aplicación
COPY . .

# Stage 2: Production
FROM node:18-alpine

# Crear usuario no-root para mayor seguridad
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Establecer directorio de trabajo
WORKDIR /app

# Copiar solo package.json y package-lock.json
COPY package*.json ./

# Instalar solo dependencias de producción
RUN npm i --only=production && \
    npm cache clean --force

# Copiar código desde stage de build
COPY --from=build /app/app.js ./

# Cambiar ownership de los archivos al usuario nodejs
RUN chown -R nodejs:nodejs /app

# Cambiar a usuario no-root
USER nodejs

# Exponer puerto
EXPOSE 3000

# Variables de entorno por defecto
ENV NODE_ENV=production \
    PORT=3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', (r) => {process.exit(r.statusCode === 200 ? 0 : 1)})"

# Comando para iniciar la aplicación
CMD ["node", "app.js"]
```

## 3. Proceso Build
- Creación de [.dockerignore](./.dockerignore)
- Construcción de la imagen
```bash
docker build -t mi-app:1.0 .
```
![mi-app](./screenshots/image1.png)

- Verificamos el tamaño
```bash
docker images mi-app
```
![tamaño mi-app](./screenshots/image2.png)

- Ejecutamos localmente
```bash
docker run -d -p 3000:3000 --name mi-app mi-app:1.0
```
![Ejecución de comando](./screenshots/image3.png)

![Vista Web](./screenshots/image4.png)

![Vista Web](./screenshots/image5.png)

- Verifiacamos logs
```bash
docker logs mi-app
```
![Logs](./screenshots/image6.png)

## 4. Testing Local

## 5. Publicación en Docker Hub
- Hacer login en Docker Hub
```bash
docker login
```
![Login](./screenshots/image7.png)

- Tagear la imagen
```bash
docker tag mi-app:1.0 kellychiara/mi-app:1.0
```
![Tag mi-app](./screenshots/image8.png)

- Push a Docker Hub
```bash
docker push kellychiara/mi-app:1.0
```
![Push mi-app](./screenshots/image9.png)

- Verificar en Docker Hub
>[kellychiara/mi-app](https://hub.docker.com/search?q=kellychiara%2Fmi-app)

![mi-app](./screenshots/image10.png)
## 6. Optimizaciones

## 7. Conclusiones
