# Despliegue de aplicación

## MySQL (base de datos relacional)

- **Imagen:** mysql
- **Puerto:** 3306
- **Nombre del container:** mi-mysql

### Secuencia de pasos
1. Comandos de ejecución container
```bash
docker run --name mi-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=dF4c1L -d mysql
```
![Ejecución container](/curso-docker-kubernetes-tareas/clase1/screenshots/image1.png)
2. Comandos de verificación
- Containers en ejecución
```bash
docker ps
```
![Verificación de containers en ejecución](/curso-docker-kubernetes-tareas/clase1/screenshots/image2.png)
- Logs del container
```bash
docker logs mi-mysql
```
![Verificación de logs del container](/curso-docker-kubernetes-tareas/clase1/screenshots/image3.png)
3. Comandos de limpieza
- Detener container
```bash
docker stop mi-mysql
```
![Detener container](/curso-docker-kubernetes-tareas/clase1/screenshots/image4.png)
- Eliminar container
```bash
docker rm mi-mysql
```
![Eliminar container](/curso-docker-kubernetes-tareas/clase1/screenshots/image5.png)
- Verificar existencia
```bash
docker ps -a
```
![Verificar container](/curso-docker-kubernetes-tareas/clase1/screenshots/image6.png)
### Explicación de comandos
| Flag | Descripción |
| :--- | :--- |
| --name mi-mysql | Asigna el nombre de **mi-mysql** al contenedor |
| -p 3306:3306 | Permite conectarse a la base de datos dentro del contenedor a través del puerto 3306 |
| -e MYSQL_ROOT_PASSWORD=dF4c1L | Pasa una variable de entorno **-e** al contenedor, a su vez establece una contraseña al usuario root |
| -d | Ejecuta el contenedor en segundo plano |
| mysql | Es el nombre de la imagen que se desea ejecutar |