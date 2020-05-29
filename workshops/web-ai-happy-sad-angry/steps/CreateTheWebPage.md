# <a name="create-a-web-page-for-the-game"></a>Crear una página web para el juego

En el [paso anterior](./CreateAFlaskWebApp.md), creó una aplicación web de Flask sencilla en la que aparecía "Hola mundo" al ejecutarse. En este paso, creará la página web del juego, donde se reflejará la emoción que está tratando de transmitir y se capturarán imágenes de la cámara.

## <a name="rendering-html"></a>Representación de HTML

Para que se muestre una página web del juego, la aplicación de Flask debe mostrar HTML. Este código HTML debe ser configurable para incluir una emoción seleccionada aleatoriamente.

Hay dos formas de devolver una página HTML desde una aplicación de Flask. Puede devolver texto o HTML sin formato, o puede usar plantillas. Las plantillas permiten definir HTML que puede mostrar datos de alguna manera y usar esa plantilla con datos para crear una página HTML. Por ejemplo, si quisiera mostrar la página con una lista de nombres, podría especificar una plantilla que recorriera en iteración la lista y devolviera elementos de lista y, a continuación, usar esa plantilla con algunos datos reales para crear la lista en HTML.

La conversión de una plantilla y datos a HTML se denomina *Representación*.

## <a name="create-the-template"></a>Crear la plantilla

Las plantillas residen en una carpeta denominada `templates`.

1. Cree una nueva carpeta en Visual Studio Code dentro de la carpeta de la aplicación. Para ello, seleccione el botón *Nueva carpeta* de la pestaña *Explorador*.
  
   ![El botón Nueva carpeta](../images/VSCodeNewFolder.png)

1. Asigne a esta carpeta el nombre `templates`.

1. Creación de un nuevo archivo en esta carpeta denominado `home.html`

1. Agregue el siguiente código a este archivo:

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <title>Happy, Sad, Angry</title>
     </head>
     <body>
       <h1>Give me your best {{ page_data.emotion }} face</h1>
       <video id="video" autoplay></video>
       <br/>
       <button id="capture">Capture emotion</button>
       <h1 id="message"></h1>

       <script type="text/javascript">
         window.addEventListener("DOMContentLoaded", () => {
           var video = document.getElementById('video');

           if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
             const getImage = async () => {
               video.srcObject = await navigator.mediaDevices.getUserMedia({ video: true })
               video.play();
             }
             getImage()
           }
         })
       </script>
     </body>
   </html>
   ```

1. Guardar el archivo

1. Abrir `app.py`

1. Al principio del archivo, importe el miembro `render_template` desde el módulo de Flask agregándolo a la instrucción de importación existente. Además, agregue una importación para el módulo `random`.
  
   ```python
   import random
   from flask import Flask, render_template
   ```

1. Agregue una lista de posibles emociones para que el jugador las muestre bajo las instrucciones de importación.

   ```python
   emotions = ['anger','contempt','disgust','fear','happiness','sadness','surprise']
   ```

   Esta lista de emociones se basa en las emociones que puede detectar la API Face de Azure Cognitive Services.

1. Reemplace la función `home` por el siguiente código:
  
   ```python
   @app.route('/')
   def home():
     page_data = {
       'emotion' : random.choice(emotions)
     }
     return render_template('home.html', page_data = page_data)
   ```

1. Guarde el archivo.

## <a name="run-the-code"></a>Ejecutar el código

La ejecución de este código se puede realizar de dos maneras:

1. En el panel Depurar de la barra de herramientas, seleccione el botón verde *Iniciar depuración*.

   Si usa este método, podrá establecer puntos de interrupción y depurar el código.

1. En el terminal, ejecute el archivo como aplicación de Flask mediante:
  
   ```sh
   flask run
   ```

  Si usa este método, no podrá establecer puntos de interrupción ni depurar el código.

La aplicación web se ejecutará y se puede obtener acceso a ella desde el dispositivo en [http://127.0.0.1:5000](http://127.0.0.1:5000). Verá esta dirección URL en la ventana de salida y puede usar **Ctrl+clic** para ir directamente a este sitio.

1. Abra esta dirección URL en un explorador web para ver la página web del juego. Es posible que se le pida permiso para que la página tenga acceso a la cámara. Si esto ocurre, es posible que se le pregunte cada vez si desea permitir siempre que se guarde esto. En la página, verá una solicitud de una emoción aleatoria y una fuente en directo de la cámara.

   ![La página del juego en la que se solicita una emoción y se muestra una fuente de la cámara](../images/GameWebPageRunningLocally.png)

1. Detenga el depurador una vez que lo haya probado.

## <a name="what-does-this-code-do"></a>¿Qué hace este código?

### <a name="the-template-file"></a>El archivo de plantilla

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Happy, Sad, Angry</title>
  </head>
  <body>
  ...
  </body>
</html>
```

