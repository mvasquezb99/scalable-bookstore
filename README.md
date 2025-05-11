# ST0263 Tópicos Especiales en Telemática

# Estudiante(s):

- Manuela Castaño - mcastanof1@eafit.edu.co
- Miguel Vásquez - mvasquezb@eafit.edu.co
- Manuel Villegas - mvillegas6@eafit.edu.co

# Profesor:

- Edwin Montoya - emontoya@eafit.edu.co

# Proyecto 2 - Aplicación Escalable

# 1. breve descripción de la actividad

Este proyecto consiste en escalar y rediseñar una aplicación monolítica de venta de libros de segunda llamada BookStore, desarrollada con Flask y MySQL. A través de tres objetivos principales, se despliega primero la aplicación en una máquina virtual en AWS, luego se escala utilizando infraestructura con autoescalamiento y alta disponibilidad, y finalmente se reliza el mismo despliegue de autoescalamiento utilizando la herramienta de docker swarm. El objetivo es aplicar patrones de escalabilidad en la nube y técnicas de ingeniería de software modernas para garantizar rendimiento, disponibilidad y mantenibilidad.

## 1.1. Que aspectos cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

### Requerimientos Funcionales

1. **Autenticación de usuarios:** Registro, inicio y cierre de sesión.

2. **Visualización del catálogo:** Mostrar libros disponibles para la venta publicados por otros usuarios.

3. **Compra de libros:** Permitir a los usuarios comprar libros del catálogo, gestionando el stock disponible.

4. **Pago simulado:** Simular el proceso de pago de los libros comprados.

5. **Envío simulado:** Simular el envío de los libros comprados.

6. **Publicación de libros:** Permitir que los usuarios publiquen libros para la venta.

7. **Separación en microservicios:** _(supeditado al tiempo)_

   - Servicio de autenticación.

   - Servicio de catálogo.

   - Servicio de compra, pago y entrega.

8. **Interoperabilidad entre microservicios:** Los servicios deben estar correctamente integrados para mantener la funcionalidad completa del sistema.

9. **Disponibilidad del servicio web:** La aplicación debe estar accesible desde un dominio público con HTTPS.

### Requerimientos No Funcionales

1. **Despliegue en la nube:** Utilizar AWS para desplegar y escalar la aplicación.

1. **Uso de Docker y docker-compose:** Para contenerizar la aplicación y su base de datos en el estado monolítico.

1. **Escalabilidad:**

   - **Objetivo 2:** Usar VMs con autoescalamiento, base de datos administrada y NFS externo con alta disponibilidad.

   - **Objetivo 3:** Desplegar en un clúster de Docker-Swarm con políticas de escalamiento.

1. **Alta disponibilidad:** Asegurar que la aplicación continúe operativa ante fallos en una VM o pod.

1. **Balanceo de carga:** Uso de ELB (Elastic Load Balancer) para distribuir tráfico entre instancias.

1. **Dominio con certificado SSL:** Exponer el servicio a través de un dominio seguro con HTTPS.

1. **Mantenibilidad:** Estructura de proyecto clara y modular para facilitar cambios futuros.

1. **Disponibilidad continua:** Garantizar que los servicios estén activos durante toda la evaluación.

1. **Entrega colaborativa:** Trabajo en grupo con participación activa de todos los integrantes en la entrega y sustentación.

1. **Automatización del despliegue:** Scripts y configuraciones que permitan reproducir el despliegue fácilmente.

## 1.2. Qué aspectos NO cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

# 2. Información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.

### Objetivo 1

En este primer objetivo se realizó el despliegue de la aplicación monolítica BookStore en una máquina virtual EC2 de AWS. Como parte del diseño de alto nivel, se optó por una arquitectura basada en un único contenedor Docker para la aplicación y otro para la base de datos MySQL, orquestados mediante Docker Compose. Esto permite mantener una separación lógica entre componentes, facilitando el despliegue y la gestión del entorno. Se utilizó una dirección IP elástica para garantizar la persistencia del acceso público a la instancia, y se integró un nombre de dominio personalizado adquirido a través de Namecheap, el cual se configuró mediante registros DNS tipo A para apuntar correctamente al servidor.

