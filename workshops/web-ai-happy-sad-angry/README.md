# <a name="happy-sad-angry-workshop"></a>Taller sobre feliz, triste, enfadado

Este taller muestra cómo compilar un juego basado en web denominado **Feliz, triste, enfadado** con [Python](https://www.python.org) y [Flask](http://flask.pocoo.org) junto con algo de HTML y JavaScript que se ejecuta en [Microsoft Azure](https://azure.microsoft.com/free/students/?WT.mc_id=hackwithazure-hackathon-cxa). Este juego selecciona una emoción y el jugador tiene que tratar de reflejar esa emoción con la cara. Cuando lo haya hecho, tome una fotografía con la cámara y el juego web comprobará cuál es esa emoción mediante la [API Face de Azure](https://azure.microsoft.com/services/cognitive-services/face/?WT.mc_id=hackwithazure-hackathon-cxa). Si es la misma que le pidieron que representara, ganará.

![Los resultados del juego muestran una victoria y una derrota.](./images/GameResults.png)

Este taller está diseñado para estudiantes y se puede ejecutar con los servicios gratuitos disponibles como parte de la oferta [Azure para estudiantes](https://azure.microsoft.com/free/students/?WT.mc_id=hackwithazure-hackathon-cxa).

## <a name="prerequisites"></a>Requisitos previos

Para completar este taller, necesitará lo siguiente:

* Una cuenta de Azure. Regístrese gratuitamente en [Azure para estudiantes](https://azure.microsoft.com/free/students/?WT.mc_id=hackwithazure-hackathon-cxa) o consiga la [cuenta gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=hackwithazure-hackathon-cxa) si no está en una institución académica.

* Python

  * **Windows:**

    Puede instalar Python desde la [Tienda Windows](https://www.microsoft.com/p/python-38/9mssztt1n39l?activetab=pivot:overviewtab&WT.mc_id=hackwithazure-hackathon-cxa). Esto configura Python correctamente en PATH sin necesidad de seguir más pasos.

    Si no desea usar la tienda, puede instalarlo desde la página de [descargas de Python](https://www.python.org/downloads/). Si lo hace, asegúrese de activar la opción *Add Python to PATH* (Agregar Python a la ruta de acceso).

    ![Cuadro de diálogo del instalador de Python que resalta la opción para agregar Python 3 8 a PATH](./images/PythonInstaller.png)

  * **macOS**
  
    Puede instalar Python desde la [página de descargas de Python](https://www.python.org/downloads/).

    Cuando Python se instale, se abrirá una ventana de Finder. Ejecute los siguientes scripts desde esa ventana de Finder para configurar los certificados y agregar Python a PATH.

    1. `Update Shell Profile.command`
    1. `Install Certificates.command`

* [Visual Studio Code](https://code.visualstudio.com/?WT.mc_id=hackwithazure-hackathon-cxa)

* La [extensión de Python para Visual Studio Code](https://marketplace.visualstudio.com/itemdetails?itemName=ms-python.python&WT.mc_id=hackwithazure-hackathon-cxa). Se puede instalar desde dentro VS Code mediante la pestaña *Extensiones*.
  
  ![La extensión de Python en Visual Studio Code](./images/PythonExtension.png)

* La [extensión de Azure App Service para Visual Studio Code](https://marketplace.visualstudio.com/itemdetails?itemName=ms-azuretools.vscode-azureappservice&WT.mc_id=hackwithazure-hackathon-cxa). Se puede instalar desde dentro VS Code mediante la pestaña *Extensiones*.
  
  ![La extensión de Azure App Service en Visual Studio Code.](./images/AppServiceExtension.png)

* Un portátil con una cámara web o una cámara externa.

## <a name="code-format"></a>Formato de código

Todo el código de este ejemplo se muestra en bloques de código como este:

```python
print('Here is some code')
```

Los puntos suspensivos se usarán para indicar otro código o se eliminarán para crear código nuevo o facilitar la vista del código que se está tratando. Por ejemplo, un bloque de código similar al siguiente:

```python
def func():
  ...
  print('end of the suite')
```

significa que `print('end of the suite')` deberá ir dentro de la función `func`, pero *después* de todo el código existente en esta función.

## <a name="steps"></a>Pasos

Los pasos de este taller son los siguientes:

1. [Crear la aplicación web de Flask](./steps/CreateAFlaskWebApp.md)
1. [Crear una página web para el juego](./steps/CreateTheWebPage.md)
1. [Implementar la aplicación web en la nube con Azure App Service](./steps/DeployTheWebAppToTheCloud.md)
1. [Crear un recurso de Face API](./steps/CreateAFaceResource.md)
1. [Crear el juego para capturar desde la cámara y comprobar la emoción](./steps/CheckTheEmotion.md)
1. [Limpiar](./Steps/CleanUp.md)

## <a name="code"></a>Código

Como referencia, puede encontrar el código final de este taller en la carpeta [Código](https://github.com/jimbobbennett/HappySadAngryWorkshop/tree/master/code).

## <a name="clean-up-azure-resources"></a>Limpieza de los recursos de Azure

Este taller utiliza recursos que están disponibles en la [cuenta de Azure for Students como servicios gratuitos](https://azure.microsoft.com/free/free-account-students-faq/?WT.mc_id=hackwithazure-hackathon-cxa). Como hay límites en el número de servicios gratuitos que puede crear, puede que desee eliminar los recursos creados una vez que haya terminado. Las instrucciones para hacerlo se encuentran en el último paso: [limpieza](./steps/CleanUp.md).
