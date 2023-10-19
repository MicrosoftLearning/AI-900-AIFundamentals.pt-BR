---
lab:
  title: Explorar a Pesquisa Visual Computacional
---

# Explorar a Pesquisa Visual Computacional

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free?azure-portal=true) na qual tenha acesso administrativo.

O serviço *Pesquisa Visual Computacional* utiliza modelos de machine learning pré-treinados para analisar imagens e extrair informações sobre elas.

Por exemplo, suponha que o varejista fictício *Northwind Traders* tenha decidido implementar uma “loja inteligente”, monitorada pelos serviços de IA para identificar os clientes que precisam de assistência e direcionar os funcionários para ajudá-los. Usando o serviço de Pesquisa Visual Computacional, as imagens feitas por câmeras instaladas em toda a loja podem ser analisadas para fornecer descrições significativas daquilo que representam.

Neste laboratório, você usará um aplicativo de linha de comando simples para ver o serviço Pesquisa Visual Computacional em ação. Os mesmos princípios e funcionalidades se aplicam em soluções do mundo real, como sites ou aplicativos de telefone.

## Criar um recurso dos *serviços de IA do Azure*

Você pode usar o serviço de Pesquisa Visual Computacional criando um recurso de **Pesquisa Visual Computacional** ou um recurso dos **Serviços de IA do Azure**.

Caso ainda não tenha feito isso, crie um recurso dos **serviços de IA do Azure** em sua assinatura do Azure.

