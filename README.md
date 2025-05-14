# ST0263 Tópicos Especiales en Telemática

# Estudiante(s):

- Manuela Castaño - mcastanof1@eafit.edu.co
- Miguel Vásquez - mvasquezb@eafit.edu.co
- Manuel Villegas - mvillegas6@eafit.edu.co

# Profesor:

- Edwin Montoya - emontoya@eafit.edu.co

# Proyecto 2 - Aplicación Escalable

# 1. Breve descripción de la actividad

Este proyecto consiste en escalar y rediseñar una aplicación monolítica de venta de libros de segunda llamada BookStore, desarrollada con Flask y MySQL. A través de tres objetivos principales, se despliega primero la aplicación en una máquina virtual en AWS, luego se escala utilizando infraestructura con autoescalamiento y alta disponibilidad, y finalmente se reliza el mismo despliegue de autoescalamiento utilizando la herramienta de docker swarm. El objetivo es aplicar patrones de escalabilidad en la nube y técnicas de ingeniería de software modernas para garantizar rendimiento, disponibilidad y mantenibilidad.

## 1.1. Aspectos cumplidos

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

## 1.2. Aspectos no logrados

### Objetivo 2

Durante el desarrollo del Objetivo 2, encontramos limitaciones en el escalamiento automatico de servicios externos a la consola de EC2, específicamente con la base de datos Aurora MySQL y el sistema de archivos EFS. Esto se debió a que nuestra cuenta de AWS Academy carecía de los permisos necesarios para utilizar las opciones de autoescalamiento automático de estos servicios. Adicionalmente, las restricciones de la cuenta nos impidieron seleccionar múltiples zonas de disponibilidad (AZ) para las instancias.

### Objetivo 3

Este objetivo se debía de desarollar principalmente utilizando la herramienta de EKS perteneciente a AWS, pero por faltas de permisos de las cuentas academy no se pudo realizar de esta manera, por el contrario implementamos el despliegue con docker swarm como se explicará mas adelante. Otro aspecto faltante, además de los mencionados en el objetivo 2, es que se realizo el despliegue con escalamiento unicamente con la aplicación monolítica, esto por elección tanto del maestro como del grupo en conjunto por falta de tiempo para el desarrollo.

# 2. Información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.

### Objetivo 1

En este primer objetivo se realizó el despliegue de la aplicación monolítica BookStore en una máquina virtual EC2 de AWS. Como parte del diseño de alto nivel, se optó por una arquitectura basada en un único contenedor Docker para la aplicación y otro para la base de datos MySQL, orquestados mediante Docker Compose. Esto permite mantener una separación lógica entre componentes, facilitando el despliegue y la gestión del entorno. Se utilizó una dirección IP elástica para garantizar la persistencia del acceso público a la instancia, y se integró un nombre de dominio personalizado adquirido a través de Namecheap, el cual se configuró mediante registros DNS tipo A para apuntar correctamente al servidor.

Como parte de las buenas prácticas para aplicaciones web, se implementó un proxy inverso utilizando NGINX. Esto actúa como intermediario entre el cliente y la aplicación, redirigiendo el tráfico HTTP/HTTPS hacia el contenedor donde corre BookStore. Además, se instaló un certificado SSL gratuito mediante la herramienta Certbot, lo cual permitió habilitar el protocolo HTTPS y asegurar las comunicaciones entre el cliente y el servidor, cumpliendo con estándares de seguridad. También se configuró una tarea automática para la renovación periódica del certificado, asegurando la continuidad del cifrado sin intervención manual.

### Objetivo 2

Para el desarrollo de este objetivo realizamos el despliegue de nuestra aplicación monolítica mediante la implementación de un grupo de autoescalamiento EC2 (EC2 Auto Scaling Group) detrás de un balanceador de carga de aplicaciones (Application Load Balancer), junto con Amazon EFS y Aurora MySQL. En la práctica, esto nos permitió tener una capa de cómputo encargada de ejecutar la aplicación de manera escalable y monitoreada, además de asegurar alta disponibilidad mediante servicios como Aurora MySQL y Amazon EFS.

Al momento de realizar el despliegue, se tuvieron en cuenta las siguientes buenas prácticas:

- La métrica que controla el escalamiento de nuestro grupo de autoescalamiento está basada en el uso de CPU, permitiendo detectar picos de carga en la aplicación y responder de manera adecuada.
- Nuestro balanceador de carga de aplicaciones realiza verificaciones de salud (health checks) de manera continua a todas las máquinas que se encuentran dentro de su grupo de destino en los servicios de Amazon.
- Los grupos de seguridad (security groups) tienen habilitados únicamente los puertos de entrada necesarios que satisfacen el rol específico de cada uno de nuestros componentes.

En el siguiente diagrama se puede observar la arquitectura y todos los componentes mencionados anteriormente:

