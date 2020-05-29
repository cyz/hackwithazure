# <a name="create-the-flask-web-app"></a>Crear la aplicación web de Flask

En este paso, creará una aplicación web de Flask.

## <a name="create-folders-for-the-project"></a>Crear carpetas para el proyecto

1. Cree una carpeta llamada `HappySadAngry` en algún lugar de la máquina.

## <a name="open-this-folder-in-visual-studio-code"></a>Abrir esta carpeta en Visual Studio Code

1. Inicie Visual Studio Code.

1. Abrir la carpeta creada recientemente
   1. En MacOS, seleccione *Archivo->Abrir...*
   1. En Windows, seleccione *Archivo->Abrir carpeta...*

1. Vaya a la carpeta `HappySadAngry` nueva y seleccione **Abrir**.

Verá que la carpeta vacía aparece en el *Explorador*.

## <a name="configure-a-virtual-environment"></a>Configurar un entorno virtual

Python se incluye en varias versiones y las aplicaciones de Python pueden usar código externo en paquetes instalados a través de una herramienta denominada `pip`. Esto puede dar lugar a problemas si diferentes aplicaciones necesitan versiones de paquete o de Python distintas. Para facilitar la prevención de incidencias con las versiones de paquete o de Python, se recomienda usar *entornos virtuales*, árboles de carpetas independientes que contienen una instalación de Python para una versión determinada de Python, además de una serie de paquetes adicionales.

1. Cree un nuevo archivo en la carpeta `HappySadAngry` llamado `app.py`. Este es el archivo que contendrá el código de la aplicación de Flask. Al crearlo, se activará la extensión de Python en Visual Studio Code. Seleccione el botón **Nuevo archivo** en el *Explorador*.

   ![El botón Nuevo archivo](../images/VSCodeNewFile.png)

1. Asignar al nuevo archivo el nombre `app.py` y presionar Entrar

   ![Asignación al archivo del nombre app.py](../images/NameAppPy.png)

   > Es posible que se le pida que instale un linter. Se trata de una herramienta que puede inspeccionar el código en busca de errores a medida que lo escribe. Si aparece una ventana emergente en la que se le pregunta si desea instalarlo, seleccione **Instalar**.

1. Al abrirse Visual Studio Code, el terminal debería abrirse de forma predeterminada. Si no lo hace, abra un nuevo terminal seleccionando *Terminal -> Nuevo terminal*.

1. Crear un nuevo entorno virtual llamado `.venv` con Python 3 ejecutando el siguiente comando en el terminal

   ```sh
   python3 -m venv .venv
   ```

1. Aparecerá un cuadro de diálogo en el que se le pregunta si desea activar este entorno virtual. Seleccione **Sí**.

   ![El cuadro de diálogo del entorno virtual](../images/LaunchVirtualEnv.png)

1. El terminal existente no tendrá el entorno virtual activado. Ciérrelo seleccionando el botón de la Papelera.

   ![El botón Terminar el terminal](../images/KillTerminal.png)

1. Cree un nuevo terminal seleccionando *Terminal -> Nuevo terminal*. El terminal cargará el entorno virtual

   ![El terminal que activa el entorno virtual](../images/TerminalWithActivatedEnvironment.png)

## <a name="install-the-flask-package"></a>Instalar el paquete de Flask