Como parte de las buenas prácticas para aplicaciones web, se implementó un proxy inverso utilizando NGINX. Esto actúa como intermediario entre el cliente y la aplicación, redirigiendo el tráfico HTTP/HTTPS hacia el contenedor donde corre BookStore. Además, se instaló un certificado SSL gratuito mediante la herramienta Certbot, lo cual permitió habilitar el protocolo HTTPS y asegurar las comunicaciones entre el cliente y el servidor, cumpliendo con estándares de seguridad. También se configuró una tarea automática para la renovación periódica del certificado, asegurando la continuidad del cifrado sin intervención manual.

### Objetivo 2

### Objetivo 3

Para el desarrollo del objetivo 3 se realizó el despliegue de la aplicación utilizando instancias EC2 de AWS que conformaban un cluster de servidores de Docker Swarm. Se encuentra como primera instancia una máquina _manager_ encargada de gestionar el enjambre y a las otras 3 máquinas _workers_ encargadas de mantener en ejecución las distintas réplicas del contenedor de la aplicación.

Además de las instancias EC2 con la ejecución de los contenedores, se implementó una instancia de RDS Aurora con MySQL y una máquina EFS.

Esto se puede evidenciar de una mejor manera con el siguiente diagrama de arquitectura:

![TelematicaSwarm](https://github.com/user-attachments/assets/16086e70-e746-49a6-b86b-5a0c0a5e459f)

La instancia _manager_ además de ser la encargada de distribuir los servicios a los distintos _workers_ que hayan, también se asegura de que en la caída de una de las instancias otro trabajador absorba todos los contenedores que esta tenií en su interior para asegurar el cumplimiento del número de répicas mínimo. Finalmente, el _manager_ tambien actúa como **Load Balancer** y redirige al cliente y lo rota entre las distintas instancias de la app.

# 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

## como se compila y ejecuta.

## detalles del desarrollo.

## detalles técnicos

## descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)

## opcional - detalles de la organización del código por carpetas o descripción de algún archivo. (ESTRUCTURA DE DIRECTORIOS Y ARCHIVOS IMPORTANTE DEL PROYECTO, comando 'tree' de linux)

##

## opcionalmente - si quiere mostrar resultados o pantallazos

# 4. Descripción del ambiente de EJECUCIÓN

Todos los objetivos realizados en el proyecto se trabajaron teniendo como base la aplicación monolítica Bookstore la que utiliza las siguientes tecnologías:

- **Flask (v3.0.2):**
  Microframework web ligero para Python, ideal para desarrollar aplicaciones web rápidamente y con gran flexibilidad.

- **Flask-SQLAlchemy (v3.1.1):**
  Extensión para Flask que simplifica el uso de SQLAlchemy, un ORM (Object Relational Mapper) para interactuar con bases de datos relacionales.

- **Flask-Login (v0.6.3):**
  Extensión que gestiona sesiones de usuario y autenticación de manera sencilla en aplicaciones Flask.

- **PyMySQL (v1.1.0):**
  Conector MySQL escrito en Python puro que permite conectar Flask (vía SQLAlchemy) a bases de datos MySQL/MariaDB.

- **Werkzeug (v3.0.2):**
  Biblioteca WSGI que proporciona herramientas útiles para el manejo de solicitudes, respuestas y utilidades relacionadas con servidores web.

# IP o nombres de dominio en nube o en la máquina servidor.

### Objetivo 1:

Para el desarrollo de este objetivo en primer lugar se asignó una IP Elástica. Posteriormente, se compró un dominio en Namecheap y se asignó dicha IP a un registro DNS tipo A.

IP Elástica en AWS EC2: `54.159.205.71`

Dominio registrado: https://proyecto2.lat/

Desde que la máquina esté corriendo, la aplicación se puede acceder con normalidad desde ese link.

### Objetivo 2

### Objetivo 3

Para el desarrollo del objetivo # 3 del proyecto se crearon 4 instancias EC2 de AWS con un manager y 3 workers configurados bajo un cluster docker swarm, la máquina maganer, la cual funciona como punto de llegada del usuario, maneja una direccion IP elástica.

IP Elástica en AWS EC2: `54.152.84.169`

Desde que las máquinas esten corriendo se puede acceder correctamente a la aplicación. Maneja un gran nivel de redundancia y por lo tanto excelente tolerante a fallos.

## descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)

## Despliegue de los servidores.

### Objetivo 3 (Despliegue en utilizando docker swarm)

Este tutorial te guía paso a paso para crear un clúster Docker Swarm en AWS usando 4 instancias EC2.

#### 1️⃣ Crear 4 Máquinas Virtuales en AWS

1. Ingresa a [AWS EC2 Console](https://console.aws.amazon.com/ec2).
2. Lanza **4 instancias** con:
   - Sistema operativo: **Amazon Linux 2** o **Ubuntu 24.04 LTS**
   - Tipo: `t2.micro` (gratuito)
   - Red: misma **VPC** y **subred** para permitir comunicación interna
   - Grupo de seguridad:
     - SSH: TCP 22 desde tu IP
     - Docker Swarm: TCP/UDP 2377, 7946, UDP 4789 desde el grupo de seguridad mismo
3. Asigna un nombre claro a cada instancia:
   - `manager`
   - `worker-1`
   - `worker-2`
   - `worker-3`

#### 2️⃣ Instalar Docker en cada instancia

Conéctate a cada instancia vía SSH:

```bash
ssh -i tu-clave.pem ec2-user@IP_PUBLICA
```

```bash
# Para Amazon Linux 2

sudo yum update -y
sudo amazon-linux-extras enable docker
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user
```

```bash
# Para Ubuntu

sudo apt update && sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

#### Inicializar Docker Swarm

En la instancia `manager`:

```bash
docker swarm init --advertise-addr IP_PRIVADA_MANAGER
```

Se guarda el token generado y se coneta desde los `workers`:

```bash
docker swarm join --token SWMTKN-xxx IP_PRIVADA_MANAGER:2377
```

#### Creación del servicio con 10 réplicas

Desde el `manager` se inicializa el servicio con las 10 réplicas, unicamente para los nodos `worker`:

```bash
docker service create \
  --name mi-servicio \
  --replicas 10 \
  --constraint 'node.role == worker' \
  tu_usuario_dockerhub/tu_imagen:tag
```

Para verificar la creación ejecuta el siguiente comando:

```bash
docker service ls
```

## Guía de uso para el usuario

Desde el punto de vista del usuario es transparente la distincion de los 3 despliegues realizados, el usuario solamente ingresa a la dirección IP de una de las máquinas (o el dominio registrado) y se le proveerá la aplicacion con sus funcionalidades correspondientes.

## Imagenes relevalntes

# 5. otra información que considere relevante para esta actividad.

# Referencias

A continuación se presentan las referencias usadas para el desarrollo del proyecto.

## Aplicación Monolítica Bookstore

Esta es la aplicación Bookstore monolítica ya proporcionada por el profesor del curso como punto de partida.

https://github.com/st0263eafit/st0263-251/blob/main/proyecto2/BookStore.zip

## Namecheap

Sitio web de donde se obtuvo el dominio proyecto2.lat, donde además se hizo la configuración de DNS.

https://www.namecheap.com/

## ChatGPT

Herramienta de Inteligencia Artificial utilizada para resolver dudas, correcciones de redacción, y consultar sobre mejores prácticas y recursos para el funcionamiento del proyecto.

https://chatgpt.com/

```

```
