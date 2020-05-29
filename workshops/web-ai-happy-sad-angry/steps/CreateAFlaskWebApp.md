# <a name="create-the-flask-web-app"></a>Criar o aplicativo Web Flask

Nesta etapa, você criará um aplicativo Web Flask.

## <a name="create-folders-for-the-project"></a>Criar pastas para o projeto

1. Crie uma pasta chamada `HappySadAngry` em algum lugar em seu computador.

## <a name="open-this-folder-in-visual-studio-code"></a>Abrir esta pasta no Visual Studio Code

1. Inicie o Visual Studio Code.

1. Abrir a pasta recém-criada
   1. No MacOS, selecione *Arquivo -> Abrir...*
   1. No Windows, selecione *Arquivo-> Abrir Pasta...*

1. Navegue até a nova pasta `HappySadAngry` e selecione **Abrir**.

Você verá que a pasta vazia aparece no *Explorer*.

## <a name="configure-a-virtual-environment"></a>Configurar um ambiente virtual

O Python é fornecido em várias versões, e aplicativos Python podem usar código externo em pacotes instalados por meio de uma ferramenta chamada `pip`. Isso poderá levar a problemas se aplicativos diferentes precisarem de versões de pacotes ou versões do Python diferentes. Para facilitar a prevenção de problemas com as versões do pacote ou do Python, a melhor prática é usar *ambientes virtuais*, árvores de pastas independentes que contêm uma instalação do Python para uma versão específica do Python, além de vários pacotes adicionais.

1. Crie um arquivo dentro da pasta `HappySadAngry` chamado `app.py`. Esse é o arquivo que conterá o código para o aplicativo Flask e, ao criá-lo, a extensão do Python no Visual Studio Code será ativada. Selecione o botão **Novo Arquivo** no *Explorer*.

   ![O botão do novo arquivo](../images/VSCodeNewFile.png)

1. Nomeie o novo arquivo `app.py` e pressione retornar

   ![Como nomear o arquivo app.py](../images/NameAppPy.png)

   > Talvez seja solicitado que você instale um Linter. Essa é uma ferramenta que pode inspecionar o código quanto a erros enquanto você o escreve. Se um pop-up for exibido perguntando se você deseja instalá-lo, selecione **Instalar**.

1. Quando o Visual Studio Code foi aberto, o terminal deve ser aberto por padrão. Caso contrário, abra um novo terminal selecionando *Terminal -> Novo Terminal*.

1. Crie um ambiente virtual chamado `.venv` usando o Python 3 e executando o comando a seguir no terminal

   ```sh
   python3 -m venv .venv
   ```

1. Uma caixa de diálogo será exibida perguntando se você deseja ativar esse ambiente virtual. Selecione **Sim**.

   ![A caixa de diálogo do ambiente virtual](../images/LaunchVirtualEnv.png)

1. O terminal existente não terá o ambiente virtual ativado. Feche-a selecionando o botão de lixeira

   ![O botão encerrar terminal](../images/KillTerminal.png)

1. Crie um terminal selecionando *Terminal -> Novo Terminal*. O terminal carregará o ambiente virtual

   ![O terminal ativando o ambiente virtual](../images/TerminalWithActivatedEnvironment.png)

## <a name="install-the-flask-package"></a>Instalar o pacote Flask

