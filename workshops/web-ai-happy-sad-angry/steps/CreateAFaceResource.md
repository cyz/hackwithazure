# <a name="create-a-face-api-resource"></a>Criar um recurso da API de Detecção Facial

No [etapa anterior](./DeployTheWebAppToTheCloud.md), você implantou seu aplicativo Web na nuvem, hospedado no Azure. Nesta etapa, você criará um recurso da API de Detecção Facial que pode ser usado para analisar as imagens da câmera.

## <a name="using-ai-to-analyze-images"></a>Usando IA para analisar imagens

Com a IA, ou inteligência artificial, os computadores podem executar tarefas normalmente associadas a pessoas, e não a computadores. Os computadores podem aprender a fazer algo, em vez de serem instruídos sobre como fazê-lo por meio das instruções explícitas presentes nos programas. Por exemplo, um computador pode ser treinado para reconhecer gatos após serem mostradas a ele milhares de imagens de gatos. Em seguida, você pode fornecer a ele uma imagem que ele não viu antes e ele poderá dizer se há um gato na imagem. Isso se chama Machine Learning, ou ML. Depois de ensinado, o computador cria um modelo, que pode ser usado por outros computadores para executar a mesma tarefa.

Você pode treinar modelos de machine learning por conta própria ou pode usar modelos criados por outras pessoas. A Microsoft disponibiliza uma variedade de modelos pré-treinados, chamados [Serviços Cognitivos](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=hackwithazure-hackathon-cxa). Esses modelos incluem o reconhecimento de imagens, o reconhecimento de fala e a tradução entre idiomas.

