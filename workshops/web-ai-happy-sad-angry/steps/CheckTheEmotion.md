# <a name="build-out-the-game-to-capture-from-the-camera-and-check-for-emotion"></a>Creación del juego para capturar desde la cámara y comprobar la emoción

En el [paso anterior](./CreateAFaceResource.md) creó un recurso de Face API que se puede usar para analizar las imágenes de una cámara. En este paso, creará el juego para capturar fotogramas de la cámara y buscar emociones.

## <a name="install-the-face-api-package"></a>Instalación del paquete de Face API

Face API está disponible como un paquete de Python.

1. Abra el archivo `requirements.txt` en Visual Studio Code.

1. Agregue lo siguiente al final del archivo:

   ```python
   azure-cognitiveservices-vision-face
   ```

1. Guarde el archivo.

1. Instale el paquete nuevo desde el terminal con el comando siguiente:
  
   ```sh
   pip install -r requirements.txt
   ```

## <a name="write-the-code"></a>Escritura del código

1. Abra el archivo `app.py` en Visual Studio Code.

1. Agregue importaciones para el paquete de Face API y un paquete de autenticación, así como algunas bibliotecas del sistema e importaciones adicionales del módulo de Flask a la parte superior del archivo.
  
   ```python
   import random, os, io, base64
   from flask import Flask, render_template, request, jsonify
   from azure.cognitiveservices.vision.face import FaceClient
   from msrest.authentication import CognitiveServicesCredentials
   ```

1. Agregue el código siguiente debajo de las importaciones:

   ```python
   credentials = CognitiveServicesCredentials(os.environ['face_api_key'])
   face_client = FaceClient(os.environ['face_api_endpoint'], credentials=credentials)

   def best_emotion(emotion):
     emotions = {}
     emotions['anger'] = emotion.anger
     emotions['contempt'] = emotion.contempt
     emotions['disgust'] = emotion.disgust
     emotions['fear'] = emotion.fear
     emotions['happiness'] = emotion.happiness
     emotions['neutral'] = emotion.neutral
     emotions['sadness'] = emotion.sadness
     emotions['surprise'] = emotion.surprise
     return max(zip(emotions.values(), emotions.keys()))[1]
   ```

1. Cree una nueva ruta para `/result` que acepte solicitudes POST en la parte inferior del archivo.

   ```python
   @app.route('/result', methods=['POST'])
   def check_results():
     body = request.get_json()
     desired_emotion = body['emotion']

     image_bytes = base64.b64decode(body['image_base64'].split(',')[1])
     image = io.BytesIO(image_bytes)

     faces = face_client.face.detect_with_stream(image,
                                                 return_face_attributes=['emotion'])

     if len(faces) == 1:
       detected_emotion = best_emotion(faces[0].face_attributes.emotion)

       if detected_emotion == desired_emotion:
         return jsonify({
           'message': '✅ You won! You showed ' + desired_emotion
         })
       else:
         return jsonify({
             'message': '❌ You failed! You needed to show ' +
                        desired_emotion +
                        ' but you showed ' +
                        detected_emotion
         })
     else:
       return jsonify({
         'message': '☠️ ERROR: No faces detected'
       })
   ```

1. Abra el archivo `home.html` de la carpeta `templates`.

1. Agregue el código siguiente dentro del agente de escucha de eventos en la etiqueta `<script>`. Colóquelo después del otro código que ya está en este agente de escucha.

    ```js
    var message = document.getElementById('message');

    document.getElementById('capture').addEventListener('click', () => {
    var canvas = document.createElement('canvas');
    canvas.width = 640;
    canvas.height = 480;
    var context = canvas.getContext('2d');
    context.drawImage(video, 0, 0, canvas.width, canvas.height);

    var data = {
        'image_base64': canvas.toDataURL("image/png"),
        'emotion': '{{ page_data.emotion }}'
    }

    const getResult = async () => {
        var result = await fetch('result', {
        method: 'POST',
        body: JSON.stringify(data),
        headers: { 'Content-Type': 'application/json' }
        })

        var jsonResult = await result.json()
        message.textContent = jsonResult.message
    }
    getResult()
    });
    ```

## <a name="deploy-the-code"></a>Implementación del código

Una vez que el código funcione de forma local, se puede implementar en Azure para que otros usuarios puedan jugar.

