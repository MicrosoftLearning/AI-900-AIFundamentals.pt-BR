---
lab:
  title: 'Explorar a fala'
---

# <a name="explore-speech"></a>Explorar a fala

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free?azure-portal=true) na qual tenha acesso administrativo.

Para criar software que possa interpretar a fala audível e responder adequadamente, você poderá usar o serviço cognitivo de **Fala**, que fornece uma maneira simples de transcrever o idioma falado em texto e vice-versa.

Por exemplo, suponha que você queira criar um dispositivo inteligente que possa responder verbalmente a perguntas faladas, como "Que horas são?". A resposta deve ser a hora local.

Para testar os recursos do serviço de Fala, usaremos um aplicativo de linha de comando simples executado no Cloud Shell. Os mesmos princípios e funcionalidades se aplicam a soluções do mundo real, como sites ou aplicativos de telefone.

## <a name="create-a-cognitive-services-resource"></a>Criar um recurso de *Serviços Cognitivos*

O serviço de Fala pode ser usado por meio da criação de um recurso de **Fala** ou de **Serviços Cognitivos**.

Caso ainda não tenha feito isso, crie um recurso dos **Serviços Cognitivos** em sua assinatura do Azure.

1. Em outra guia do navegador, abra o portal do Azure em [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e entre com sua conta Microsoft.

1. Clique no botão **&#65291;Criar um recurso**, pesquise *Serviços Cognitivos* e crie um recurso dos **Serviços Cognitivos** com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*.
    - **Grupo de recursos**: *selecione ou crie um grupo de recursos com um nome exclusivo*.
    - **Região**: *escolha uma região disponível*.
    - **Nome**: *insira um nome exclusivo*.
    - **Tipo de preço**: Standard S0
    - **Ao marcar esta caixa, confirmo que li e compreendi todos os termos abaixo**: selecionada.

1. Revise e crie o recurso.

### <a name="get-the-key-and-location-for-your-cognitive-services-resource"></a>Obter a chave e o local do recurso dos Serviços Cognitivos

1. Aguarde o fim da implantação. Em seguida, acesse o recurso dos Serviços Cognitivos e, na página de **visão geral**, clique no link para gerenciar as chaves do serviço. Você precisará do ponto de extremidade e das chaves para se conectar ao recurso dos Serviços Cognitivos de aplicativos cliente.

1. Exiba a página **Chaves e Ponto de Extremidade** do seu recurso. Você precisará do **local/região** e da **chave** para se conectar de aplicativos cliente.

## <a name="run-cloud-shell"></a>Executar o Cloud Shell

Para testar os recursos do serviço de Fala, usaremos um aplicativo de linha de comando simples executado no Cloud Shell no Azure.

1. No portal do Azure, selecione o botão **[>_]** (*Cloud Shell*) na parte superior da página à direita da caixa de pesquisa. Isso abre um painel do Cloud Shell na parte inferior do Portal.

    ![Inicie o Cloud Shell clicando no ícone à direita da caixa de pesquisa superior](media/recognize-synthesize-speech/powershell-portal-guide-1.png)

1. Na primeira vez que você abrir o Cloud Shell, talvez precise escolher o tipo de shell que deseja usar (*Bash* ou *PowerShell).* Selecione **PowerShell**. Se não vir essa opção, ignore a etapa.  

1. Se precisar criar o armazenamento para o Cloud Shell, verifique se sua assinatura está especificada e selecione **Criar armazenamento**. Aguarde um minuto para a criação do armazenamento.

    ![Crie o armazenamento com um clique em Confirmar.](media/recognize-synthesize-speech/powershell-portal-guide-2.png)

1. Verifique se o tipo de shell indicado na parte superior esquerda do painel do Cloud Shell está marcado como *PowerShell*. Se for *Bash*, alterne para o *PowerShell* usando o menu suspenso.

    ![Como localizar o menu suspenso à esquerda para alternar para o PowerShell](media/recognize-synthesize-speech/powershell-portal-guide-3.png)

1. Aguarde o início do PowerShell. Você deverá ver a seguinte tela no portal do Azure:  

    ![Aguarde o início do PowerShell.](media/recognize-synthesize-speech/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>Configurar e executar um aplicativo cliente

Agora que você tem um modelo personalizado, pode executar um aplicativo cliente simples que usa o serviço de Fala.

1. No shell do comando, insira o comando a seguir para baixar o aplicativo de exemplo e salvá-lo em uma pasta chamada ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >**Dica** Se você já usou esse comando em outro laboratório para clonar o repositório *ai-900*, ignore esta etapa.

1. Os arquivos são baixados em uma pasta chamada **ai-900**. Agora queremos ver todos os arquivos em seu armazenamento do Cloud Shell e trabalhar com eles. Digite o seguinte comando no shell:

     ```PowerShell
    code .
    ```

    Observe como isso abre um editor como o da imagem abaixo:

    ![O editor de código.](media/recognize-synthesize-speech/powershell-portal-guide-4.png)

1. No painel **Arquivos** à esquerda, expanda **ai-900** e selecione **speaking-clock.ps1**. Esse arquivo contém algum código que usa o serviço de Fala para reconhecer e sintetizar a fala:

    ![O editor que contém o código para usar o serviço Fala](media/recognize-synthesize-speech/speaking-clock-code.png)

1. Não se preocupe muito com os detalhes do código, o importante é que ele precisa da região/local e de uma das chaves do seu recurso dos Serviços Cognitivos. Copie-os da página **Chaves e Pontos de Extremidade** do seu recurso do portal do Azure e cole no editor de códigos, substituindo os valores de espaço reservado **YOUR_KEY** e **YOUR_LOCATION**, respectivamente.

    > **Dica** Talvez seja necessário usar a barra separadora para ajustar a área da tela durante o trabalho com os painéis **Chaves e ponto de extremidade** e **Editor**.

    Depois de colar a chave e os valores de região/local, as primeiras linhas de código devem ser semelhantes a esta:

    ```PowerShell
    $key = "1a2b3c4d5e6f7g8h9i0j...."
    $region="somelocation"
    ```

1. No canto superior direito do painel do editor, use o botão **...** para abrir o menu e selecione **Salvar** para salvar as alterações. Em seguida, abra o menu novamente e selecione **Fechar Editor**.

    O aplicativo cliente de amostra usará seu serviço de Fala para transcrever a entrada falada e sintetizará uma resposta falada apropriada. Um aplicativo real poderia aceitar a entrada de um microfone e enviar a resposta a um alto-falante, mas neste exemplo simples, vamos usar uma entrada pré-gravada em um arquivo e salvar a resposta como outro arquivo.

    Use o player de vídeo abaixo para ouvir o áudio de entrada que o aplicativo processará:

    <div class="embeddedvideo"><iframe src="https://www.microsoft.com/videoplayer/embed/RWMAvi" frameborder="0" allowfullscreen="true" data-linktype="external"></iframe></div>

1. No painel do PowerShell, insira o seguinte comando para executar o código:

    ```PowerShell
    cd ai-900
    ./speaking-clock.ps1
    ```

1. Examine a saída, que deve ter reconhecido com êxito o texto "Que horas são?" e salvo uma resposta adequada em um arquivo chamado *output.wav*.

    Use o seguinte player de vídeo para ouvir a saída falada gerada pelo aplicativo:

    <div class="embeddedvideo"><iframe src="https://www.microsoft.com/videoplayer/embed/RWMSIU" frameborder="0" allowfullscreen="true" data-linktype="external"></iframe></div>

## <a name="learn-more"></a>Saiba mais

Esse aplicativo simples mostra apenas alguns dos recursos do serviço de Fala. Saiba mais sobre o que você pode fazer com esse serviço consultando a [página de Fala](https://azure.microsoft.com/services/cognitive-services/speech-services/).
