# Introducción
En este lab vamos a ver los pasos necesarios para montar una pequeña aplicación con Microservicios en Oracle Cloud. 
Todos los componentes de la aplicación estarán desplegados en Kubernetes de forma automatizada usando Visual Builder Studio.

# Prerequisitos
Para poder empezar este lab necesitamos una cuenta de Oracle Cloud, antes del inicio del taller debeis haber recibido un email con un enlace para registraros de forma gratuita, si aun no lo habeis hecho, podeis hacerlo en este enlace: [clic aqui!](https://signup.oraclecloud.com/).

Os debe salir algo parecido a esto:
 
![oracle_cloud_login](/images/oracle_cloud_registro_1_1.PNG)

Rellenais con vuestros datos y elegis "Frankfurt" como region.

![oracle_cloud_login](/images/oracle_cloud_registro_1_2.PNG)

Una vez completado el formulario, recibirás un correo de Oracle con la URL de acceso (Este proceso puede tardar).

# Componentes de este lab

[1. Preparando la instancia de cloud](#preparando-la-instancia-de-cloud)

[2. Configurando la instancia de Visual Builder Studio](#configurando-la-instancia-de-visual-builder-studio)

[3. Configurando la instancia de Kubernetes](#configurando-la-instancia-de-kubernetes)

[4. Desplegando nuestras apps](#desplegando-nuestras-apps)

# Preparando la instancia de cloud

Una vez te has registrado y tu instancia de Oracle Cloud, está lista, debes recibir un correo como el que se muestra a continuación, con el nombre de tu instancia (el que rellenaste en el formulario) y un enlace para conectar:

![oracle_cloud_registro](/images/oracle_cloud_registro_1.PNG)

Es necesario que hagais login usando el SSO como se muestra a continuación con vuestro usuario:

![oracle_cloud_login](/images/oracle_cloud_login_2.png)

Una vez lez logados, os encontrareis en la consola de Oracle Cloud Infrastructure, que debería parecerse a esto:

![oracle_cloud_login](/images/oci_3.png)

## Crear un compartimento

Lo primero que vamos a hacer es crear un compartimento, para ello vamos a hacer clic en la hamburguesa de la izquierda:

![oracle_cloud_login](/images/hamburguesa_4.PNG)

Y vamos a Identity->Compartment:

![oracle_cloud_login](/images/identity_compartments_5.PNG)

Hacemos click en el botón "Create Compartment":

![oracle_cloud_login](/images/create_compartment_6.PNG)

Rellenamos los datos y hacemos clic en "Create Compartment"

- Name: Enter ESIWorkshop
- Description: Compartment to be used in ESIWorkshop
- Parent Compartment: Selecciona el root.

![oracle_cloud_login](/images/create_compartment_7.PNG)

Una vez creado, debe aparece en listado de compartments:

![oracle_cloud_login](/images/configure_compartment_8.PNG)

Si hacemos clic en él, entraremos en el detalle, anotad el OCID, ya que lo usaremos mas adelante.

![oracle_cloud_login](/images/configure_compartment_9.PNG)

## Crear Token y API Key de nuestro usuario

Para poder acceder a los servicios de cloud de forma segura, necesitamos usar una API Key y un token, a continuación vamos a ver como crear estos elementos.

### Obtener los datos de tu usuario

Vamos a buscar nuestro usuario en el buscador de OCI:

![oracle_cloud_login](/images/search_user_10.PNG)

Selecciona el usuario que se parece a esto: oracleidentitycloudservice/(tu_nombre)

### Crear el token

Abajo a la izquierda, hacemos clic en Auth Tokens y en Generate Token:

![oracle_cloud_login](/images/create_token_11.PNG)

Le damos un nombre al token, por ejemplo: "OCI access Auth Token" y hacemos clic en "Generate Token" :

![oracle_cloud_login](/images/create_token_12.PNG)

En el popup que nos aparece, hacemos clic en Copy, y anotamos este token en un archivo de texto, (¡¡Si pierdes el token habrá que volver a crearlo!!)

![oracle_cloud_login](/images/create_token_13.PNG)

### Crear un API Key

En la página de detalle de nuestro usuario, abajo a la izquierda hacemos clic en "API Keys" y hacemos clic en "Add API Key":

![oracle_cloud_login](/images/api_key_14.PNG)

Elegimos la opción "Generate API Key pair" y hacemos clic en descargar la clave privada y la pública, una vez lo hemos descargado, hacemos clic en "add":

![oracle_cloud_login](/images/api_key_15.PNG)

En el siguiente popup, hacemos clic en copy, y guardamos estos datos. Hacemos clic en "close"

![oracle_cloud_login](/images/api_key_16.PNG)

# Configurando la instancia de Visual Builder Studio

En esta sección vamos a ver como crear la instancia de Visual Builder Studio(VBS) y como configurarla.

## Creando la instancia

Para crear la instancia, desde el menu de OCI vamos a Platform Services -> Developer

![oracle_cloud_login](/images/vbs_create_17.PNG)

Hacemos clic en "Create instance":

![oracle_cloud_login](/images/vbs_create_18.PNG)

Rellenamos el formulario, le damos un nombre a la instancia, descripción, nuestro email y en region elegimos : eu-frankfurt-1.

![oracle_cloud_login](/images/vbs_create_19.PNG)

En la siguiente pantalla podemos ver un resumen, hacemos clic en Create.

![oracle_cloud_login](/images/vbs_create_20.PNG)

En la siguiente pantalla vemos como se está creado la instancía (puede tardar unos minutos):

![oracle_cloud_login](/images/vbs_create_21.PNG)

Podemos ir refrescando la página, una vez creado hacemos clic en la hamburguesa y en "Access Service Instance"

![oracle_cloud_login](/images/vbs_create_22.png)

## Conectando VBS con OCI

Para conectar con OCI, necesitamos ciertos datos, algunos ya los hemos ido guardando, para obtener los que nos faltan, tenemos que ir a Administration -> Tenancy Details y guardamos el valor de "Object Storage Namespace"

![oracle_cloud_login](/images/vbs_configure_26.png)

Volvemos a VBS y en la pestaña OCI Accountm hacemos clic en "Connect":

![oracle_cloud_login](/images/vbs_configure_23.png)

Rellenamos el formulario con los datos que hemos ido obteniendo, hacemos clic en validate y si todo está correcto hacemos clic en "Save":

![oracle_cloud_login](/images/vbs_configure_24.png)

## Crear una máquina virtual

Una vez hemos conectado la cuenta de OCI, vamos a configurar las maquinas virtuales que vamos a usar para ejecutar los jobs de construcción de las imagenes y despliegue en kubernetes, para ello vamos a la pestaña "Virtual Machines Templates" y hacemos clic en "Create template"

![oracle_cloud_login](/images/vbs_configure_template_27.png)

Le damos un nombre a la template, puede ser OKE, elegimos Oracle Linux 7 y hacemos clic en create:

![oracle_cloud_login](/images/vbs_configure_template_28.png)

Una vez creada, elegimos la template y hacemos clic en "Configure Software":

![oracle_cloud_login](/images/vbs_configure_template_29.png)

Seleccionamos los siguientes paquetes de software:

- Docker 17.12
- Kubectl
- OCIcli ==> Al añadir este paquete, pedirá añadir tambien Python3 (se añade haciendo clic en el +)
- SQLcl 

![oracle_cloud_login](/images/vbs_configure_template_30.png)

Una vez configurado, hacemos clic en Done y veremos un resumen lo que tiene configura la template:

![oracle_cloud_login](/images/vbs_configure_template_30.png)

Ahora que tenemos una plantilla donde hemos configurado que queremos que tenga nuestra VM, vamos a crearla, para ello vamos a "Virtual Machines" y hacemos clic en "Create VM":

![oracle_cloud_login](/images/vbs_create_vm_32.png)

Elegimos las siguientes opciones y hacemos clic en "Add":

![oracle_cloud_login](/images/vbs_create_vm_33.png)

## Creando un nuevo proyecto en VBS

Vamos a la pestaña "projects" y hacemos clic en "Create":

![oracle_cloud_login](/images/vbs_create_project_34.PNG)

Le damos un nombre al proyecto y hacemos clic en "Next":

![oracle_cloud_login](/images/vbs_create_project_35.PNG)

Elegimos la opción "Empty Project" y hacemos clic en "Next":

![oracle_cloud_login](/images/vbs_create_project_36.PNG)

Hacemos clic en "Finish":

![oracle_cloud_login](/images/vbs_create_project_37.PNG)

Nos debe aparecer una pantalla donde podemos ver las herramientas que se están cargando en el proyecto, una vez finaliza nos redirigirá a la pantalla del proyecto:

![oracle_cloud_login](/images/vbs_create_project_38.PNG)

## Configurando OCIR en VBS

Ahora vamos a conectar nuestro proyecto de VBS con OCIR, para ello vamos a la opción "Docker" en el menú de la izquierda y hacemos clic en "Link External Registry":

![oracle_cloud_login](/images/config_ocir_39.PNG)

Rellenamos en formulario con los siguientes datos:

- Registry Name: MyOCIR
- Registry URL: La url del repositorio de OCIR. Para Frankfurt es: https://fra.ocir.io , si habeís elegido otra zona debeis cambiar "fra" por el código de la zona.
- Username:  Está compuesto por {object namespace}/oracleidentitycloudservice/{username}, teneis el ejemplo de mi usuario en el siguiente pantallazo.
- Password: El token que hemos configurado previamente (No es la contraseña de vuestro usuario!)

![oracle_cloud_login](/images/config_ocir_40.PNG)
# Configurando la instancia de Kubernetes

Primero vamos a crear la instancia de Kubernetes, para ello vamos a Developer Services -> Kubernetes Clusters:

![oracle_cloud_login](/images/create_k8s_cluster_41.PNG)

Elegimos un compartment y hacemos clic en "Create Cluster":

![oracle_cloud_login](/images/create_k8s_cluster_42.PNG)

Elegimos "Quick Create" y hacemos clic en "Launch Workflow"

![oracle_cloud_login](/images/create_k8s_cluster_43.PNG)

Le damos un nombre a nuestro cluster y dejamos lo demás por defecto, hacemos clic en "Next"

![oracle_cloud_login](/images/create_k8s_cluster_44.PNG)

En la siguiente pantalla vemos un resumen con lo que se va a crear, hacemos clic en "Create cluster":

![oracle_cloud_login](/images/create_k8s_cluster_45.PNG)

En esta pantalla vemos como se estan creando todos los recursos necesarios, cuando haya terminado, hacemos clic en "Close":

![oracle_cloud_login](/images/create_k8s_cluster_46.PNG)

Nos aparecerá el detalle del cluster con estado "creando":

![oracle_cloud_login](/images/create_k8s_cluster_47.PNG)

Una vez finalizado, nos aparecerá con estado "active", hacemos clic en el boton "Access Cluster", y clic en el botón "Launch Cloud Shell":

![oracle_cloud_login](/images/access_k8s_cluster_48.PNG)

Creamos un directorio y accedemos a el:

`mkdir -p esi_workshop`
`cd esi_workshop`

Lanzamos el comando del punto 2:

![oracle_cloud_login](/images/access_k8s_cluster_49.PNG)

Vamos a visualizar el archivo:

![oracle_cloud_login](/images/access_k8s_cluster_50.PNG)

Copiamos el contenido del archivo en nuestro pc:

![oracle_cloud_login](/images/access_k8s_cluster_51.PNG)

Creamos en k8s un secret con los datos de conexión a nuestro OCIR:

`kubectl create secret docker-registry ocirsecret --docker-server=<region-key>.ocir.io  --docker-username='<tenancy-name>/<user-name>' --docker-password='<auth-token>'  --docker-email='<email-address>'`

Ejemplo:

`kubectl create secret docker-registry ocirsecret --docker-server=fra.ocir.io  --docker-username='frvaiebqmilk/oracleidentitycloudservice/angel.mogollon@avanttic.com' --docker-password='stZjDP-)-hzl2QX:+<<Z'  --docker-email='angel.mogollon@avanttic.com'`

![oracle_cloud_login](/images/access_k8s_cluster_52.PNG)

Verificar que se ha creado:

`kubectl get secret ocirsecret --output=yaml`

# Creando la instancia de MySQL

Vamos a crear una instancia de MySQL para almacenar los datos de nuestra aplicación

## Configurando politicas

Lo primero es configurar las políticas de acceso a la MySQL.

Primero accedemos a Identity -> Policies

![oracle_cloud_login](/images/policies_53.PNG)

Hacemos clic en "Create Policy":

![oracle_cloud_login](/images/policies_54.PNG)

Le damos un nombre y hacemos clic en "Customize":

![oracle_cloud_login](/images/policies_55.PNG)

Pegamos las siguientes tres lineas:

`Allow group Administrators to {SUBNET_READ, SUBNET_ATTACH, SUBNET_DETACH, VCN_READ, COMPARTMENT_INSPECT} in tenancy`
`Allow group Administrators to manage mysql-family in tenancy`
`Allow group Administrators to use tag-namespaces in tenancy`

Y hacemos clic en "Create":

![oracle_cloud_login](/images/policies_56.PNG)

## Instanciando MySQL

Vamos ahora a crear la instancia de MySQL, para ello navegamos a MySQL -> DB Systems:

![oracle_cloud_login](/images/create_mysql_57.PNG)

## Creando instancia linux para conectar a la MySQL

Elegimos el compartment y hacemos clic en "Create MySQL DB System":

![oracle_cloud_login](/images/create_mysql_58.PNG)

Dejamos todo por defecto y hacemos clic en Next:

![oracle_cloud_login](/images/create_mysql_59.PNG)

Le damos nombre de usuario y password:

Podeis usar estos datos:

- Username: admin
- Password: ESIW0rk$h0p

Lo demás lo dejamos por defecto y hacemos clic en "Next":

![oracle_cloud_login](/images/create_mysql_60.PNG)

Todo por defecto y hacemos clic en "Create":

![oracle_cloud_login](/images/create_mysql_61.PNG)

Nos aparecerá el detalle de la mysql con estado creando:

![oracle_cloud_login](/images/create_mysql_62.PNG)

Una vez creado, nos aparecerá toda la la información, copiamos  la ip, ya que la necesitaremos en proximos pasos:

![oracle_cloud_login](/images/create_mysql_63.PNG)

## Creando nuestra instancia de linux para acceder a la MySQL

Para acceder a la MySQL por seguridad solo podemos acceder desde una máquina que esté en la misma red, para ello vamos a crear una máquina virtual, para ello navegamos a Compute -> Instances:

![oracle_cloud_login](/images/create_instance_64.PNG)

Y hacemos clic en "Create instance":

![oracle_cloud_login](/images/create_instance_64_2.PNG)

En la siguiente pantalla dejamos todo por defecto, pero antes nos descargamos la clave privada y púbica y la guardamos en nuestro pc:

![oracle_cloud_login](/images/create_instance_65.PNG)

Nos aparecerá el detalle de la instancia con estado creando, una vez finalice aparecerá con estado running y podemos ver los detalles de la máquina, vamos a necesitar la Public IP Address:

![oracle_cloud_login](/images/create_instance_66.PNG)

Volvemos a nuestra Cloud Shell, vamos a pasar la clave ssh que tenemos en nuestro pc a la cloud shell, para ello vamos a abrir la clave que hemos descargado con un editor de texto (bloc de notas, notepad++...) y vamos a copiar el contenido.

En la cloud shell vamos a crear un archivo con la clave:

`vi ssh.key`

Pegamos el contenido y guardamos.

Vamos a darle los permisos necesarios:

`chmod 600 ssh.key`

Conectamos a la máquina por ssh con el siguiente comando (Teneis que cambiar la IP por la de vuestra instancia):

`ssh -i ssh.key opc@130.61.59.51`

![oracle_cloud_login](/images/connect_to_mysql_67.PNG)

Una vez dentro vamos a instalar el cliente MySQL para conectar a la BBDD:

`sudo yum install https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm`
`sudo yum install mysql`

Para poder conectar desde está máquina a la MySQL, primero tenemos que habilitar la conexión a sus puertos, para eso vamos a la subred de la MySQL y añadimos la siguiente regla:

![oracle_cloud_login](/images/connect_to_mysql_68.PNG)

Una vez configurados los puertos, ya podemos acceder, para ello usamos el siguiente comando, sustituyendo la IP por la de vuestra MySQL:

`mysql --host 10.0.10.6 -u admin -p`

![oracle_cloud_login](/images/connect_to_mysql_69.PNG)

Creamos nuestra base de datos:

`CREATE DATABASE db_avtday;`


# Desplegando nuestras apps
