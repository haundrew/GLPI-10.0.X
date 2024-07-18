# Instalación de GLPI 10.0.X
### ¿Que es GLPI?
GLPI es una aplicación de código abierto que se utiliza para gestionar activos de TI (Tecnologías de la Información) y proporcionar soluciones de gestión de servicios de TI (ITSM, por sus siglas en inglés). El nombre GLPI proviene de "Gestionnaire Libre de Parc Informatique," que en francés significa "Gestión Libre de Parque Informático."


### Requsitos
- 2 servidores web con las siguientes características:
- - Capacidad de 4 CPU’s
- - 8GB de RAM
- - 100GB Servidor APP
- - 500GB Servidor BD (Varia durante la cantidad de información que se vaya almacenando en la Base de Datos.

- Sistema Operativo Linux (recomendado): 
- - Distribuciones como CentOS, Ubuntu, Debian, openSUSE, o cualquier otra distribución Linux compatible suelen ser opciones comunes para ejecutar GLPI.
- - Windows: También es posible ejecutar GLPI en un servidor con Windows, pero es menos común.

- Servidor Web:
- - Apache: Es el servidor web más utilizado con GLPI.
- - NGINX: También es una opción viable.

- Servidor Base de Datos:
- - MySQL: Esta es la base de datos recomendada.
- - MariaDB: Otra alternativa compatible con MySQL. (Recomendable)
- - PostgreSQL: Puede ser utilizado, pero es menos común.

- Lenguaje de Programación:
- - PHP: GLPI está escrito en PHP. Necesitarás una versión compatible de PHP (por ejemplo, PHP 7.3 a 8.2).


🚨 NOTA: La versión de PHP debe ser instalada dentro del rango 7.4.0 a 8.2.0, si la versión supera o es baja a este rango, GLPI no funcionará

# Proceso de Instlación GLPI

## Servidor 1 (APP)
### Instalación de Apache
 Para proceder con la instalación se actualice el httpd índice del paquete Apache local para reflejar los últimos cambios ascendentes:

> sudo yum update httpd

Una vez actualizado el paquete se procede con la instalación:

> sudo yum install httpd

Apache no se inicia automáticamente en CentOS una vez que se completa la instalación. Deberá iniciar el proceso de Apache manualmente

> sudo systemctl start httpd

Se verifica es estatus con:

> sudo systemctl status httpd

<br>

<img src="img/1.PNG" width="">

Adicionalmente habilitamos el puerto 80 para que funcione apache

>sudo firewall-cmd --zone=public --add-port=80/tcp --permanent

>sudo firewall-cmd –reload

Verificamos con:

>sudo firewall-cmd --list-all

<div align="center">
<img src="img/2.PNG">
</div>

### Instalación de PHP

Hay que saber que los repositorios oficiales de CentOS 7 sólo ofrecen hasta la versión de PHP 5.4.16, que es una versión obsoleta e insegura, por lo que debemos realizar los siguientes pasos para proceder con la última versión.

Antes debemos agregar el repositorio.

> sudo yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm

Procedemos a instalar las herramientas de yum para configurar el nuevo repo

>sudo yum install -y yum-utils

Ahora configuramos el repositorio para especificar la versión de PHP 8.2 que nos interesa. 

>sudo yum-config-manager --enable remi-php82

Por último, para finalizar el agregado del repositorio, basta con actualizar la información de los repositorios

>sudo yum update -y

Ahora se procede con la instalación 

>sudo yum install php

>sudo yum install php-{gd,imap,intl,ldap,mbstring,opcache,pear-CAS,pecl-apcu,xmlrpc,pecl-zip, mysql}

Con esto queda instalado más sin embargo, los cambios no tendrán efecto en el servidor web hasta que lo reiniciemos. Por lo que se procede a reiniciar el servicio

>sudo systemctl restart httpd

Ahora ya que se cuenta con los requisitos básicos se puede proceder con la instalación de GLPI para el servidor 1 (APP)

## Servidor 2 (Base de Datos)
### Instalación de MariaDB
