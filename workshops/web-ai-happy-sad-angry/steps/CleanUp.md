# <a name="clean-up"></a>Limpar

Nesta [etapa anterior](./CheckTheEmotion.md), você criou o jogo para capturar quadros da câmera e procurar emoções. Nesta etapa, você limpará os recursos do Azure.

## <a name="deleting-the-resource-group"></a>Excluindo o grupo de recursos

Tudo o que você criou para este workshop deve estar no mesmo grupo de recursos. Esse grupo de recursos foi criado quando você implantou o Serviço de Aplicativo do Azure pela primeira vez e foi usado quando você criou a API de Detecção Facial.

Excluir esse grupo de recursos excluirá todos os recursos criados.

1. Abra o portal do Azure em [portal.azure.com](https://portal.azure.com/?WT.mc_id=hackwithazure-hackathon-cxa). Faça logon se necessário.

1. Na barra de pesquisa na parte superior, pesquise pelo grupo de recursos que foi criado. Selecione-o nos resultados.
  
   ![Pesquisando pelo grupo de recursos no Azure](../images/SearchForResourceGroup.png)

1. Selecione **Excluir grupo de recursos**.
  
   ![Botão excluir grupo de recursos](../images/DeleteResourceGroupButton.png)

1. O painel de confirmação será exibido, mostrando todos os recursos que serão excluídos com o grupo de recursos.

   ![A confirmação de exclusão do grupo de recursos](../images/DeleteResourceGroupConfirm.png)

1. Insira o nome do grupo de recursos e selecione *Excluir*.

Todos os recursos no grupo serão excluídos.