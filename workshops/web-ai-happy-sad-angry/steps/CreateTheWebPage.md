# <a name="create-a-web-page-for-the-game"></a>Criar uma página da Web para o jogo

Na [etapa anterior](./CreateAFlaskWebApp.md), você criou um aplicativo Web Flask simples que mostrou 'Olá, Mundo' ao ser executado. Nesta etapa, você criará a página da Web para o jogo que mostra a emoção que você está tentando transmitir e captura imagens da câmera.

## <a name="rendering-html"></a>Renderização de HTML

Para mostrar uma página da Web para o jogo, o aplicativo Flask precisa apresentar algum HTML. Esse HTML precisa ser configurável para incluir uma emoção selecionada aleatoriamente.

Há duas maneiras de retornar uma página HTML de um aplicativo Flask. Você pode retornar texto ou HTML bruto, ou pode usar modelos. Os modelos permitem que você defina trechos HTML capazes de mostrar dados de alguma forma e que use esse modelo com dados para criar uma página HTML. Por exemplo, se quisesse mostrar uma página com uma lista de nomes, você poderia especificar um modelo que iterou a lista e retornou itens de lista, então usar esse modelo com alguns dados reais para criar a lista em HTML.

A conversão de um modelo e de dados em HTML é chamada de *Renderização*.

## <a name="create-the-template"></a>Criar o modelo

Os modelos residem em uma pasta chamada `templates`.

1. Crie uma pasta no Visual Studio Code, dentro da sua pasta de aplicativos. Para fazer isso, selecione o botão *Nova pasta* na guia *Explorador*.
  
   ![O botão Nova pasta](../images/VSCodeNewFolder.png)

1. Dê a esta pasta o nome `templates`.

1. Criar um arquivo nesta pasta chamado `home.html`

1. Adicione o seguinte código a este arquivo:

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

1. Salvar o arquivo

1. Abrir `app.py`

1. No início do arquivo, importe o membro de `render_template` do módulo do Flask adicionando-o à instrução de importação existente. Além disso, adicione uma importação para o módulo `random`.
  
   ```python
   import random
   from flask import Flask, render_template
   ```

1. Adicione uma lista de emoções possíveis para o player mostrar abaixo das instruções de importação.

   ```python
   emotions = ['anger','contempt','disgust','fear','happiness','sadness','surprise']
   ```

   Essa lista de emoções baseia-se nas emoções que podem ser detectadas pela API de Detecção Facial dos Serviços Cognitivos do Azure.

1. Substitua a função `home` pelo seguinte código:
  
   ```python
   @app.route('/')
   def home():
     page_data = {
       'emotion' : random.choice(emotions)
     }
     return render_template('home.html', page_data = page_data)
   ```

1. Salve o arquivo.

## <a name="run-the-code"></a>Executar o código

Há duas formas de executar esse código:

1. No painel Depurar da barra de ferramentas, selecione o botão verde *Iniciar depuração*.

   Se usar esse método, você será capaz de definir pontos de interrupção e depurar seu código.

1. No terminal, execute o arquivo como um aplicativo do Flask usando:
  
   ```sh
   flask run
   ```

  Se usar esse método, você não será capaz de definir pontos de interrupção e depurar seu código.

O aplicativo Web será executado e poderá ser acessado em seu dispositivo em [http://127.0.0.1:5000](http://127.0.0.1:5000). Você verá essa URL na janela de saída e poderá usar **Ctrl + clique** para ir diretamente ao site em questão.

1. Abra essa URL em um navegador da Web para ver a página da Web do jogo. Pode ser solicitado que você forneça permissão para a página acessar a câmera. Se isso acontecer, recomendamos fortemente que permita sempre para evitar que essa pergunta se repita todas as vezes. Na página, você verá uma emoção aleatória sendo solicitada e um feed ao vivo da câmera.

   ![A página do jogo solicitando uma emoção e mostrando um feed da câmera](../images/GameWebPageRunningLocally.png)

1. Pare o depurador depois de testar isso.

## <a name="what-does-this-code-do"></a>O que este código faz?

### <a name="the-template-file"></a>O arquivo de modelo

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

Esse é um arquivo HTML padrão com um corpo e o título "Feliz, triste, irritado", o nome deste jogo.

```html
<h1>Give me your best {{ page_data.emotion }} face</h1>
```

Esse código coloca um cabeçalho na tela para mostrar a emoção que o jogador precisa transmitir. A parte `{{ page_data.emotion }}` indica que se trata de um valor que será processado usando dados passados para o renderizador do Flask. Ele vai procurar uma propriedade passada chamada `page_data` e, nela, encontrará um campo chamado `emotion` e inserirá esse valor no cabeçalho. Por exemplo, se o valor de `page_data.emotion` for `"angry"`, o cabeçalho mostrará `Give me your best angry face`.

```html
<video id="video" autoplay></video>
<br/>
<button id="capture">Capture emotion</button>
<h1 id="message"></h1>
```

Isso define alguns elementos HTML padrão, ou seja, um player de vídeo, um botão e outro cabeçalho que mostrará o resultado. Esses elementos são designados definindo sua propriedade `id` para que possam ser acessados no código em uma etapa posterior.

```html
<script type="text/javascript">
...
</script>
```

Isso define um bloco de código JavaScript dentro do HTML.

```js
window.addEventListener("DOMContentLoaded", function() {
  ...
})
```

Isso define um evento que é executado quando a página é totalmente carregada no navegador.

```js
var video = document.getElementById('video');
```

Esse código obterá o player de vídeo do HTML com base em `id` de `video` e o atribuirá a uma variável.

```js
if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
  const getImage = async () => {
    video.srcObject = await navigator.mediaDevices.getUserMedia({ video: true })
    video.play();
  }
  getImage()
}
```

Esse código usará as APIs de dispositivos de mídia do JavaScript para acessar a câmera local do navegador, se houver. Isso definirá a origem do elemento de vídeo como a câmera e a iniciará. Isso mostrará um fluxo contínuo de vídeo da câmera na página.

### <a name="the-apppy-file"></a>O arquivo `app.py`

```python
import random
from flask import Flask, render_template
```

Isso informa ao compilador do Python que desejamos usar o código no módulo `render_template` que foi instalado como parte do pacote de `flask`, bem como o módulo `random` que vem como parte do Python.

```python
emotions = ['anger','contempt','disgust','fear','happiness','sadness','surprise']
```

Isso define uma lista das possíveis emoções que o jogo solicitará que você apresente.

```python
page_data = {
  'emotion' : random.choice(emotions)
}
```

Isso define um dicionário de valores a serem passados ao modelo HTML.

```python
return render_template('home.html', page_data = page_data)
```

Isso renderizará o HTML do arquivo de modelo `home.html`, passando o dicionário `page_data` como um parâmetro chamado `page_data`. Os tokens do modelo HTML que se referem a `page_data` vão se referir aos valores passados nesse campo.

## <a name="next-step"></a>Próxima etapa

Nesta etapa, você criou a página da Web para o jogo que mostra a emoção que você está tentando transmitir e captura imagens da câmera. Na [próxima etapa](./DeployTheWebAppToTheCloud.md), você implantará esse aplicativo Web na nuvem usando o Serviço de Aplicativo do Azure.
