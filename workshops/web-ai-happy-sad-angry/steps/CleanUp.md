# <a name="clean-up"></a>Limpieza

En el [paso anterior](./CheckTheEmotion.md), creo el juego para capturar fotogramas de la cámara y buscar emociones. En este paso limpiará los recursos de Azure.

## <a name="deleting-the-resource-group"></a>Eliminación del grupo de recursos

Todo lo que ha creado para este taller debería estar en el mismo grupo de recursos. Este grupo de recursos se creó la primera vez que implementó Azure App Service y se usó cuando creó la Face API.

Si elimina este grupo de recursos se eliminarán todos los recursos creados.

1. Abra Azure Portal en [portal.azure.com](https://portal.azure.com/?WT.mc_id=hackwithazure-hackathon-cxa). Inicie sesión si es necesario.

1. En la barra de búsqueda que está arriba, busque el grupo de recursos que se creó. Selecciónelo en los resultados.
  
   ![Búsqueda del grupo de recursos en Azure](../images/SearchForResourceGroup.png)

1. Seleccione **Eliminar el grupo de recursos**.
  
   ![El botón Eliminar el grupo de recursos](../images/DeleteResourceGroupButton.png)

1. Aparecerá el panel de confirmación en el que se muestran todos los recursos que se eliminarán junto con el grupo de recursos.

   ![Confirmación de la eliminación del grupo de recursos](../images/DeleteResourceGroupConfirm.png)

1. Escriba el nombre del grupo de recursos y seleccione *Eliminar*.

Se eliminarán todos los recursos de este grupo.