Se trata de un archivo HTML estándar con un cuerpo y el título "Feliz, triste,enfadado", el nombre de este juego.

```html
<h1>Give me your best {{ page_data.emotion }} face</h1>
```

Este código coloca un encabezado en la pantalla para mostrar la emoción que el jugador tiene que mostrar. La parte `{{ page_data.emotion }}` indica que se trata de un valor que se representará mediante datos pasados al representador de Flask. Buscará una propiedad pasada llamada `page_data`, donde encontrará un campo denominado `emotion` e insertará ese valor en el encabezado. Por ejemplo, si el valor de `page_data.emotion` era `"angry"`, se mostraría `Give me your best angry face` en el encabezado.

```html
<video id="video" autoplay></video>
<br/>
<button id="capture">Capture emotion</button>
<h1 id="message"></h1>
```

Esto define algunos elementos HTML estándar, es decir, un reproductor de vídeo, un botón y otro encabezado en el que se mostrará el resultado. Se asigna un nombre a estos elementos estableciendo su propiedad `id` de modo que se pueda tener acceso a ellos en el código en un paso posterior.

```html
<script type="text/javascript">
...
</script>
```

Esto define un bloque de código JavaScript dentro del código HTML.

```js
window.addEventListener("DOMContentLoaded", function() {
  ...
})
```

Define un evento que se ejecuta una vez que la página se ha cargado completamente en el explorador.

```js
var video = document.getElementById('video');
```

Este código obtendrá el reproductor de vídeo del código HTML basado en su `id` de `video` y lo asignará a una variable.

```js
if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
  const getImage = async () => {
    video.srcObject = await navigator.mediaDevices.getUserMedia({ video: true })
    video.play();
  }
  getImage()
}
```

Este código usará las API de dispositivos multimedia de JavaScript para tener acceso a la cámara local del explorador si la hay. Establecerá el origen del elemento de vídeo para que sea la cámara y empezará a reproducirlo. Esto mostrará una secuencia continua de vídeo de la cámara en la página.

### <a name="the-apppy-file"></a>El archivo `app.py`

```python
import random
from flask import Flask, render_template
```

Esto indica al compilador de Python que queremos usar código en el módulo `render_template` que se instaló como parte del paquete `flask`, así como el módulo `random` que se incluye como parte de Python.

```python
emotions = ['anger','contempt','disgust','fear','happiness','sadness','surprise']
```

Asimismo, define una lista de las posibles emociones que el juego le pedirá que muestre

```python
page_data = {
  'emotion' : random.choice(emotions)
}
```

y un diccionario de valores que se van a pasar a la plantilla HTML.

```python
return render_template('home.html', page_data = page_data)
```

Además, representará el código HTML del archivo de plantilla `home.html`, pasando el diccionario `page_data` como un parámetro llamado `page_data`. Los tokens de la plantilla HTML que hacen referencia a `page_data` harán referencia a los valores pasados en este campo.

## <a name="next-step"></a>Siguiente paso

En este paso ha creado la página web del juego, donde se reflejará la emoción que está tratando de transmitir y se capturarán imágenes de la cámara. En el [paso siguiente](./DeployTheWebAppToTheCloud.md), implementará esta aplicación web en la nube mediante Azure App Service.