[Flask](http://flask.pocoo.org) é uma microestrutura Python para criar aplicativos Web. É leve e fácil de usar para criar aplicativos Web simples. Ele está disponível como um pacote do Python que pode ser instalado com `pip`.

Em vez de instalá-lo usando `pip` do terminal, ele deve ser configurado dentro de um arquivo `requirements.txt`. Esse arquivo lista todos os pacotes dos quais um aplicativo Python depende e será necessário quando o aplicativo Web for implantado na nuvem em uma etapa posterior. Esse arquivo informará qualquer servidor na nuvem que o site Web está em execução nos pacotes que precisam ser instalados antes que o aplicativo Web seja iniciado.

1. Criar um arquivo na pasta chamado `requirements.txt`

1. Adicione o seguinte pacote ao arquivo:
  
   ```python
   flask
   ```

1. Salvar o arquivo

   > Se você não quiser ter que se lembrar de sempre salvar arquivos, poderá ativar o salvamento automático selecionando *Arquivo -> Salvamento Automático*.

1. Instale os pacotes por meio do terminal usando o seguinte comando:
  
   ```sh
   pip install -r requirements.txt
   ```

   Isso instalará todos os pacotes no arquivo `requirements.txt`, ignorando os que já estão instalados.

## <a name="write-the-code"></a>Escrever o código

1. Abrir o arquivo `app.py`

1. Adicione o seguinte código a este arquivo:
  
    ```python
    from flask import Flask

    app = Flask(__name__)

    @app.route('/')
    def home():
      return 'Hello World'
    ```

1. Salvar o arquivo

## <a name="run-the-code"></a>Executar o código

Esse código não pode ser executado do terminal usando o comando `Python`. Em vez disso, ele deve ser executado como um aplicativo Flask usando o pacote Flask. Existem duas maneiras de fazer isso:

1. No painel Depurar da barra de ferramentas, solte a caixa *Depurar configuração* e selecione *Python: Flask*.
  
   > Se você não vir essa opção, selecione *Adicionar configuração* para editar o arquivo `launch.json`. Isso deve criar um conjunto de opções de inicialização para arquivos Python.
   >
   > Se essas opções não forem criadas automaticamente, selecione *Adicionar Configuração...* e selecione a opção *Flask*. Salve esse arquivo.
   >
   > Selecione *Python: Flask* na caixa *Depurar configuração*.

   Selecione o botão verde *Iniciar Depuração*.

   Se usar esse método, você será capaz de definir pontos de interrupção e depurar seu código.

1. No terminal, execute o arquivo como um aplicativo do Flask usando:
  
   ```sh
   flask run
   ```

   Se usar esse método, você não será capaz de definir pontos de interrupção e depurar seu código.

O aplicativo Web será executado e poderá ser acessado em seu dispositivo em [http://127.0.0.1:5000](http://127.0.0.1:5000). Você verá essa URL na janela de saída e poderá usar **Ctrl + clique** para ir diretamente ao site em questão.

1. Abra essa URL em um navegador da Web para ver a mensagem `Hello World`.

   ![Um site mostrando Olá, Mundo](../images/HelloWorldOnWebSite.png)

1. Pare o depurador depois de testar isso.

## <a name="what-does-this-code-do"></a>O que este código faz?

```python
from flask import Flask
```

Isso informa ao compilador do Python que desejamos usar o código no módulo `Flask`. Esse módulo foi instalado como parte do pacote do `flask`.

```python
app = Flask(__name__)
```

Isso cria um aplicativo Web Flask chamado como qualquer que seja o nome do arquivo. `__name__` é uma variável especial no Python que retorna o nome do módulo atual. Portanto, o arquivo sem a extensão `.py`.

```python
@app.route('/')
def home():
```

Isso define uma função chamada `home`. Essa função é mapeada para uma rota chamada `/`. Em um aplicativo Web, uma rota é a parte da URL após o nome de domínio, e rotas diferentes podem ser mapeadas para páginas da Web diferentes. `/` geralmente é a home page, e pode haver tantas outras rotas quantas forem necessárias, por exemplo, `/about` poderia rotear para uma página Sobre, `/basket` poderia rotear para uma cesta de compras. Se o seu site estava em `http://www.mywebsite.com`, a rota `/` é aquela que seria usada quando você aponta seu navegador para `http://www.mywebsite.com`, `/about` seria usado quando você passou para `http://www.mywebsite.com/about` e assim por diante.

```python
return 'Hello World'
```

O conteúdo da função `home` apenas retorna texto simples, e isso será renderizado por um navegador da Web como texto bruto.

## <a name="next-step"></a>Próxima etapa

Nesta etapa, você criou um aplicativo Web Flask simples que mostrou 'Olá, Mundo' ao ser executado. Na [próxima etapa](./CreateTheWebPage.md), você criará a página da Web para o jogo que mostra a emoção que você está tentando transmitir e captura imagens da câmera.
