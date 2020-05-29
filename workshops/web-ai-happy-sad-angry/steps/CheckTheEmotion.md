# <a name="build-out-the-game-to-capture-from-the-camera-and-check-for-emotion"></a>Crie o jogo para capturar da câmera e verifique se há emoções

Na [etapa anterior](./CreateAFaceResource.md), você criou um recurso da API de Detecção Facial que pode ser usado para analisar as imagens da câmera. Nesta etapa, você criará o jogo para capturar quadros da câmera e procurar emoções.

## <a name="install-the-face-api-package"></a>Instale o pacote da API de Detecção Facial

A API de Detecção Facial está disponível como um pacote do Python.

1. Abra o arquivo `requirements.txt` no Visual Studio Code.

1. Adicione o seguinte ao final do arquivo:

   ```python
   azure-cognitiveservices-vision-face
   ```

1. Salvar o arquivo

1. Instale o novo pacote por meio do terminal usando o seguinte comando:
  
   ```sh
   pip install -r requirements.txt
   ```

## <a name="write-the-code"></a>Escrever o código

1. Abra o arquivo `app.py` no Visual Studio Code.

1. Adicione importações para o pacote da API de Detecção Facial e um pacote de autenticação, bem como algumas bibliotecas do sistema e importações adicionais do módulo Flask para a parte superior do arquivo.
  
   ```python
   import random, os, io, base64
   from flask import Flask, render_template, request, jsonify
   from azure.cognitiveservices.vision.face import FaceClient
   from msrest.authentication import CognitiveServicesCredentials
   ```

1. Adicione o seguinte código abaixo das importações:

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

1. Crie uma rota para `/result` que aceita solicitações POST na parte inferior do arquivo.

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

1. Abra o arquivo `home.html` da pasta `templates`.

1. Adicione o código a seguir dentro do ouvinte de eventos na marca `<script>`. Coloque-o após o outro código já neste ouvinte.

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

## <a name="deploy-the-code"></a>Implantar o código

Depois que o código estiver funcionando localmente, ele poderá ser implantado no Azure para que o jogo possa ser jogado por outras pessoas.

1. Abra a paleta de comandos:

   1. No Windows, pressione Ctrl + Shift + P
   1. No MacOS, pressione Cmd + Shift + P

1. Selecione *Serviço de Aplicativo do Azure: Implantar no aplicativo Web...*
  
   ![A paleta de comandos mostrando a opção do Serviço de Aplicativo do Azure: Opção Implantar no aplicativo Web](../images/CommandPaletteDeployAppService.png)

1. Uma caixa de diálogo será exibida perguntando se você deseja substituir a implantação existente. Selecione o botão **Implantar**.
  
   ![A caixa de diálogo Substituir implantação existente](../images/OverwriteDeploy.png)

1. Um pop-up mostrando o progresso da implantação será exibido. Você pode monitorar o progresso na janela *Saída* selecionando *Exibição > Saída* e selecionando *Serviço de Aplicativo do Azure* no seletor de janela.
  
   ![A caixa de diálogo Progresso da implantação](../images/DeployProgress.png)

1. Abra o site em seu navegador e jogue o jogo.

## <a name="play-the-game"></a>Jogue o jogo

Toda vez que você carregar a página, ela selecionará uma emoção diferente.

1. Examine sua câmera e faça o possível para mostrar a emoção solicitada.

1. Clique no botão **Capturar emoção**. Uma imagem será capturada e verificada para identificação da sua emoção.

1. Se você apresentou com êxito a emoção informada, verá uma marca de seleção verde; caso contrário, verá uma cruz vermelha acompanhada da emoção que API de Detecção Facial pensou que você mostrou. Se não houver rostos na imagem, você verá um erro.

   ![Os resultados do jogo mostrando um jogo ganho e um perdido](../images/GameResults.png)

## <a name="what-does-this-code-do"></a>O que este código faz?

### <a name="the-apppy-file"></a>O arquivo `app.py`

```python
import random, os, io, base64
from flask import Flask, render_template, request, jsonify
from azure.cognitiveservices.vision.face import FaceClient
from msrest.authentication import CognitiveServicesCredentials
```

Isso importa todos os módulos necessários dos pacotes de sistema do Python e os pacotes instalados como parte desse aplicativo.

```python
credentials = CognitiveServicesCredentials(os.environ['face_api_key'])
face_client = FaceClient(os.environ['face_api_endpoint'], credentials=credentials)
```

Esse código cria um `FaceClient`: um objeto que pode ser usado para acessar a API de Detecção Facial do Azure. Isso precisa se conectar a um recurso de API de Detecção Facial específico e é configurado usando o ponto de extremidade do recurso que foi criado e a chave de API, por meio de um objeto `CognitiveServicesCredentials` que encapsula essa chave.

Os valores para o ponto de extremidade e a chave são lidos de variáveis de ambiente. Quando executados localmente, eles são provenientes do arquivo `.env`, mas quando executados em um Serviço de Aplicativo do Azure, são provenientes das configurações do aplicativo.

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