[Flask](http://flask.pocoo.org) es un micromarco de Python para crear Web Apps. Es ligero y fácil de usar para crear instancias de Web Apps sencillas. Está disponible como paquete de Python que se puede instalar con `pip`.

En lugar de instalarlo mediante `pip` del terminal, debe configurarse dentro de un archivo `requirements.txt`. En este archivo se enumeran todos los paquetes de los que depende una aplicación de Python y se necesitarán una vez que la aplicación web se implemente en la nube en un paso posterior. Este archivo indicará el servidor en la nube que ejecuta el sitio web en esos paquetes que deben instalarse antes de que pueda iniciarse la aplicación web.

1. Crear un nuevo archivo en la carpeta denominado `requirements.txt`

1. Agregue el siguiente paquete al archivo:
  
   ```python
   flask
   ```

1. Guardar el archivo

   > Si no desea tener que acordarse siempre de guardar los archivos, puede activar Guardado automático seleccionando *Archivo -> Guardado automático*.

1. Instale los paquetes desde el terminal con el siguiente comando:
  
   ```sh
   pip install -r requirements.txt
   ```

   Este instalará todos los paquetes en el archivo `requirements.txt`, omitiendo cualquiera que ya esté instalado.

## <a name="write-the-code"></a>Escribir el código

1. Abrir el archivo `app.py`

1. Agregue el siguiente código a este archivo:
  
    ```python
    from flask import Flask

    app = Flask(__name__)

    @app.route('/')
    def home():
      return 'Hello World'
    ```

1. Guardar el archivo

## <a name="run-the-code"></a>Ejecutar el código

Este código no se puede ejecutar desde el terminal con el comando `Python`, sino que debe ejecutarse como aplicación de Flask mediante el paquete de Flask. Existen dos formas de hacerlo:

1. En el panel Depurar de la barra de herramientas, despliegue el cuadro *Configuración de depuración* y seleccione *Python: Flask*.
  
   > Si no ve esta opción, seleccione *Agregar configuración* para editar el archivo `launch.json`. Esto debe crear un conjunto de opciones de inicio para los archivos de Python.
   >
   > Si estas opciones no se crean automáticamente, seleccione *Agregar configuración...* y la opción *Flask*. Guarde este archivo.
   >
   > Seleccione *Python: Flask* en el cuadro *Configuración de depuración*.

   Seleccione el botón verde *Iniciar depuración*.

   Si usa este método, podrá establecer puntos de interrupción y depurar el código.

1. En el terminal, ejecute el archivo como aplicación de Flask mediante:
  
   ```sh
   flask run
   ```

   Si usa este método, no podrá establecer puntos de interrupción ni depurar el código.

La aplicación web se ejecutará y se puede obtener acceso a ella desde el dispositivo en [http://127.0.0.1:5000](http://127.0.0.1:5000). Verá esta dirección URL en la ventana de salida y puede usar **Ctrl+clic** para ir directamente a este sitio.

1. Abra esta dirección URL en un explorador web para ver el mensaje `Hello World`.

   ![Un sitio web que muestra Hola mundo](../images/HelloWorldOnWebSite.png)

1. Detenga el depurador una vez que lo haya probado.

## <a name="what-does-this-code-do"></a>¿Qué hace este código?

```python
from flask import Flask
```

Esto indica al compilador de Python que queremos usar código en el módulo `Flask`. Este módulo se instaló como parte del paquete `flask`.

```python
app = Flask(__name__)
```

Esto crea una aplicación web de Flask llamada como el archivo, tenga el nombre que tenga. `__name__` es una variable especial de Python que devuelve el nombre del módulo actual, es decir, el archivo sin la extensión `.py`.

```python
@app.route('/')
def home():
```

Esto define una función llamada `home`. Esta función está asignada a una ruta llamada `/`. En una aplicación web, una ruta es la parte de la dirección URL que sigue al nombre de dominio y se pueden asignar rutas diferentes a páginas web distintas. `/` suele ser la página principal y puede haber tantas rutas como sea necesario. Por ejemplo, `/about` se enrutaría a una página Acerca de, mientras que `/basket` podría enrutarse a una cesta de la compra. Si el sitio web se encontraba en `http://www.mywebsite.com`, la ruta `/` es la que se utilizaría al señalar el explorador a `http://www.mywebsite.com`, `/about` se usaría al ir a `http://www.mywebsite.com/about`, etc.

```python
return 'Hello World'
```

El contenido de la función `home` se limita a devolver texto simple, que un explorador web representará como texto sin procesar.

## <a name="next-step"></a>Siguiente paso

En este paso, creó una aplicación web de Flask sencilla en la que aparecía "Hola mundo" al ejecutarse. En el [paso siguiente](./CreateTheWebPage.md), creará la página web del juego, donde se reflejará la emoción que está tratando de transmitir y se capturarán imágenes de la cámara.
