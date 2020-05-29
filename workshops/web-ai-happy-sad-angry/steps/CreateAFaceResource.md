# <a name="create-a-face-api-resource"></a>Creación de un recurso de Face API

En el [paso anterior](./DeployTheWebAppToTheCloud.md), implementó una aplicación web en la nube, hospedada en Azure. En este paso creará un recurso de Face API que se puede usar para analizar las imágenes de una cámara.

## <a name="using-ai-to-analyze-images"></a>Uso de IA para analizar imágenes

La inteligencia artificial (IA) es donde los equipos pueden realizar tareas habitualmente asociadas con personas, no computadoras. Los equipos tienen la capacidad de aprender a hacer algo en lugar de que se les indique cómo hacerlo de manera explícita con programas escritos. Por ejemplo, es posible mostrarle a un equipo miles de imágenes de gatos para entrenarlo a reconocer estos animales. Después puede pasarle una imagen que nunca ha visto y le dirá si aparece algún gato. Esto se denomina Machine Learning o ML. Una vez que aprende, el equipo crea un modelo y este lo pueden volver a utilizar otros equipos para hacer la misma tarea.

Usted mismo puede entrenar los modelos de Machine Learning, o bien puede usar modelos creados por otros usuarios. Microsoft tiene disponible una variedad de estos modelos entrenados previamente llamada [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=hackwithazure-hackathon-cxa). Estos modelos incluyen el reconocimiento de imágenes, el reconocimiento de voz o la traducción entre distintos idiomas.

