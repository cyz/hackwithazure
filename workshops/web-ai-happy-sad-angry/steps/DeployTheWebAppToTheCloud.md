# <a name="deploy-the-web-app-to-the-cloud-using-azure-app-service"></a>Implementación de la aplicación web en la nube con Azure App Service

En el [paso anterior](./CreateTheWebPage.md) se creó la página web del juego, donde se reflejará la emoción que está tratando de transmitir y se capturarán imágenes de la cámara. En este paso, implementará esta aplicación web en la nube mediante Azure App Service.

## <a name="running-web-apps-in-the-cloud"></a>Ejecución de aplicaciones web en la nube

En el último paso, ejecutó la aplicación web en el equipo local. Después de esto, podrá ver la página web, pero esta no estará disponible para nadie más. Para que lo esté, debe ejecutarla en un equipo al que se pueda acceder a través de Internet. Los servicios en la nube permiten implementar fácilmente los sitios web en los equipos que se ejecutan en la nube. El servicio en la nube disponible en Microsoft se denomina [Azure](https://azure.microsoft.com/?WT.mc_id=hackwithazure-hackathon-cxa).

La ejecución de sitios web solía suponer mucho trabajo. Sería necesario configurar un equipo conectado a Internet, instalar software para hospedar el sitio web, conectarlo a Internet con un nombre de dominio, configurar la seguridad para evitar el acceso de los hackers y asegurarse de que se realiza una copia de seguridad de todo por si se produce alguna interrupción. En los sitios con mucho tráfico, es posible que desee configurar varios equipos para distribuir la carga, además de servicios para garantizar que un equipo no se sobrecargue debido a la administración de solicitudes. Ahora, gracias a los servicios en la nube, puede implementar el código simplemente y dejar que el proveedor de servicios en la nube haga todo lo demás.

Con la suscripción a Azure, podrá implementar el código en la nube, y Azure se encargará de abordar todas las complejidades derivadas del uso de [Azure App Service](https://azure.microsoft.com/services/app-service/?WT.mc_id=hackwithazure-hackathon-cxa). Debe configurar una instancia de App Service y enviarle el código, y Azure se encargará del resto.

## <a name="deploying-to-an-app-service"></a>Implementación en una instancia de App Service

Puede configurar una instancia de Azure App Service e implementar el código desde dentro de Visual Studio Code.

1. Abra la paleta de comandos:
  1. En Windows, presione Ctrl+Mayús+P.
  1. En MacOS, presione Cmd+Mayús+P.

1. Seleccione *Azure App Service: Implementar en aplicación web...*
  
   ![Paleta de comandos que muestra la opción Azure App Service: Implementar en aplicación web](../images/CommandPaletteDeployAppService.png)

1. Se le preguntará qué código desea implementar. Esta opción seleccionará automáticamente la carpeta que contiene el código, por lo que debe seleccionarla.

   ![Paleta de comandos que muestra la opción Origen de implementación](../images/SelectDeployFolder.png)

1. Si nunca ha iniciado sesión en Azure desde Visual Studio Code, se le pedirá que lo haga.
   1. Seleccione *Inicie sesión en Azure...*
   1. Se abre el explorador para que pueda iniciar sesión con la cuenta de Azure.
   1. Una vez iniciada la sesión desde el explorador, puede cerrar la página web que se abrió.

1. Seleccione la suscripción de Azure que quiere usar.
  
   ![Paleta de comandos que muestra la opción Seleccionar suscripción](../images/SelectDeploySubscription.png)

1. Seleccione *+ Crear aplicación web*.

   Hay dos opciones *Crear aplicación web*, una de ellas marcada como *Avanzada*. Desea utilizar la normal, **no** la *Avanzada*.

   ![Paleta de comandos que muestra la opción Crear aplicación web](../images/CreateNewWebApp.png)

1. Asigne un nombre a la aplicación web. Este formará parte de la dirección del sitio web público, por lo que debe ser único en todo el mundo. Por ejemplo, podría usar `jimspythonwebapp2019`.

   ![Paleta de comandos que muestra la opción Nuevo nombre de aplicación web](../images/SelectWebAppName.png)

1. Seleccione el tiempo de ejecución de la aplicación de App Service. Se trata de una aplicación de Python, por lo que debe seleccionar la última versión de Python en tiempo de ejecución, como *Python 3.7*.

   ![Paleta de comandos que muestra la opción Seleccionar tiempo de ejecución](../images/SelectPythonRuntime.png)

1. Se empezará a crear la aplicación de App Service. Verá una barra de progreso en la parte inferior derecha, que le indicará cuándo se ha completado el proceso. Puede supervisar el progreso en la ventana *Salida*; para ello, seleccione *Ver -> Salida* y, después, *Azure App Service* en el selector de ventanas.

   ![Barra de progreso de Crear servicio de aplicaciones](../images/CreateWebAppProgress.png)

1. Aparecen algunos elementos emergentes en los que se pregunta si desea realizar cambios de configuración para acelerar la implementación e implementar siempre esta aplicación web. Seleccione **Sí** para ambas opciones.
  
   ![Cuadro de diálogo Actualizar configuración del área de trabajo](../images/UpdateWorkspaceConfigDialog.png)
  
   ![Cuadro de diálogo de configuración Implementar siempre en la aplicación web](../images/AlwaysDeployDialog.png)

1. Aparecerá un elemento emergente que muestra el progreso de la implementación. Puede supervisar el progreso en la ventana *Salida*; para ello, seleccione *Ver -> Salida* y, después, *Azure App Service* en el selector de ventanas.
  
   ![Cuadro de diálogo Progreso de la implementación](../images/DeployProgress.png)

1. Una vez implementado el código, podrá verlo a través de Internet. Inicie el explorador y abra el sitio web. La dirección será `https://<web app name>.azurewebsites.net/`. Por ejemplo, para mi sitio web es `https://jimspythonwebapp2019.azurewebsites.net/`. Verá la página del juego.

> Con estos pasos, se creará un nivel Gratis de App Service e implementará la aplicación en él. Solo puede tener un nivel Gratis por suscripción, por lo que si ya tiene un servicio de aplicaciones gratuito en ejecución, use la opción *Crear aplicación web avanzada* y cree con un nivel de pago. No olvide eliminar esto después de finalizar el taller para dejar de usar sus créditos.

## <a name="resource-groups"></a>Grupos de recursos

En Azure, los recursos como App Service o los servicios de inteligencia artificial deben pertenecer a un grupo de recursos. Los grupos de recursos son agrupaciones lógicas de recursos, agrupados de tal forma que tenga sentido. Cada recurso debe pertenecer a un solo grupo de recursos.

Los grupos de recursos son útiles, ya que se pueden usar para agrupar los recursos que componen una aplicación. Puede ver todos los recursos del grupo y administrarlos juntos; por ejemplo, si desea eliminar todos los recursos, puede eliminar el grupo de recursos y, de esta forma, se eliminarán todos los recursos que contenga.

Una vez creada la instancia de App Service, esta se crearía dentro de un nuevo grupo de recursos, probablemente con un nombre parecido a `appsvc_linux_centralus`. Agregaremos todos los recursos nuevos a este grupo de recursos en el resto de este taller y eliminaremos este grupo de recursos al final para quitar todos los recursos.

## <a name="next-step"></a>Siguiente paso

En este paso, implementó la aplicación web en la nube, hospedada en Azure. En el [paso siguiente](./CreateAFaceResource.md), creará un recurso de Face API que se puede usar para analizar las imágenes de una cámara.