Um dos modelos, a [API de Detecção Facial](https://azure.microsoft.com/services/cognitive-services/face/?WT.mc_id=hackwithazure-hackathon-cxa), pode ser usado para procurar rostos em uma imagem. Quando encontra um rosto, ela pode identificar qual emoção ele está expressando (felicidade, tristeza etc.), indicar se a pessoa está sorrindo, procurar a cor de cabelo, se há pelos no rosto e até mesmo estimar a idade da pessoa. Podemos usar essa API em nosso aplicativo para procurar rostos na imagem carregada e prever a emoção exibida em cada rosto.

## <a name="sign-up-for-a-face-api-subscription-key"></a>Inscrever-se para receber uma chave de assinatura da API de Detecção Facial

Para usar a API de Detecção Facial, você precisará de uma chave de assinatura. Ela pode ser obtida em sua conta do Azure usada para implantar o aplicativo Web.

1. Abra o portal do Azure em [portal.azure.com](https://portal.azure.com/?WT.mc_id=hackwithazure-hackathon-cxa). Faça logon se necessário.

1. Selecione *Criar um recurso* ou clique no botão de adição verde.

1. Pesquise por *Detecção Facial*
  
   ![Pesquisando por Detecção Facial no portal do Azure](../images/SelectFaceInAzure.png)

1. Selecione *Detecção Facial* e clique no botão **Criar**.

1. Insira os detalhes necessários:

   1. Forneça um nome. Ele precisa ser globalmente exclusivo, pois se tornará parte de uma URL que você precisa chamar para encontrar rostos na imagem.

   1. Selecione a assinatura do Azure que deseja usar.

   1. Escolha um local no qual o código será executado. O Azure tem "regiões" no mundo inteiro, sendo uma região um grupo de data centers repleto de computadores e outros hardwares de nuvem. Escolha a região mais próxima de você.

   1. Selecione o tipo de preço. Com esse aplicativo, você fará menos de 20 chamadas por minuto e menos de 30.000 por mês. Sendo assim, selecione *F0*, a camada gratuita. Há uma camada paga para aplicativos que precisam usar o serviço com mais frequência.

      > Você só pode ter uma camada gratuita de cada serviço do Azure, portanto, se já tiver criado um recurso da API de Detecção Facial na camada gratuita, precisará usar uma camada paga ou se conectar ao recurso existente.

   1. Selecione o grupo de recursos no qual deseja executar o código. Foi criado um grupo de recursos quando você implantou o aplicativo Web em uma etapa anterior chamada `appsvc_linux_centralus`, portanto, selecione-o.

      > Tudo que você cria no Azure, como o acesso à API de Detecção Facial, os Serviços de Aplicativos e os bancos de dados, é chamado de Recurso. Os grupos de recursos são uma forma de agrupar esses recursos para que você possa gerenciá-los em massa. Ter tudo o que é usado neste workshop no mesmo grupo de recursos facilita a exclusão de tudo no final, quando você terminar.

   1. Selecione o botão **Criar**.

   ![A folha criar detecção facial no Azure](../images/CreateFaceAzure.png)

1. O recurso de detecção facial será criado. Quando concluído, você será notificado. Selecione **Ir para o recurso** no pop-up ou no painel de notificação.
  
   ![Notificação mostrando que o recurso de detecção facial foi criado](../images/FaceCreated.png)

1. No recurso, vá para a guia *Início Rápido*. Anote os valores de *Key1* e *Ponto de extremidade*, pois você precisará deles mais tarde.

## <a name="create-configuration-variables-for-the-face-api-key-and-endpoint"></a>Criar variáveis de configuração para a chave e o ponto de extremidade da API de Detecção Facial

Para usar a API de Detecção Facial, você precisará da chave e do ponto de extremidade que copiou anteriormente. Idealmente, você não coloca a chave e o ponto de extremidade da API de Detecção Facial no código. Em vez disso, eles deve ser armazenados em algum tipo de configuração.

O Flask usa arquivos chamados `.env` para armazenar dados de configuração como pares de chave/valor. Eles são lidos na inicialização e são disponibilizados como variáveis de ambiente que podem ser lidas de seu código Python. A vantagem desse método é que você pode criar Configurações de Aplicativo em seu Serviço de Aplicativo do Azure, que são lidas da mesma maneira que as variáveis de ambiente.

Coloque todas as suas chaves secretas no arquivo `.env` e não faça check-in de nenhum arquivo `.env` no controle do código-fonte. O Flask pode acessar os valores nesse arquivo usando o pacote `python-dotenv`.

1. Abra o arquivo `requirements.txt` no Visual Studio Code.

1. Adicione o seguinte ao final do arquivo:

    ```python
    python-dotenv
    ```

1. Salve o arquivo

1. Instale o novo pacote por meio do terminal usando o seguinte comando:
  
    ```sh
    pip install -r requirements.txt
    ```

1. Crie um arquivo chamado `.env`.

1. Adicione as seguintes linhas:

   ```sh
   face_api_endpoint=<endpoint>
   face_api_key=<key>
   ```

   Substitua `<endpoint>` pela primeira parte do ponto de extremidade anotado anteriormente no Início Rápido do Recurso da API de Detecção Facial. Você não precisa da parte `/face/v1.0`.

   Por exemplo, se seu recurso se chamasse `HappySadAngryFaceApi`, o ponto de extremidade exibido no portal seria `https://happysadangryfaceapi.cognitiveservices.azure.com/face/v1.0`. Defina `face_api_endpoint` como `https://happysadangryfaceapi.cognitiveservices.azure.com`.

   Substitua `<key>` pela chave que você anotou anteriormente no Início Rápido do Recurso da API de Detecção Facial.

## <a name="deploy-the-configuration-variables-to-azure-app-service"></a>Implantar as variáveis de configuração no Serviço de Aplicativo do Azure

Para usar esses valores no aplicativo implantado, você precisará adicioná-los às Configurações de Aplicativo de seu Serviço de Aplicativo do Azure. Isso pode ser feito de dentro do Visual Studio Code.

1. Abra a paleta de comandos:
   1. No Windows, pressione Ctrl + Shift + P
   1. No MacOS, pressione Cmd + Shift + P

1. Selecione *Serviço de Aplicativo do Azure: Carregar Configurações Locais...*

   ![A paleta de comandos mostrando a opção do Serviço de Aplicativo do Azure: Carregar Configurações Locais](../images/UploadLocalSettings.png)

1. Selecione o arquivo `.env` que você criou.

   ![Selecionando o arquivo env local](../images/SelectEnvFile.png)

1. Selecione a assinatura do Azure para o aplicativo Web.
  
   ![A paleta de comandos mostrando a opção Selecionar Assinatura](../images/SelectDeploySubscription.png)

1. Selecione o aplicativo Web no qual a implantação foi feita.

As configurações serão implantadas no aplicativo Web. Você verá um pop-up quando o processo for concluído.

## <a name="verify-the-application-settings"></a>Verificar as Configurações de Aplicativo

Você pode exibir as configurações de aplicativo na guia Azure da barra de ferramentas do Visual Studio Code.

1. Na seção *Serviço de Aplicativo*, expanda sua assinatura.

1. Expanda a seção *Configurações de Aplicativo*.

Você deve ver as duas novas configurações de aplicativo, `face_api_endpoint` e `face_api_key`, com os valores ocultos. É possível exibir e ocultar os valores clicando neles

## <a name="next-step"></a>Próxima etapa

Nesta etapa, você criou um recurso da API de Detecção Facial que pode ser usado para analisar as imagens da câmera. Na [próxima etapa](./CheckTheEmotion.md), você criará o jogo para capturar quadros da câmera e procurar emoções.
