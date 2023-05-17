---
lab:
  title: 'Explorar a tradução'
---

# <a name="explore-translation"></a>Explorar a tradução

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free?azure-portal=true) na qual tenha acesso administrativo.

Uma das forças motrizes que permitiu o desenvolvimento da civilização humana é a capacidade de comunicação mútua. Na maioria das empreitadas humanas, a comunicação é fundamental.

A IA (Inteligência Artificial) pode ajudar a simplificar a comunicação traduzindo texto ou fala entre idiomas, ajudando a remover barreiras de comunicação entre países e culturas.

Para testar os recursos do serviço de Tradução, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell. Os mesmos princípios e funcionalidades se aplicam em soluções do mundo real, como sites ou aplicativos de telefone.

## <a name="create-a-cognitive-services-resource"></a>Criar um recurso dos *Serviços Cognitivos*

O serviço de Tradução pode ser usado por meio da criação de um recurso **Tradutor** ou de um recurso dos **Serviços Cognitivos**.

Caso ainda não tenha feito isso, crie um recurso dos **Serviços Cognitivos** em sua assinatura do Azure.

1. Em outra guia do navegador, abra o portal do Azure em [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e entre com sua conta Microsoft.

1. Selecione o botão **&#65291;Criar um recurso**, procure *Serviços Cognitivos* e crie um recurso dos **Serviços Cognitivos** com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*.
    - **Grupo de recursos**: *selecione ou crie um grupo de recursos com um nome exclusivo*.
    - **Região**: *escolha uma região disponível*.
    - **Nome**: *insira um nome exclusivo*.
    - **Tipo de preço**: Standard S0
    - **Ao marcar esta caixa, confirmo que li e compreendi todos os termos abaixo**: selecionada.

1. Examine e crie o recurso e aguarde a conclusão da implantação. Em seguida, vá para o recurso implantado.

1. Exiba a página **Chaves e Ponto de Extremidade** do recurso dos Serviços Cognitivos. Você precisará das chaves e do local para se conectar de aplicativos cliente.

### <a name="get-the-key-and-location-for-your-cognitive-services-resource"></a>Obter a chave e o local do recurso dos Serviços Cognitivos

1. Aguarde o fim da implantação. Em seguida, acesse o recurso dos Serviços Cognitivos e, na página de **Visão geral**, selecione o link para gerenciar as chaves do serviço. Você precisará das chaves e do local para se conectar ao recurso dos Serviços Cognitivos de aplicativos cliente.

1. Exiba a página **Chaves e Ponto de Extremidade** do seu recurso. Você precisará do **local/região** e da **chave** para se conectar de aplicativos cliente.

> **Observação** Para usar o serviço Tradução, não é preciso usar o ponto de extremidade do Serviço Cognitivo. Um ponto de extremidade global apenas para o serviço Tradutor é fornecido. 

## <a name="run-cloud-shell"></a>Executar o Cloud Shell

Para testar os recursos do serviço de Tradução, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell no Azure. 

1. No portal do Azure, selecione o botão **[>_]** (*Cloud Shell*) na parte superior da página à direita da caixa de pesquisa. Isso abre um painel do Cloud Shell na parte inferior do Portal.

    ![Inicie o Cloud Shell clicando no ícone à direita da caixa de pesquisa superior](media/translate-text-and-speech/powershell-portal-guide-1.png)

1. Na primeira vez que você abrir o Cloud Shell, talvez precise escolher o tipo de shell que deseja usar (*Bash* ou *PowerShell).* Selecione **PowerShell**. Se não vir essa opção, ignore a etapa.  

1. Se precisar criar o armazenamento para o Cloud Shell, verifique se sua assinatura está especificada e selecione **Criar armazenamento**. Aguarde um minuto para a criação do armazenamento.

    ![Crie o armazenamento com um clique em Confirmar.](media/translate-text-and-speech/powershell-portal-guide-2.png)

1. Verifique se o tipo de shell indicado na parte superior esquerda do painel do Cloud Shell indica *PowerShell*. Se for *Bash*, alterne para o *PowerShell* usando o menu suspenso. 

    ![Como localizar o menu suspenso à esquerda para alternar para o PowerShell](media/translate-text-and-speech/powershell-portal-guide-3.png) 

1. Aguarde o início do PowerShell. Você deverá ver a seguinte tela no portal do Azure:  

    ![Aguarde o início do PowerShell.](media/translate-text-and-speech/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>Configurar e executar um aplicativo cliente

Agora que você tem um modelo personalizado, pode executar um aplicativo cliente simples que usa o serviço de Tradução.

1. No shell de comando, digite o comando a seguir para baixar o aplicativo de exemplo e salvá-lo em uma pasta chamada ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >**Dica** Se você já usou esse comando em outro laboratório para clonar o repositório *ai-900*, ignore esta etapa.

1. Os arquivos são baixados em uma pasta chamada **ai-900**. Agora queremos ver todos os arquivos em seu armazenamento do Cloud Shell e trabalhar com eles. Digite o seguinte comando no shell: 

     ```PowerShell
    code .
    ```

    Observe como isso abre um editor como o da imagem abaixo: 

    ![O editor de código.](media/translate-text-and-speech/powershell-portal-guide-4.png)

1. No painel **Arquivos** à esquerda, expanda **ai-900** e selecione **translator.ps1**. Esse arquivo contém um código que usa o serviço Tradutor:

    ![O editor que contém o código para usar o serviço Tradutor](media/translate-text-and-speech/translate-code.png)

1. Não se preocupe muito com os detalhes do código, o importante é que ele precisa da região/local e de uma das chaves do seu recurso dos Serviços Cognitivos. Copie esses dados da página **Chaves e pontos de extremidade** do recurso do portal do Azure e cole-os no editor de códigos, substituindo os valores de espaço reservado **YOUR_KEY** e **YOUR_LOCATION**, respectivamente.

    Depois de colar os valores de chave e local, as primeiras linhas de código devem ser semelhantes a esta:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $location="somelocation"
    ```

1. No canto superior direito do painel do editor, use o botão **…** para abrir o menu e selecione **Salvar** para salvar as alterações. Em seguida, abra o menu novamente e selecione **Fechar Editor**.

    O aplicativo cliente de exemplo usará o serviço Tradutor para realizar várias tarefas:
    - Traduzir textos do inglês para o francês, italiano e chinês.
    - Traduzir áudios em inglês para texto em francês

    Use o player de vídeo abaixo para ouvir o áudio de entrada que o aplicativo processará:

    <div class="embeddedvideo"><iframe src="https://www.microsoft.com/videoplayer/embed/RWORN0" frameborder="0" allowfullscreen="true" data-linktype="external"></iframe></div>


    > **Observação** Um aplicativo real poderia aceitar a entrada de um microfone e enviar a resposta a um alto-falante, mas neste exemplo simples, vamos usar uma entrada pré-gravada em um arquivo de áudio.

1. No painel do Cloud Shell, insira o seguinte comando para executar o código:

    ```PowerShell
    cd ai-900
    ./translator.ps1
    ```

1. Examine a saída. Você observou a tradução de texto de inglês para francês, italiano e chinês?  Você observou o áudio em inglês "hello" traduzido para o francês em texto?

## <a name="learn-more"></a>Saiba mais

Esse aplicativo simples mostra apenas alguns dos recursos do serviço Tradutor. Saiba mais sobre o que você pode fazer com esse serviço consultando a [página Tradução de Texto](https://docs.microsoft.com/azure/cognitive-services/translator/translator-overview).