1. Em outra guia do navegador, abra o portal do Azure em [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e entre com sua conta Microsoft.

1. Clique no botão **&#65291;Criar um recurso** e pesquise por *serviços de IA do Azure*. Selecione **criar** um plano dos **serviços de IA do Azure**. Você será levado para uma página para criar um recurso dos serviços de IA do Azure. Defina-o com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*.
    - **Grupo de recursos**: *selecione ou crie um grupo de recursos com um nome exclusivo*.
    - **Região**: *escolha uma região disponível*.
    - **Nome**: *insira um nome exclusivo*.
    - **Tipo de preço**: Standard S0
    - **Ao marcar esta caixa, confirmo que li e compreendi todos os termos abaixo**: selecionada.

1. Examine e crie o recurso e aguarde a conclusão da implantação. Em seguida, vá para o recurso implantado.

1. Exiba a página **Chaves e Ponto de Extremidade** do recurso dos serviços de IA do Azure. Você precisará do ponto de extremidade e das chaves para se conectar em aplicativos cliente.

## Executar o Cloud Shell

Para testar os recursos do serviço de Pesquisa Visual Computacional, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell no Azure.

1. No portal do Azure, selecione o botão **[>_]** (*Cloud Shell*) na parte superior da página à direita da caixa de pesquisa. Isso abre um painel do Cloud Shell na parte inferior do Portal.

    ![Inicie o Cloud Shell clicando no ícone à direita da caixa de pesquisa superior](media/analyze-images-computer-vision-service/powershell-portal-guide-1.png)

1. Na primeira vez que você abrir o Cloud Shell, talvez precise escolher o tipo de shell que deseja usar (*Bash* ou *PowerShell).* Selecione **PowerShell**. Se não vir essa opção, ignore a etapa.  

1. Se precisar criar o armazenamento para o Cloud Shell, verifique se sua assinatura está especificada e selecione **Criar armazenamento**. Aguarde um minuto para a criação do armazenamento.

    ![Crie o armazenamento com um clique em Confirmar.](media/analyze-images-computer-vision-service/powershell-portal-guide-2.png)

1. Verifique se o tipo de shell indicado na parte superior esquerda do painel do Cloud Shell está marcado como *PowerShell*. Se for *Bash*, alterne para o *PowerShell* usando o menu suspenso.

    ![Como localizar o menu suspenso à esquerda para alternar para o PowerShell](media/analyze-images-computer-vision-service/powershell-portal-guide-3.png)

1. Aguarde o início do PowerShell. Você deverá ver a seguinte tela no portal do Azure:  

    ![Aguarde o início do PowerShell.](media/analyze-images-computer-vision-service/powershell-prompt.png)

## Configurar e executar um aplicativo cliente

Agora que você tem um ambiente de Cloud Shell, pode executar um aplicativo simples que usa o serviço de Pesquisa Visual Computacional para analisar uma imagem.

1. No shell de comando, digite o comando a seguir para baixar o aplicativo de exemplo e salvá-lo em uma pasta chamada ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    > **Dica** Se você já usou esse comando em outro laboratório para clonar o repositório *ai-900*, ignore esta etapa.

1. Os arquivos são baixados em uma pasta chamada **ai-900**. Agora queremos ver todos os arquivos em seu armazenamento do Cloud Shell e trabalhar com eles. Digite o seguinte comando no shell:

    ```PowerShell
    code .
    ```

    Observe como isso abre um editor como o da imagem abaixo:

    ![O editor de código.](media/analyze-images-computer-vision-service/powershell-portal-guide-4.png)

1. No painel **Arquivos** à esquerda, expanda **ai-900** e selecione **analyze-image.ps1**. Esse arquivo contém um código que usa o serviço Pesquisa Visual Computacional para analisar uma imagem, como mostrado aqui:

    ![O editor que contém o código para analisar uma imagem](media/analyze-images-computer-vision-service/analyze-image-code.png)

1. Não se preocupe muito com o código, o importante é que ele precisa da URL do ponto de extremidade e de uma das chaves do seu recurso de serviços de IA do Azure. Copie-os da página **Chaves e Pontos de Extremidade** do seu recurso do portal do Azure e cole no editor de códigos, substituindo os valores de espaço reservado **YOUR_KEY** e **YOUR_ENDPOINT**, respectivamente.

    > **Dica** Talvez seja necessário usar a barra separadora para ajustar a área da tela durante o trabalho com os painéis **Chaves e ponto de extremidade** e **Editor**.

    Depois de colar os valores de chave e ponto de extremidade, as duas primeiras linhas de código devem ser semelhantes a esta:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. No canto superior direito do painel do editor, use o botão **…** para abrir o menu e selecione **Salvar** para salvar as alterações.

    O aplicativo cliente de exemplo usará seu serviço de Pesquisa Visual Computacional para analisar a imagem abaixo, feita por uma câmera na loja Northwind Traders:

    ![Imagem de um pai usando uma câmera de celular para tirar uma foto do filho em uma loja](media/analyze-images-computer-vision-service/store-camera-1.jpg)

1. No painel do PowerShell, insira os seguintes comandos para executar o código:

    ```PowerShell
    cd ai-900
    ./analyze-image.ps1 store-camera-1.jpg
    ```

1. Examine os resultados da análise de imagem, que incluem:
    - Uma legenda sugerida que descreve a imagem.
    - Uma lista de objetos identificados na imagem.
    - Uma lista de “tags” relevantes para a imagem.

1. Vamos experimentar agora com outra imagem:

    ![Imagem de uma pessoa com uma cesta de compras em um supermercado](media/analyze-images-computer-vision-service/store-camera-2.jpg)

    Para analisar a segunda imagem, digite o seguinte comando:

    ```PowerShell
    ./analyze-image.ps1 store-camera-2.jpg
    ```

1. Examine os resultados da análise da segunda imagem.

1. Vamos tentar mais uma:

    ![Imagem de uma pessoa com um carrinho de compras](media/analyze-images-computer-vision-service/store-camera-3.jpg)

    Para analisar a terceira imagem, digite o seguinte comando:

    ```PowerShell
    ./analyze-image.ps1 store-camera-3.jpg
    ```

1. Examine os resultados da análise da terceira imagem.

## Saiba mais

Esse aplicativo simples mostra apenas alguns dos recursos do serviço de Pesquisa Visual Computacional. Saiba mais sobre o que você pode fazer com esse serviço consultando a [página Pesquisa Visual Computacional](https://azure.microsoft.com/products/ai-services?activetab=pivot:visiontab).

