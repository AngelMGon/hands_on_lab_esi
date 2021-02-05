# Introducción
En este lab vamos a ver los pasos necesarios para montar una pequeña aplicación con Microservicios en Oracle Cloud. 
Todos los componentes de la aplicación estarán desplegados en Kubernetes de forma automatizada usando Visual Builder Studio.

# Prerequisitos
Para poder empezar este lab necesitamos una cuenta de Oracle Cloud, antes del inicio del taller debeis haber recibido un email con un enlace para registraros de forma gratuita, si aun no lo habeis hecho, podeis hacerlo en este enlace: [clic aqui!](https://signup.oraclecloud.com/).

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

# Configurando la instancia de Visual Builder Studio
# Configurando la instancia de Kubernetes
# Desplegando nuestras apps
