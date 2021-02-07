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

![oracle_cloud_login](/images/vbs_create_project_39.PNG)

Rellenamos en formulario con los siguientes datos:

- Registry Name: MyOCIR
- Registry URL: La url del repositorio de OCIR. Para Frankfurt es: https://fra.ocir.io , si habeís elegido otra zona debeis cambiar "fra" por el código de la zona.
- Username:  Está compuesto por {object namespace}/oracleidentitycloudservice/{username}, teneis el ejemplo de mi usuario en el siguiente pantallazo.
- Password: El token que hemos configurado previamente (No es la contraseña de vuestro usuario!)

![oracle_cloud_login](/images/vbs_create_project_40.PNG)
# Configurando la instancia de Kubernetes
# Desplegando nuestras apps
