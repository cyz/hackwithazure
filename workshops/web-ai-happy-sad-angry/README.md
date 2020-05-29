# <a name="happy-sad-angry-workshop"></a>Workshop Feliz, Triste e Irritado

Este workshop mostra como criar um jogo baseado na Web chamado **Feliz, Triste e Irritado** usando o [Python](https://www.python.org) e o [Flask](http://flask.pocoo.org) em conjunto com HTML e JavaScript em execução no [Microsoft Azure](https://azure.microsoft.com/free/students/?WT.mc_id=hackwithazure-hackathon-cxa). O jogo seleciona uma emoção e você precisa se esforçar ao máximo para expressá-la em seu rosto. Quando estiver expressando a emoção da melhor maneira, você fará uma foto com a câmera e o jogo da Web verificará qual emoção você está expressando usando a [API de Detecção Facial do Azure](https://azure.microsoft.com/services/cognitive-services/face/?WT.mc_id=hackwithazure-hackathon-cxa). Se for a mesma emoção que você foi solicitado a demonstrar, você ganhará.

![Os resultados do jogo mostrando um jogo ganho e um perdido](./images/GameResults.png)

Este workshop foi desenvolvido para alunos e pode ser executado usando os serviços gratuitos disponibilizados como parte da oferta [Azure for Students](https://azure.microsoft.com/free/students/?WT.mc_id=hackwithazure-hackathon-cxa).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir o workshop, você precisará de:

* Uma conta do Azure. Inscreva-se gratuitamente usando o [Azure for Students](https://azure.microsoft.com/free/students/?WT.mc_id=hackwithazure-hackathon-cxa) ou a [conta gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=hackwithazure-hackathon-cxa) se não estiver matriculado em uma instituição acadêmica.

* Python

  * **Windows:**

    Você pode instalar o Python na [Microsoft Store](https://www.microsoft.com/p/python-38/9mssztt1n39l?activetab=pivot:overviewtab&WT.mc_id=hackwithazure-hackathon-cxa). Isso configura o Python corretamente em seu caminho e não há mais etapas.

    Se não quiser usar a Store, instale usando a [página de Downloads do Python](https://www.python.org/downloads/). Se fizer isso, marque a opção *Adicionar Python ao caminho*.

    ![A caixa de diálogo do instalador de Python destacando a opção Adicionar Python 3.8 ao caminho](./images/PythonInstaller.png)

  * **macOS**
  
    Você pode instalar o Python usando a [página de Downloads do Python](https://www.python.org/downloads/).

    Quando o Python for instalado, ele abrirá uma janela do Localizador. Execute os scripts a seguir dentro dessa janela do Localizador para configurar certificados e adicionar o Python ao caminho:

    1. `Update Shell Profile.command`
    1. `Install Certificates.command`

* [Visual Studio Code](https://code.visualstudio.com/?WT.mc_id=hackwithazure-hackathon-cxa)

* A [Extensão de Python para o Visual Studio Code](https://marketplace.visualstudio.com/itemdetails?itemName=ms-python.python&WT.mc_id=hackwithazure-hackathon-cxa). Ela pode ser instalada de dentro do VS Code usando a guia *Extensões*.
  
  ![A Extensão de Python no Visual Studio Code](./images/PythonExtension.png)

* A [Extensão do Serviço de Aplicativo do Azure para o Visual Studio Code](https://marketplace.visualstudio.com/itemdetails?itemName=ms-azuretools.vscode-azureappservice&WT.mc_id=hackwithazure-hackathon-cxa). Ela pode ser instalada de dentro do VS Code usando a guia *Extensões*.
  
  ![A extensão do Serviço de Aplicativo do Azure no Visual Studio Code](./images/AppServiceExtension.png)

* Um laptop com uma webcam ou uma câmera externa.

## <a name="code-format"></a>Formato de código

Todo o código neste exemplo é mostrado em blocos de código como este:

```python
print('Here is some code')
```

Reticências são usadas para indicar outro código, removido para criar código novo, ou para facilitar a visualização do código que está sendo discutido. Por exemplo, um bloco de código como este:

```python
def func():
  ...
  print('end of the suite')
```

significa que o `print('end of the suite')` precisará ficar dentro da função `func`, mas *depois* de todo o código existente nessa função

## <a name="steps"></a>Etapas

As etapas deste workshop são:

1. [Criar o aplicativo Web Flask](./steps/CreateAFlaskWebApp.md)
1. [Criar uma página da Web para o jogo](./steps/CreateTheWebPage.md)
1. [Implantar o aplicativo Web na nuvem usando o Serviço de Aplicativo do Azure](./steps/DeployTheWebAppToTheCloud.md)
1. [Criar um recurso da API de Detecção Facial](./steps/CreateAFaceResource.md)
1. [Criar o jogo para capturar da câmera e verificar se há emoções](./steps/CheckTheEmotion.md)
1. [Limpar](./Steps/CleanUp.md)

## <a name="code"></a>Código

Para referência, você pode encontrar o código final deste workshop na pasta [Código](https://github.com/jimbobbennett/HappySadAngryWorkshop/tree/master/code).

## <a name="clean-up-azure-resources"></a>Limpar recursos do Azure

Este workshop usa recursos disponibilizados na [Conta do Azure for Students como serviços gratuitos](https://azure.microsoft.com/free/free-account-students-faq/?WT.mc_id=hackwithazure-hackathon-cxa). Como há limites para o número de serviços gratuitos que você pode criar, talvez você queira excluir os recursos criados quando terminar. As instruções para fazer isso estão na última etapa, [Limpar](./steps/CleanUp.md).