1. Abra la paleta de comandos:

   1. En Windows, presione Ctrl+Mayús+P.
   1. En MacOS, presione Cmd+Mayús+P.

1. Seleccione *Azure App Service: Implementar en aplicación web...*
  
   ![Paleta de comandos que muestra la opción Azure App Service: Implementar en aplicación web](../images/CommandPaletteDeployAppService.png)

1. Aparecerá un cuadro de diálogo en el que se le preguntará si desea sobrescribir la implementación existente. Seleccione el botón **Implementar**.
  
   ![Cuadro de diálogo para sobrescribir la implementación existente](../images/OverwriteDeploy.png)

1. Aparecerá un elemento emergente que muestra el progreso de la implementación. Puede supervisar el progreso en la ventana *Salida*; para ello, seleccione *Ver -> Salida* y, después, *Azure App Service* en el selector de ventanas.
  
   ![Cuadro de diálogo del progreso de la implementación](../images/DeployProgress.png)

1. Abra el sitio web en el explorador y practique con el juego.

## <a name="play-the-game"></a>Juego

Cada vez que cargue la página, se seleccionará una emoción diferente.

1. Mire a la cámara e intente reflejar la emoción solicitada.

1. Haga clic en el botón **Capture emotion** (Capturar emoción). Se capturará una imagen y se comprobará su emoción.

1. Verá una marca de verificación verde que indica que ha reflejado correctamente la emoción determinada, o bien una cruz roja si no lo ha hecho, junto con la emoción que Face API pensó que estaba mostrando. Si no hay ninguna cara en la imagen, verá un error.

   ![Los resultados del juego muestran una victoria y una derrota.](../images/GameResults.png)

## <a name="what-does-this-code-do"></a>¿Qué hace este código?

### <a name="the-apppy-file"></a>El archivo `app.py`

```python
import random, os, io, base64
from flask import Flask, render_template, request, jsonify
from azure.cognitiveservices.vision.face import FaceClient
from msrest.authentication import CognitiveServicesCredentials
```

Esto importa todos los módulos necesarios de los paquetes del sistema de Python y los paquetes instalados como parte de esta aplicación.

```python
credentials = CognitiveServicesCredentials(os.environ['face_api_key'])
face_client = FaceClient(os.environ['face_api_endpoint'], credentials=credentials)
```

Este código crea `FaceClient`: un objeto que se puede usar para tener acceso a la Face API de Azure. Necesita conectarse a un recurso de Face API específico, lo cual se configura mediante el punto de conexión del recurso que se creó y la clave de API a través de un objeto `CognitiveServicesCredentials` que contiene esa clave.

Los valores del punto de conexión y la clave se leen desde las variables de entorno. Cuando se ejecutan localmente, proceden del archivo `.env`, cuando se ejecutan en Azure App Service, proceden de la configuración de la aplicación.

```python
def best_emotion(emotion):
  emotions = {}
  emotions['anger'] = emotion.anger
  emotions['contempt'] = emotion.contempt
  emotions['disgust'] = emotion.disgust
  emotions['fear'] = emotion.fear
  emotions['happiness'] = emotion.happiness
  emotions['neutral'] = emotion.neutral
  emotions['sadness'] = emotion.sadness
  emotions['surprise'] = emotion.surprise
  return max(zip(emotions.values(), emotions.keys()))[1]
```

La información de emoción se envía como un conjunto de propiedades en la emoción con un valor que refleja la probabilidad de que esa emoción determinada se muestre en una escala de 0-1, siendo 0 no probable y 1 muy probable. Queremos encontrar la emoción con la mayor probabilidad, por lo que este código declara una función para ello colocando estos valores en un diccionario del nombre de la emoción para la probabilidad. El código busca en el diccionario de emociones y detecta la que tiene la mayor probabilidad, tomando su nombre como valor devuelto de la función `best_emotion`.

```python
@app.route('/result', methods=['POST'])
def check_results():
```

Esto declara una nueva función que se expone como un punto de conexión REST. La realización de una solicitud HTTP POST a `<website>/result` ejecutará esta función. Se llamará a este punto de conexión REST pasando un documento JSON que contiene la emoción que el jugador tiene que reflejar, así como su imagen almacenada como una cadena codificada en base64.

```python
body = request.get_json()
```

De este modo, se obtiene el JSON que se pasó a este punto de conexión REST cuando se le llama.