As informações de emoção são enviadas como um conjunto de propriedades sobre a emoção com um valor para a probabilidade de que essa emoção específica esteja sendo mostrada em uma escala de 0 a 1, em que 0 representa pouca probabilidade e 1 representa uma probabilidade muito alta. Queremos encontrar a emoção que apresente a maior probabilidade, portanto, esse código declara uma função para fazer isso colocando esses valores como a probabilidade em um dicionário de nomes de emoção. O código examina o dicionário de emoções e detecta aquela com a maior probabilidade, retornando o nome dela como o valor retornado da função `best_emotion`.

```python
@app.route('/result', methods=['POST'])
def check_results():
```

Isso declara uma nova função que é exposta como um ponto de extremidade REST. Fazer uma solicitação HTTP POST para `<website>/result` executará essa função. Esse ponto de extremidade REST será chamado ao passar um documento JSON contendo a emoção que o jogador precisa mostrar, bem como a imagem do jogador armazenada como uma cadeia de caracteres codificada em base64.

```python
body = request.get_json()
```

Esse procedimento resulta na obtenção do JSON que foi passado para esse ponto de extremidade REST quando chamado.

```python
desired_emotion = body['emotion']
image_bytes = base64.b64decode(body['image_base64'].split(',')[1])
```

Esse código obtém o valor das propriedades `emotion` e `image_base64` no JSON. O documento JSON esperado está neste formato:

```json
{
  "emotion" : "<the expected emotion>",
  "image_base64" : "<the image encoded as a data URL>"
}
```

A propriedade `image_base64` é a imagem codificada como uma URL de dados, que é uma forma de codificar dados embutidos em páginas da Web. No arquivo de modelo, o código que extrai a imagem vai gerar dados nesse formato. O formato de uma imagem é `data:image/png;base64,<base64 encoded data>`, com o `<base64 encoded data>` que contém a imagem codificado como uma cadeia de caracteres em base64. Para obter a imagem, essa cadeia de caracteres precisa ser dividida com a vírgula como separador e apenas a seção depois dela deve ser usada. Depois da divisão, os dados codificados em base64 são decodificados em dados binários.

```python
image = io.BytesIO(image_bytes)
```

Isso converte os dados da imagem binária em um fluxo.

```python
faces = face_client.face.detect_with_stream(image,
                                            return_face_attributes=['emotion'])
```

Isso chama a API de Detecção Facial, passando o fluxo de imagem. Essa API pode retornar vários recursos para cada rosto detectado e, nesta chamada, queremos apenas a emoção, indicada pelo parâmetro `return_face_attributes`.

```python
if len(faces) == 1:
  ...
else:
return jsonify({
  'message': '☠️ ERROR: No faces detected'
})
```

Isso verifica quantos rostos foram detectados, retornando um erro se nenhum foi encontrado. Essa função retorna dados como JSON no seguinte formato:

```json
{
  "message" : "<message>"
}
```

O valor de `<message>` é definido como o que precisa ser exibido para o player. Nesse caso, a mensagem é um erro informando que nenhum rosto foi detectado.

```python
detected_emotion = best_emotion(faces[0].face_attributes.emotion)
```

Esse procedimento descobre a emoção com a maior probabilidade no rosto que foi detectado.

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

Se a emoção detectada for aquela que foi solicitada que o jogador mostrasse, o JSON retornado terá uma mensagem dizendo que o jogador ganhou; caso contrário, a mensagem indicará que o jogador perdeu.

### <a name="the-template-file"></a>O arquivo de modelo

```js
var message = document.getElementById('message');
```

Isso localiza o elemento HTML `h1` que será usado para mostrar a mensagem pela respectiva ID.

```js
document.getElementById('capture').addEventListener('click', () => {
  ...
});
```

Isso cria um ouvinte para o evento `click` do botão `capture`.

```js
var canvas = document.createElement('canvas');
canvas.width = 640;
canvas.height = 480;
var context = canvas.getContext('2d');
context.drawImage(video, 0, 0, canvas.width, canvas.height);
```

Isso cria uma tela HTML dimensionada em 640x480 e desenha uma imagem para ela com base no elemento de vídeo, essencialmente capturando a saída da câmera.

```js
var data = {
  'image_base64': canvas.toDataURL("image/png"),
  'emotion': '{{ page_data.emotion }}'
}
```

Isso criou o documento JSON que será passado para a rota `/result`, contendo a imagem extraída da tela como uma URL de dados e a emoção que o jogador está tentando mostrar.

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

Isso declara uma função Async chamada `getResult` que faz uma solicitação POST REST para a rota `/result`, passando o documento JSON. Depois que a chamada é feita, o JSON no resultado é extraído e o valor `message` nesse documento JSON é definido como o texto para o elemento HTML da mensagem, mostrando o resultado.

```js
getResult()
```

Essa chama a função `getResult`.

## <a name="next-step"></a>Próxima etapa

Nesta etapa, você criou o jogo para capturar quadros da câmera e procurar emoções. Na [próxima etapa](./CleanUp.md), você limpará os recursos do Azure.
