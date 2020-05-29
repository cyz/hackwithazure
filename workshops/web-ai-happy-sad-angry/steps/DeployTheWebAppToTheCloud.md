# <a name="deploy-the-web-app-to-the-cloud-using-azure-app-service"></a>Implantar o aplicativo Web na nuvem usando o Serviço de Aplicativo do Azure

Na [etapa anterior](./CreateTheWebPage.md), você criou a página da Web para o jogo que mostra a emoção que você está tentando transmitir e captura imagens da câmera. Nesta etapa, você implantará esse aplicativo Web na nuvem usando o Serviço de Aplicativo do Azure.

## <a name="running-web-apps-in-the-cloud"></a>Como executar aplicativos Web na nuvem

Na última etapa, você executou o aplicativo Web no computador local. Isso significa que você pode ver a página da Web, mas ela não está disponível para outras pessoas. Para disponibilizá-la, ela precisa ser executada em um computador em algum lugar que esteja acessível pela Internet. Os serviços de nuvem permitem que você implante facilmente seus sites da Web em computadores em execução na nuvem. O serviço de nuvem disponível da Microsoft é chamado [Azure](https://azure.microsoft.com/?WT.mc_id=hackwithazure-hackathon-cxa).

A execução de sites da Web costumava ser muito trabalhosa. Você precisaria configurar um computador conectado à Internet, instalar o software para hospedar o site, conectá-lo à Internet com um nome de domínio, configurar a segurança para que os hackers não conseguissem invadir e garantir que tudo fosse armazenado em backup no caso de alguma interrupção. Para sites ocupados, talvez você queira configurar vários computadores para distribuir a carga e serviços para garantir que um computador não fique sobrecarregado com solicitações de tratamento. Agora, graças aos serviços na nuvem, você pode simplesmente implantar seu código e deixar que o provedor de nuvem faça tudo funcionar.

Usando sua assinatura do Azure, você poderá implantar seu código na nuvem, com o Azure gerenciando todas as complexidades para você usando o [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/?WT.mc_id=hackwithazure-hackathon-cxa). Você configura um Serviço de Aplicativo e envia seu código para ele, e o Azure lida com o resto.

## <a name="deploying-to-an-app-service"></a>Implantação em um Serviço de Aplicativo

Você pode configurar um Serviço de Aplicativo do Azure e implantar seu código de dentro do Visual Studio Code.

1. Abra a paleta de comandos:
  1. No Windows, pressione Ctrl + Shift + P
  1. No MacOS, pressione Cmd + Shift + P

1. Selecione *Serviço de Aplicativo do Azure: Implantar no aplicativo Web...*
  
   ![A paleta de comandos mostrando a opção do Serviço de Aplicativo do Azure: Opção Implantar no aplicativo Web](../images/CommandPaletteDeployAppService.png)

1. Você deverá informar o código que deseja implantar. Essa opção selecionará automaticamente a pasta com seu código. Portanto, selecione-a.

   ![A paleta de comandos mostrando a opção Origem da Implantação](../images/SelectDeployFolder.png)

1. Se você nunca tiver entrado no Azure do Visual Studio Code, será solicitado a entrar.
   1. Selecione *Entrar no Azure...*
   1. Seu navegador será iniciado, e você poderá entrar com sua conta do Azure.
   1. Depois de entrar usando o navegador, você poderá fechar a página da Web que foi iniciada.

1. Selecione a assinatura do Azure que deseja usar.
  
   ![A paleta de comandos mostrando a opção Selecionar Assinatura](../images/SelectDeploySubscription.png)

1. Selecione *+ Criar Novo Aplicativo Web*

   Há duas opções para *Criar Aplicativo Web*; uma está marcada como *Avançado*. Você quer a opção normal, **não** a opção *Avançado*.

   ![A paleta de comandos mostrando a opção Criar Aplicativo Web](../images/CreateNewWebApp.png)

1. Dê um nome para o aplicativo Web. Isso fará parte do endereço do site da Web público; portanto, precisa ser exclusivo em todo o mundo. Por exemplo, eu poderia usar `jimspythonwebapp2019`.

   ![A paleta de comandos mostrando a opção Nome do Aplicativo Web](../images/SelectWebAppName.png)

1. Selecione o runtime para seu aplicativo do Serviço de Aplicativo. Este é um aplicativo Python; portanto, selecione a versão mais recente do runtime do Python, como *Python 3.7*

   ![A paleta de comandos mostrando a opção Selecionar Runtime](../images/SelectPythonRuntime.png)

1. O Serviço de Aplicativo começará a ser criado. Você verá uma barra de progresso na parte inferior direita, que mostrará quando ela for concluída. Você pode monitorar o progresso na janela *Saída* selecionando *Exibição > Saída* e selecionando *Serviço de Aplicativo do Azure* no seletor de janela.

   ![A barra de progresso Criar Serviço de Aplicativo](../images/CreateWebAppProgress.png)

1. Alguns pop-ups serão exibidos perguntando se você deseja fazer alterações de configuração para acelerar a implantação e sempre implantar este aplicativo Web. Selecione **Sim** para ambos.
  
   ![A caixa de diálogo Atualizar configuração do workspace](../images/UpdateWorkspaceConfigDialog.png)
  
   ![A caixa de diálogo Sempre implantar na configuração do aplicativo Web](../images/AlwaysDeployDialog.png)

1. Um pop-up mostrando o progresso da implantação será exibido. Você pode monitorar o progresso na janela *Saída* selecionando *Exibição > Saída* e selecionando *Serviço de Aplicativo do Azure* no seletor de janela.
  
   ![A caixa de diálogo Progresso da implantação](../images/DeployProgress.png)

1. Depois que o código tiver sido implantado, você poderá exibir o código pela Internet. Inicie o navegador e abra seu site. O endereço será `https://<web app name>.azurewebsites.net/`. Por exemplo, para meu site, isso é `https://jimspythonwebapp2019.azurewebsites.net/`. Você verá a página do jogo.

> Essas etapas criarão um Serviço de Aplicativo de camada gratuita e implantarão seu aplicativo nele. Você só pode ter uma camada gratuita por assinatura. Portanto, se você já tiver um serviço de aplicativo gratuito em execução, use a opção *Criar Aplicativo Web Avançado* e crie usando uma camada paga. Não se esqueça de excluir isso depois de concluir o workshop para parar de usar seus créditos.

## <a name="resource-groups"></a>Grupos de recursos

No Azure, os recursos como o Serviço de Aplicativo ou os serviços de IA precisam pertencer a um Grupo de Recursos. Os Grupos de Recursos são agrupamentos lógicos de recursos, agrupados de maneira que faça sentido para você. Cada recurso deve pertencer a apenas um Grupo de Recursos.

Os Grupos de Recursos são úteis, pois você pode usá-los para agrupar recursos que compõem um aplicativo. Você poderá ver todos os recursos no grupo e gerenciá-los juntos, por exemplo, se desejar excluir todos os recursos, poderá excluir o Grupo de Recursos, e todos os recursos nele serão excluídos.

Quando o seu Serviço de Aplicativo foi criado, ele teria sido criado dentro de um novo Grupo de Recursos, provavelmente com o nome `appsvc_linux_centralus`. Vamos adicionar todos os novos recursos a esse Grupo de Recursos no restante deste workshop e excluir esse Grupo de Recursos no final para remover todos os recursos.

## <a name="next-step"></a>Próxima etapa

Nesta etapa, você implantou seu aplicativo Web na nuvem, hospedado no Azure. Na [próxima etapa](./CreateAFaceResource.md), você criará um recurso da API de Detecção Facial que pode ser usado para analisar as imagens da câmera.