```python
desired_emotion = body['emotion']
image_bytes = base64.b64decode(body['image_base64'].split(',')[1])
```

Este código obtiene el valor de las propiedades `emotion` y `image_base64` en el JSON. El documento JSON que se espera tiene este formato:

```json
{
  "emotion" : "<the expected emotion>",
  "image_base64" : "<the image encoded as a data URL>"
}
```

La propiedad `image_base64` es la imagen codificada como una dirección URL de datos, que es una forma de codificar datos alineados en páginas web. En el archivo de plantilla, el código que extrae la imagen generará datos en este formato. El formato de una imagen es `data:image/png;base64,<base64 encoded data>`, donde `<base64 encoded data>` contiene la imagen codificada como una cadena base64. Para obtener la imagen, esta cadena debe dividirse en la coma, y solo debe usarse la sección de después. Una vez dividida, los datos codificados en base64 se descodifican en datos binarios.

```python
image = io.BytesIO(image_bytes)
```

Esto convierte los datos de la imagen binaria en un flujo.

```python
faces = face_client.face.detect_with_stream(image,
                                            return_face_attributes=['emotion'])
```

Llama a Face API, pasando el flujo de imágenes. Esta API puede devolver una serie de características para cada una de las caras detectadas y, en esta llamada, solo queremos la emoción, indicada por el parámetro `return_face_attributes`.

```python
if len(faces) == 1:
  ...
else:
return jsonify({
  'message': '☠️ ERROR: No faces detected'
})
```

Comprueba el número de caras detectadas y devuelve un error si no se encuentra ninguna. Esta función devuelve datos como JSON en el formato siguiente:

```json
{
  "message" : "<message>"
}
```

El valor de `<message>` se establece en lo que sea necesario mostrar al jugador. En este caso, el mensaje es un error que indica que no se han detectado caras.

```python
detected_emotion = best_emotion(faces[0].face_attributes.emotion)
```

Esto busca la emoción con la probabilidad más alta en la cara detectada.

```python
if detected_emotion == desired_emotion:
  return jsonify({
    'message': '✅ You won! You showed ' + desired_emotion
  })
else:
  return jsonify({
    'message': '❌ You failed! You needed to show ' +
               desired_emotion +
               ' but you showed ' +
               detected_emotion
  })
```

Si la emoción detectada es la que se le pidió al jugador, el JSON devuelto incluye un mensaje que informa de que ganó; de lo contrario, el mensaje indica que se produjo un error.

### <a name="the-template-file"></a>El archivo de plantilla

```js
var message = document.getElementById('message');
```

Esto busca el elemento HTML `h1` que se utilizará para mostrar el mensaje por su identificador.

```js
document.getElementById('capture').addEventListener('click', () => {
  ...
});
```

Crea un agente de escucha para el evento `click` del botón `capture`.

```js
var canvas = document.createElement('canvas');
canvas.width = 640;
canvas.height = 480;
var context = canvas.getContext('2d');
context.drawImage(video, 0, 0, canvas.width, canvas.height);
```

Crea un lienzo HTML de tamaño 640x480 y dibuja una imagen en él desde el elemento de vídeo, capturando esencialmente la salida de la cámara.

```js
var data = {
  'image_base64': canvas.toDataURL("image/png"),
  'emotion': '{{ page_data.emotion }}'
}
```

Esto creó el documento JSON que se pasará a la ruta de `/result`, que contiene la imagen extraída del lienzo como una dirección URL de datos y la emoción que el jugador está intentando reflejar.

```js
const getResult = async () => {
  var result = await fetch('result', {
    method: 'POST',
    body: JSON.stringify(data),
    headers: { 'Content-Type': 'application/json' }
  })

  var jsonResult = await result.json()
  message.textContent = jsonResult.message
}
```

Esto declara una función asincrónica denominada `getResult` que realiza una solicitud POST REST a la ruta de `/result`, pasando el documento JSON. Una vez realizada la llamada, se extrae el JSON en el resultado y el valor `message` de ese documento JSON se establece como texto para el elemento HTML del mensaje, mostrando el resultado.

```js
getResult()
```

Esto llama a la función `getResult`.

## <a name="next-step"></a>Siguiente paso

En este paso, se crea el juego para capturar fotogramas de la cámara y buscar emociones. En el [paso siguiente](./CleanUp.md) se limpiarán los recursos de Azure.
