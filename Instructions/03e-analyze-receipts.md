---
lab:
  title: Explorar o reconhecimento de formulários
---

# Explorar o reconhecimento de formulários

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free?azure-portal=true) na qual tenha acesso administrativo.

No campo IA (inteligência artificial) da pesquisa visual computacional, o OCR (reconhecimento óptico de caracteres) é comumente usado para ler documentos impressos ou manuscritos. Geralmente, o texto é simplesmente extraído dos documentos em um formato que possa ser usado em processamento ou análise posterior.

Um cenário de OCR mais avançado é a extração de informações de formulários, como pedidos de compra ou faturas, com uma compreensão semântica do que os campos no formulário representam. O serviço **Reconhecimento de Formulários** foi projetado especificamente para esse tipo de problema de IA.

O Reconhecimento de Formulários usa modelos de machine learning treinados para extrair texto de imagens de faturas, recibos e muito mais. Embora outros modelos de pesquisa visual computacional possam capturar texto, o Reconhecimento de Formulários também captura a estrutura do texto, como pares chave/valor e informações em tabelas. Assim, em vez de ter que digitar manualmente entradas de um formulário em um banco de dados, você pode capturar automaticamente as relações entre o texto do arquivo original. 

Para testar os recursos do serviço de Reconhecimento de Formulários, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell. Os mesmos princípios e funcionalidades se aplicam em soluções do mundo real, como sites ou aplicativos de telefone.

## Criar um recurso dos *serviços de IA do Azure*

Você pode usar o serviço de Reconhecimento de Formulários com a criação de um recurso do **Reconhecimento de Formulários** ou de um recurso dos **serviços de IA do Azure**.

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

Para testar os recursos do serviço de Reconhecimento de Formulários, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell no Azure. 

1. No portal do Azure, selecione o botão **[>_]** (*Cloud Shell*) na parte superior da página à direita da caixa de pesquisa. Isso abre um painel do Cloud Shell na parte inferior do Portal. 

    ![Inicie o Cloud Shell clicando no ícone à direita da caixa de pesquisa superior](media/analyze-receipts/powershell-portal-guide-1.png)

1. Na primeira vez que você abrir o Cloud Shell, talvez precise escolher o tipo de shell que deseja usar (*Bash* ou *PowerShell).* Selecione **PowerShell**. Se não vir essa opção, ignore a etapa.  

1. Se precisar criar o armazenamento para o Cloud Shell, verifique se sua assinatura está especificada e selecione **Criar armazenamento**. Aguarde um minuto para a criação do armazenamento.

    ![Crie o armazenamento com um clique em Confirmar.](media/analyze-receipts/powershell-portal-guide-2.png)

1. Verifique se o tipo de shell indicado na parte superior esquerda do painel do Cloud Shell está marcado como *PowerShell*. Se for *Bash*, alterne para o *PowerShell* usando o menu suspenso.

    ![Como localizar o menu suspenso à esquerda para alternar para o PowerShell](media/analyze-receipts/powershell-portal-guide-3.png) 

1. Aguarde o início do PowerShell. Você deverá ver a seguinte tela no portal do Azure:  

    ![Aguarde o início do PowerShell.](media/analyze-receipts/powershell-prompt.png) 

## Configurar e executar um aplicativo cliente

Agora que você tem um modelo personalizado, pode executar um aplicativo cliente simples que usa o serviço de Reconhecimento de Formulários.

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

    ![O editor de código.](media/analyze-receipts/powershell-portal-guide-4.png)

1. No painel **Arquivos** à esquerda, expanda **ia-900** e selecione **form-recognizer.ps1**. Esse arquivo contém código que usa o serviço de Reconhecimento de Formulários para analisar os campos em um recibo, como mostrado aqui:

    ![O editor que contém o código para analisar campos em um recibo.](media/analyze-receipts/recognize-receipt-code.png)

1. Não se preocupe muito com os detalhes do código, o importante é que ele precisa da URL do ponto de extremidade e de uma das chaves do recurso dos serviços de IA do Azure. Copie-os da página **Chaves e Pontos de Extremidade** do seu recurso do portal do Azure e cole no editor de códigos, substituindo os valores de espaço reservado **YOUR_KEY** e **YOUR_ENDPOINT**, respectivamente.

    > **Dica** Talvez seja necessário usar a barra separadora para ajustar a área da tela durante o trabalho com os painéis **Chaves e ponto de extremidade** e **Editor**.

    Depois de colar os valores de chave e ponto de extremidade, as duas primeiras linhas de código devem ser semelhantes a esta:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. No canto superior direito do painel do editor, use o botão **…** para abrir o menu e selecione **Salvar** para salvar as alterações. Em seguida, abra o menu novamente e selecione **Fechar Editor**. Agora que configurou a chave e o ponto de extremidade, você pode usar seu recurso para analisar campos de um recibo. Nesse caso, você usará o modelo interno do Reconhecimento de Formulários para analisar um recibo da loja fictícia Northwind Traders.

    O aplicativo cliente de exemplo analisará a seguinte imagem:

    ![Esta é uma imagem de um recibo.](media/analyze-receipts/receipt.jpg)

1. No painel do PowerShell, insira os seguintes comandos para executar o código e ler o texto:

    ```PowerShell
    cd ai-900
    ./form-recognizer.ps1
    ```

1. Examine os resultados retornados. Veja que o Reconhecimento de Formulários é capaz de interpretar os dados no formulário, identificando corretamente o endereço e o número de telefone da loja e a data e hora da transação, bem como os itens de linha, subtotal, impostos e valores totais.

## Saiba mais

Esse aplicativo simples mostra apenas alguns dos recursos do Reconhecimento de Formulários do serviço de Pesquisa Visual Computacional. Saiba mais sobre o que você pode fazer com esse serviço consultando a [página do Reconhecimento de Formulários](https://docs.microsoft.com/azure/applied-ai-services/form-recognizer/overview).