![image](https://github.com/user-attachments/assets/9868eee5-f684-4720-a8bd-618fe69c2265)

### Objetivo 3

Para el desarrollo del objetivo 3 se realizó el despliegue de la aplicación utilizando instancias EC2 de AWS que conformaban un cluster de servidores de Docker Swarm. Se encuentra como primera instancia una máquina _manager_ encargada de gestionar el enjambre y a las otras 3 máquinas _workers_ encargadas de mantener en ejecución las distintas réplicas del contenedor de la aplicación.

Además de las instancias EC2 con la ejecución de los contenedores, se implementó una instancia de RDS Aurora con MySQL y una máquina EFS.

Esto se puede evidenciar de una mejor manera con el siguiente diagrama de arquitectura:

![TelematicaSwarm](https://github.com/user-attachments/assets/16086e70-e746-49a6-b86b-5a0c0a5e459f)

La instancia _manager_ además de ser la encargada de distribuir los servicios a los distintos _workers_ que hayan, también se asegura de que en la caída de una de las instancias otro trabajador absorba todos los contenedores que esta tenií en su interior para asegurar el cumplimiento del número de répicas mínimo. Finalmente, el _manager_ tambien actúa como **Load Balancer** y redirige al cliente y lo rota entre las distintas instancias de la app.

# 3. Descripción del ambiente de EJECUCIÓN

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

En este objetivo, vamos a acceder a nuestra aplicación desplegada a través del **Application Load Balancer** que tenemos en nuestra arquitectura. La dirección DNS que Amazon nos provee para acceder es la siguiente:

[Bookstore](bookstore-alb-11071920.us-east-1.elb.amazonaws.com)

### Objetivo 3

Para el desarrollo del objetivo # 3 del proyecto se crearon 4 instancias EC2 de AWS con un manager y 3 workers configurados bajo un cluster docker swarm, la máquina maganer, la cual funciona como punto de llegada del usuario, maneja una direccion IP elástica.

IP Elástica en AWS EC2: `54.152.84.169`

Desde que las máquinas esten corriendo se puede acceder correctamente a la aplicación. Maneja un gran nivel de redundancia y por lo tanto excelente tolerante a fallos.

## Descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)

### Objetivo 2

La configuración de parámetros para este objetivo es bastante simple. A continuación, se presenta una lista de todo lo que debemos configurar antes de realizar nuestro despliegue:

1. Crear los **security groups** para cada una de nuestras máquinas (habilitaremos los puertos necesarios):

   - Para las máquinas EC2 (`sg-web`):
     - Puerto 8000 TCP - Anywhere.
     - Puerto 3306 TCP - Anywhere.
     - Puerto 2049 TCP - Amazon EFS interno.
   - Para la base de datos (`sg-aurora`):
     - Puerto 3306 TCP - Anywhere - Entrante.
   - Para el Application Load Balancer (`sg-alb`):
     - Puertos 80 (HTTP) / 443 (HTTPS) - Anywhere.

2. Crear la base de datos (Aurora MySQL) y guardar el string de conexión.

3. Crear un archivo `docker-compose.yml` con el siguiente contenido:

   ```yml
   version: "3.8"

   services:
     flaskapp:
       image: <dockerhub user>/<dockerhub repo>:latest
       restart: always
       environment:
         - FLASK_ENV=development
       ports:
         - "5000:5000"
       volumes:
         - /mnt/shared:/app/uploads
   ```

   > _No te preocupes por la línea de `image`, el siguiente paso te explica qué debes poner allí._

4. Publicar una imagen de nuestra aplicación en **DockerHub** usando GitHub Actions, para que podamos acceder a ella de manera fácil y eficiente. Para esto:

   1. Crear una cuenta en [DockerHub](https://hub.docker.com/) e iniciar un repositorio con el nombre de tu proyecto.
   2. Ir a [GitHub](https://github.com) y acceder a tu repositorio.
   3. Dirigirse a **Settings > Secrets and variables > Actions** y crear dos secretos con tus credenciales de DockerHub:
      ```bash
      DOCKERHUB_USER = xxxx
      DOCKERHUB_PASS = xxxx
      ```
   4. Ir a la pestaña de **Actions** y buscar **Docker image**. Esto creará un archivo `docker-image.yml` con el siguiente contenido:

      ```yml
      name: Docker Image CI

      on:
        push:
          branches: [main]
        pull_request:
          branches: [main]

      jobs:
        build:
          runs-on: ubuntu-latest
          steps:
            - uses: actions/checkout@v2

            - name: Docker Login
              env:
                DOCKER_USER: ${{ secrets.DOCKERHUB_USER }}
                DOCKER_PASS: ${{ secrets.DOCKERHUB_PASS }}
              run: |
                docker login -u $DOCKER_USER -p $DOCKER_PASS

            - name: Build the Docker image
              run: docker build . --file Dockerfile --tag <dockerhub user>/<dockerhub repo>:latest

            - name: Docker Push
              run: docker push <dockerhub user>/<dockerhub repo>
      ```

   5. Realiza el commit. Si todo está correcto, tendrás tu imagen publicada y lista para usarse.

### Objetivo 3

La configuración de este objetivo esta soportada sobre el punto anterior, esto ya que se utiliza la misma imagen de docker hub configurada anteriormente.

- Para las máquinas EC2 (`sg-web`):
  - Puertos mencionados en el obetivo 2
  - Puerto 7946 (TCP y UDP) - Anywhere
  - Puerto 4789 (UDP) - Anywhere
  - Puerto 2377 (TCP) - Anywhere
  - Puerto 5000 (TCP) - Anywhere

EL resto de la configuración del despliegue se realiza dentro de las instancias y a travéz de docker swarm.

## Despliegue de los servidores

### Objetivo 2

Para desplegar nuestra aplicación monolítica escalable y altamente disponible, comenzaremos por instanciar los servicios que utilizaremos en AWS y luego configuraremos los elementos clave de nuestra arquitectura.

---

#### Instanciar los servicios de AWS

1. Para crear la instancia de Aurora MySQL:

   - En la consola de AWS, busca **RDS Aurora** y selecciona **Create database**.
   - Asegúrate de que:
     - Sea del tipo MySQL.
     - Use la opción `Free tier`.
     - Definas un usuario y contraseña de acceso.
     - Asignes el `security group` previamente creado.

2. Luego, busca **EFS** y selecciona **Create file system**. Asigna el `security group` y continúa con la configuración predeterminada.

---

#### Crear los componentes clave en la consola de EC2

1. Crear una `launch template` para desplegar máquinas idénticas ejecutando nuestro proyecto:

   - Ir a **Launch templates > Create launch template** y configurar:

     - Sistema operativo: `Amazon AMI 2023`.
     - Tipo de instancia: `t2.micro`.
     - Security group: `sg-web`.
     - En **Advanced details**, baja hasta `User Data` y agrega lo siguiente:

       ```bash
       #!/bin/bash
       set -euo pipefail
       set -x

       sudo yum update -y
       sudo yum install -y docker amazon-efs-utils git

       sudo systemctl start docker

       sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
       sudo chmod +x /usr/local/bin/docker-compose

       sudo service docker start

       sudo mkdir -p /mnt/shared
       sudo mount -t efs -o tls fs-<EFS-ID>:/ /mnt/shared
       echo "fs-<EFS-ID>:/ /mnt/shared efs _netdev,tls 0 0" | sudo tee -a /etc/fstab

       sudo git clone --depth 1 https://github.com/<gitUser>/<gitRepo>.git /opt/deploy
       cd /opt/deploy

       sudo docker-compose pull
       sudo docker-compose up -d
       ```

       > Recuerda reemplazar los valores marcados con `<>` por tus respectivas credenciales y configuraciones.

   Esto permite que las máquinas estén listas para recibir peticiones apenas se creen mediante el Auto Scaling Group.

2. Crear el **grupo de destino** donde se conectarán las instancias, y que será usado por el **Application Load Balancer**:

   - Protocolo: `HTTP`.
   - Puerto: `5000`.

3. Crear el **Application Load Balancer**:

   - Ir a **Load balancers > Create load balancer**.
   - Configurar como `Internet-facing`.
   - Listener en el puerto 80.
   - Asignar el grupo de seguridad `sg-alb`.
   - Asociar el grupo de destino previamente creado.

4. Crear el **Auto Scaling Group**:

   - Ir a **Auto Scaling groups > Create Auto Scaling group**.
   - Asignar un nombre específico.
   - Usar la `launch template` creada anteriormente.
   - Asociar el Application Load Balancer (esto enlaza automáticamente el grupo de destino).
   - Tamaño del grupo:
     - Máquinas deseadas: 2
     - Mínimas: 2
     - Máximas: 6
   - Políticas de escalado:
     - **Target tracking**
     - `Target value`: 50%
     - `Instance warmup`: 120 segundos.

   Le damos **Create Auto Scaling group** y esperamos a que todo aparezca inicializado.

---

¡Y listo! Nuestro Auto Scaling Group se encargará de crear automáticamente las instancias, y podremos acceder a la aplicación a través del **Application Load Balancer**.

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

## Imagenes relevantes

Nodos disponibles del cluster de docker swarm:
![Screenshot_2025-05-08-20-46-23_1920x2160](https://github.com/user-attachments/assets/58dddb67-8e7b-4e6d-a3b0-2a282d06ee1a)

Réplicas de los contenedores en los nodos `workers`:
![Screenshot_2025-05-08-20-50-18_1920x2160](https://github.com/user-attachments/assets/1d3ffcae-60de-45ef-803d-d4e1db9a57b0)

La aplicación ejecutandose en los distintos nodos (id de la máquina en la navbar):
![Screenshot_2025-05-08-20-51-19_1920x2160](https://github.com/user-attachments/assets/b8048b44-3fb3-4373-b901-e67a2dbacb67)
![Screenshot_2025-05-08-20-51-35_1920x2160](https://github.com/user-attachments/assets/80161b76-6234-4749-8f8f-11c092ac15d2)
![Screenshot_2025-05-08-20-51-46_1920x2160](https://github.com/user-attachments/assets/670c9ffe-0fdb-4a7e-abd8-78c892bd2979)

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
