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

## 1.2. Que aspectos NO cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

# 2. información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.

### Objetivo 1

### Objetivo 2

### Objetivo 3

Para el desarrollo del objetivo 3 se realizo el despliegue de la aplicación utilizando instancias EC2 de AWS que conformaban un cluster de servidores de docker swarm. Se encuentra como primera instancia una máquina _manager_ encargada de manejar el enjambre y a las otras 3 máquinas _workers_ encargadas de mantener en ejecucion las distintas replicas del contenedor de la aplicación.

Ademas de las instancias EC2 con la ejecución de los contenedores se implementó una instancia de RDS Aurora con MySQL y una máquina EFS.

Se puede evidenciar de una mejor manera con el siguiente diagrama de arquitectura:

![TelematicaSwarm](https://github.com/user-attachments/assets/16086e70-e746-49a6-b86b-5a0c0a5e459f)

La instancia _manager_ ademas de ser la encargada de distribuir los servicios a los distintos _workers_ que hayan tambien se asegura de que en la caida de una de las instancias otro trabajador absorva todos los contenedores que esta tenia en su interior para asegurar el cumplimiento del número de répicas mínimo. Finalmente el _manager_ tambien actúa como **Load Balancer** y re dirige al cliente y lo rota entre las distintas instancias de la app.

# 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

## como se compila y ejecuta.

## detalles del desarrollo.

## detalles técnicos

## descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)

## opcional - detalles de la organización del código por carpetas o descripción de algún archivo. (ESTRUCTURA DE DIRECTORIOS Y ARCHIVOS IMPORTANTE DEL PROYECTO, comando 'tree' de linux)

##

## opcionalmente - si quiere mostrar resultados o pantallazos

# 4. Descripción del ambiente de EJECUCIÓN (en producción) lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

# IP o nombres de dominio en nube o en la máquina servidor.

## descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)

## como se lanza el servidor.

## una mini guia de como un usuario utilizaría el software o la aplicación

## opcionalmente - si quiere mostrar resultados o pantallazos

# 5. otra información que considere relevante para esta actividad.

# referencias:

<debemos siempre reconocer los créditos de partes del código que reutilizaremos, así como referencias a youtube, o referencias bibliográficas utilizadas para desarrollar el proyecto o la actividad>

## sitio1-url

## sitio2-url

## url de donde tomo info para desarrollar este proyecto
