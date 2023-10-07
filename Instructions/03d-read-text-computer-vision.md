---
lab:
  title: Explorar o reconhecimento óptico de caracteres
---

# Explorar o reconhecimento óptico de caracteres

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free?azure-portal=true) na qual tenha acesso administrativo.

Um desafio comum da Pesquisa Visual Computacional é detectar e interpretar texto em uma imagem. Esse tipo de processamento geralmente é conhecido como OCR (*reconhecimento óptico de caracteres*). A API de Leitura da Microsoft permite acesso a funcionalidades de OCR. 

Para testar as funcionalidades da API de Leitura, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell. Os mesmos princípios e funcionalidades se aplicam em soluções do mundo real, como sites ou aplicativos de telefone.

## Usar o serviço de Visão de IA do Azure para ler texto em uma imagem

O serviço **Visão de IA do Azure** fornece suporte para tarefas de OCR, incluindo:

- Uma API de **Leitura**, otimizada para documentos maiores. Essa API é usada de forma assíncrona e pode ser usada para textos impressos e manuscritos.

## Criar um recurso dos *serviços de IA do Azure*

Use o serviço de Visão de IA do Azure criando um recurso de **Pesquisa Visual Computacional** ou um recurso dos **serviços de IA do Azure**.

Caso ainda não tenha feito isso, crie um recurso dos **serviços de IA do Azure** em sua assinatura do Azure.

1. Em outra guia do navegador, abra o portal do Azure em [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e entre com sua conta Microsoft.

1. Clique no botão **&#65291;Criar um recurso** e pesquise por *serviços de IA do Azure*. Selecione **criar** um plano de **serviços de IA do Azure**. Você será levado para uma página para criar um recurso dos serviços de IA do Azure. Defina-o com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*.
    - **Grupo de recursos**: *selecione ou crie um grupo de recursos com um nome exclusivo*.
    - **Região**: *escolha uma região disponível*.
    - **Nome**: *insira um nome exclusivo*.
    - **Tipo de preço**: Standard S0
    - **Ao marcar esta caixa, confirmo que li e compreendi todos os termos abaixo**: selecionada.

1. Examine e crie o recurso e aguarde a conclusão da implantação. Em seguida, vá para o recurso implantado.

1. Exiba a página **Chaves e Ponto de Extremidade** do recurso dos serviços de IA do Azure. Você precisará do ponto de extremidade e das chaves para se conectar em aplicativos cliente.

## Executar o Cloud Shell

Para testar os recursos do serviço de Visão Personalizada, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell no Azure.

1. No portal do Azure, selecione o botão **[>_]** (*Cloud Shell*) na parte superior da página à direita da caixa de pesquisa. Isso abre um painel do Cloud Shell na parte inferior do Portal. 

    ![Inicie o Cloud Shell clicando no ícone à direita da caixa de pesquisa superior](media/read-text-computer-vision/powershell-portal-guide-1.png)

1. Na primeira vez que você abrir o Cloud Shell, talvez precise escolher o tipo de shell que deseja usar (*Bash* ou *PowerShell).* Selecione **PowerShell**. Se não vir essa opção, ignore a etapa.  

1. Se precisar criar o armazenamento para o Cloud Shell, verifique se sua assinatura está especificada e selecione **Criar armazenamento**. Aguarde um minuto para a criação do armazenamento.

    ![Crie o armazenamento com um clique em Confirmar.](media/read-text-computer-vision/powershell-portal-guide-2.png)

1. Verifique se o tipo de shell indicado na parte superior esquerda do painel do Cloud Shell está marcado como *PowerShell*. Se for *Bash*, alterne para o *PowerShell* usando o menu suspenso.

    ![Como localizar o menu suspenso à esquerda para alternar para o PowerShell](media/read-text-computer-vision/powershell-portal-guide-3.png) 

1. Aguarde o início do PowerShell. Você deverá ver a seguinte tela no portal do Azure:  

    ![Aguarde o início do PowerShell.](media/read-text-computer-vision/powershell-prompt.png) 

## Configurar e executar um aplicativo cliente

Agora que você tem um modelo personalizado, pode executar um aplicativo cliente simples que usa o serviço de OCR.

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

    ![O editor de código.](media/read-text-computer-vision/powershell-portal-guide-4.png)

1. No painel **Arquivos** à esquerda, expanda **ai-900** e selecione **ocr.ps1**. Esse arquivo contém um código que usa o serviço Pesquisa Visual Computacional para detectar e analisar textos em uma imagem, como mostrado aqui:

    ![O editor que contém o código para analisar texto em imagens.](media/read-text-computer-vision/ocr-code.png)

1. Não se preocupe muito com os detalhes do código, o importante é que ele precisa do URL do ponto de extremidade e de uma das chaves do recurso dos serviços de IA do Azure. Copie-os da página **Chaves e Pontos de Extremidade** do seu recurso do portal do Azure e cole no editor de códigos, substituindo os valores de espaço reservado **YOUR_KEY** e **YOUR_ENDPOINT**, respectivamente.

    > **Dica** Talvez seja necessário usar a barra separadora para ajustar a área da tela durante o trabalho com os painéis **Chaves e ponto de extremidade** e **Editor**.

    Depois de colar os valores de chave e ponto de extremidade, as duas primeiras linhas de código devem ser semelhantes a esta:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. No canto superior direito do painel do editor, use o botão **…** para abrir o menu e selecione **Salvar** para salvar as alterações. Em seguida, abra o menu novamente e selecione **Fechar Editor**. Agora que você já definiu a chave e o ponto de extremidade, use o recurso dos serviços de IA do Azure para extrair texto de uma imagem.

    Vamos usar a API de **Leitura**. Nesse caso, você tem uma imagem de anúncio da empresa fictícia de varejo Northwind Traders que inclui texto.

    O aplicativo cliente de exemplo analisará a seguinte imagem:

    ![Uma imagem de um anúncio do supermercado Northwind Traders.](media/read-text-computer-vision/advert.jpg)

1. No painel do PowerShell, insira os seguintes comandos para executar o código e ler o texto:

    ```PowerShell
    cd ai-900
    ./ocr.ps1 advert.jpg
    ```

1. Revise os detalhes encontrados na imagem. O texto encontrado na imagem é organizado em uma estrutura hierárquica de regiões, linhas e palavras, e o código os lê para recuperar os resultados.

    Observe que o local do texto é indicado pelas coordenadas superior esquerda e pela largura e altura de uma *caixa delimitadora*, conforme mostrado aqui:

    ![Uma imagem do texto na imagem descrita](media/read-text-computer-vision/lab-05-bounding-boxes.png)

1. Vamos experimentar agora com outra imagem:

    ![Uma imagem de uma carta digitada.](media/read-text-computer-vision/letter.jpg)

    Para analisar a segunda imagem, digite o seguinte comando:

    ```PowerShell
    ./ocr.ps1 letter.jpg
    ```

1. Examine os resultados da análise da segunda imagem. Também devem ser retornados o texto e as caixas delimitadoras do texto.

## Saiba mais

Esse aplicativo simples mostra apenas alguns dos recursos de OCR do serviço de Pesquisa Visual Computacional. Saiba mais sobre o que você pode fazer com esse serviço consultando a [página OCR](https://docs.microsoft.com/azure/cognitive-services/computer-vision/overview-ocr).