Uno de los modelos, [Face API](https://azure.microsoft.com/services/cognitive-services/face/?WT.mc_id=hackwithazure-hackathon-cxa), se puede usar para buscar caras en una imagen. Si encuentra alguna, puede adivinar la emoción que muestra esa cara (ya sea felicidad, tristeza, etc.), decir si la persona en cuestión sonríe, descubrir el color de pelo, si hay vello facial e, incluso, calcular la edad de las personas. Esta API la podemos usar en nuestra aplicación para buscar caras en la imagen cargada y predecir la emoción que se muestra en cada cara.

## <a name="sign-up-for-a-face-api-subscription-key"></a>Registro para una clave de suscripción de Face API

Antes de poder usar Face API, necesitará una clave de suscripción. Puede obtenerla en la cuenta de Azure que usó para implementar la aplicación web.

1. Abra Azure Portal en [portal.azure.com](https://portal.azure.com/?WT.mc_id=hackwithazure-hackathon-cxa). Inicie sesión si es necesario.

1. Seleccione *Crear un recurso* o el botón verde con el signo más.

1. Busque *Face*.
  
   ![Búsqueda de Face en Azure Portal](../images/SelectFaceInAzure.png)

1. Seleccione *Face* y, luego, el botón **Crear**.

1. Escriba los detalles necesarios:

   1. Asígnele un nombre. El nombre debe ser globalmente único porque será parte de una dirección URL a la que debe llamar si quiere buscar caras en la imagen.

   1. Seleccione la suscripción de Azure que quiere usar.

   1. Elija una ubicación donde ejecutar este código. Azure tiene "regiones" en todo el mundo. Una región es un grupo de centros de datos llenos de equipos y otros tipos de hardware de nube. Elija la región que le quede más cerca.

   1. Seleccione el plan de tarifa. Con esta aplicación, hará menos de 20 llamadas por minuto y menos de 30 000 llamadas al mes, así es que seleccione *F0*, el nivel Gratis. Hay un nivel de pago para las aplicaciones que necesitan usar el servicio con más frecuencia.

      > Solo puede tener un nivel Gratis de cada servicio de Azure, así es que si ya creó un recurso de Face API de nivel Gratis antes, tendrá que usar un nivel de pago o conectarse al recurso existente.

   1. Seleccione el grupo de recursos en el que quiere ejecutar el código. Se habría creado uno cuando implementó la aplicación web en un paso anterior con un nombre como `appsvc_linux_centralus`, así es que selecciónelo.

      > Todo lo que se crea en Azure, como el acceso a Face API, App Services y las bases de datos se denominan Recursos. Los grupos de recursos son una manera de agrupar los recursos para que pueda administrarlos en bloque. Tener todo esto en el mismo grupo de recursos para este taller facilitará la eliminación de todo el contenido cuando termine.

   1. Seleccione el botón **Crear**.

   ![La hoja Crear cara en Azure](../images/CreateFaceAzure.png)

1. Se creará el recurso Cara. Se le notificará cuando esté listo. Seleccione **Ir al recurso** en el menú emergente o en el panel de notificaciones.
  
   ![Notificación que muestra el recurso Cara creado](../images/FaceCreated.png)

1. Desde el recurso, vaya a la pestaña *Inicio rápido*. Anote los valores de *Key1* y *Endpoint*, porque los necesitará más adelante.

## <a name="create-configuration-variables-for-the-face-api-key-and-endpoint"></a>Creación de variables de configuración para la clave y el punto de conexión de Face API

Para usar Face API, necesitará la clave y el punto de conexión que copió anteriormente. Idealmente, no ponga la clave ni el punto de conexión de Face API en el código, sino que mejor guárdela en algún tipo de configuración.

Flask usa archivos denominados `.env` para almacenar los datos de configuración como pares clave-valor. Estos se leen en el inicio y quedan disponibles como variables de entorno que se pueden leer desde el código de Python. La ventaja de este método es que se puede crear la configuración de la aplicación en Azure App Service que se lee de la misma manera que las variables de entorno.

Debe colocar todas las claves secretas en el archivo `.env` y no debe comprobar los archivos `.env` en el control de código fuente. Flask puede acceder a los valores de este archivo con el paquete `python-dotenv`.

1. Abra el archivo `requirements.txt` en Visual Studio Code.

1. Agregue lo siguiente al final del archivo:

    ```python
    python-dotenv
    ```

1. Guarde el archivo.

1. Instale el paquete nuevo desde el terminal con el comando siguiente:
  
    ```sh
    pip install -r requirements.txt
    ```

1. Cree un archivo nuevo denominado `.env`.

1. Agregue estas líneas:

   ```sh
   face_api_endpoint=<endpoint>
   face_api_key=<key>
   ```

   Reemplace `<endpoint>` por la primera parte del punto de conexión que anotó antes en el inicio rápido del recurso de Face API. No necesita la parte de `/face/v1.0`.

   Por ejemplo, si el recurso se llamaba `HappySadAngryFaceApi`, el punto de conexión que aparece en el portal será `https://happysadangryfaceapi.cognitiveservices.azure.com/face/v1.0`. Establezca `face_api_endpoint` en `https://happysadangryfaceapi.cognitiveservices.azure.com`.

   Reemplace `<key>` una de las claves que anotó antes en el inicio rápido del recurso de Face API.

## <a name="deploy-the-configuration-variables-to-azure-app-service"></a>Implementación de las variables de configuración en Azure App Service

Para usar estos valores en la aplicación implementada, tendrá que agregarlos a la configuración de la aplicación para Azure App Service. Esto se puede hacer en Visual Studio Code.

1. Abra la paleta de comandos:
   1. En Windows, presione Ctrl+Mayús+P
   1. En MacOS, presione Cmd+Mayús+P

1. Seleccione *Azure App Service: Cargar configuración local…*

   ![Paleta de comandos que muestra Azure App Service: Opción Cargar configuración local](../images/UploadLocalSettings.png)

1. Seleccione el archivo `.env` que creó.

   ![Selección del archivo env local](../images/SelectEnvFile.png)

1. Seleccione la suscripción de Azure para la aplicación web.
  
   ![Paleta de comandos que muestra la opción Seleccionar suscripción](../images/SelectDeploySubscription.png)

1. Seleccione la aplicación web en la que se realizó la implementación.

La configuración se implementará en la aplicación web. Cuando termine, verá una ventana emergente.

## <a name="verify-the-application-settings"></a>Comprobación de la configuración de la aplicación

Puede ver la configuración de la aplicación en la pestaña Azure de la barra de herramientas de Visual Studio Code.

1. En la sección *App Service*, expanda su suscripción.

1. Expanda la sección *Configuración de la aplicación*.

Debería ver los dos nuevos valores de configuración de la aplicación, `face_api_endpoint` y `face_api_key` con los valores ocultos. Ahora puede hacer clic en los valores para mostrarlos y ocultarlos.

## <a name="next-step"></a>Siguiente paso

En este paso creó un recurso de Face API que se puede usar para analizar las imágenes de una cámara. En el [paso siguiente](./CheckTheEmotion.md), creará el juego para capturar fotogramas de la cámara y buscar emociones.
