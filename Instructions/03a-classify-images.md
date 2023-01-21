---
lab:
  title: Explorar a classificação de imagem
---

# <a name="explore-image-classification"></a>Explorar a classificação de imagem

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free?azure-portal=true) na qual tenha acesso administrativo.

O serviço cognitivo de *Pesquisa Visual Computacional* fornece modelos predefinidos úteis para trabalhar com imagens, mas você frequentemente precisará treinar seu próprio modelo para pesquisa visual computacional. Por exemplo, suponha que a empresa de varejo da Northwind Traders queira criar um sistema de check-out automatizado que identifica os itens de mercearia que os clientes desejam comprar com base em uma imagem tirada por uma câmera no final do check-out. Para fazer isso, você precisará treinar um modelo de classificação que possa classificar as imagens para identificar o item que está sendo comprado.

No Azure, você pode usar o serviço cognitivo de ***Visão Personalizada*** para treinar um modelo de classificação de imagem com base em imagens existentes. A criação de uma solução de classificação de imagem envolve dois elementos. Primeiro, você deve treinar um modelo para reconhecer classes diferentes usando imagens existentes. Depois, após o treinamento do modelo, você deverá publicá-lo como um serviço que pode ser consumido por aplicativos.

Para testar os recursos do serviço de Visão Personalizada, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell. Os mesmos princípios e funcionalidades se aplicam em soluções do mundo real, como sites ou aplicativos de telefone.

## <a name="create-a-cognitive-services-resource"></a>Criar um recurso dos *Serviços Cognitivos*

Você pode usar o serviço Visão Personalizada criando um recurso de **Visão Personalizada** ou um recurso dos **Serviços Cognitivos**.

>**Observação** Nem todos recursos estão disponíveis em todas as regiões. Se você criar um recurso de Visão Personalizada ou de Serviços Cognitivos, somente recursos criados em [determinadas regiões](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) podem ser usados para acessar serviços de Visão Personalizada. Para simplificar, uma região é pré-selecionada para você nas instruções de configuração abaixo.

Crie um recurso dos **Serviços Cognitivos** em sua assinatura do Azure.

1. Em outra guia do navegador, abra o portal do Azure em [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e entre com sua conta Microsoft.

1. Clique no botão **&#65291;Criar um recurso**, pesquise *Serviços Cognitivos* e crie um recurso dos **Serviços Cognitivos** com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*.
    - **Grupo de recursos**: *selecione ou crie um grupo de recursos com um nome exclusivo*.
    - **Região:** Leste dos EUA
    - **Nome**: *insira um nome exclusivo*.
    - **Tipo de preço**: Standard S0
    - **Ao marcar esta caixa, confirmo que li e compreendi todos os termos abaixo**: selecionada.

1. Examine e crie o recurso e aguarde a conclusão da implantação. Em seguida, vá para o recurso implantado.

1. Exiba a página **Chaves e Ponto de Extremidade** do recurso dos Serviços Cognitivos. Você precisará do ponto de extremidade e das chaves para se conectar em aplicativos cliente.

## <a name="create-a-custom-vision-project"></a>Criar um projeto de Visão Personalizada

Para treinar um modelo de detecção de objetos, você precisa criar um projeto de Visão Personalizada com base em seu recurso de treinamento. Para fazer isso, você usará o portal de Visão Personalizada.

1. Baixe e extraia as imagens de treinamento do https://aka.ms/fruit-images. Essas imagens são fornecidas em uma pasta compactada que, quando extraída, contém subpastas chamadas **maçã**, **banana** e **laranja**.

1. Em outra guia do navegador, abra o portal de Visão Personalizada em [https://customvision.ai](https://customvision.ai?azure-portal=true). Caso solicitado, entre usando a conta Microsoft associada à sua assinatura do Azure e concorde com os termos de serviço.

1. No portal de Visão Personalizada, crie um projeto com as seguintes configurações:

    - **Nome**: Check-out na Mercearia
    - **Descrição**: classificação de imagem para mercearia
    - **Recurso**: *o recurso de Visão Personalizada criado anteriormente*
    - **Tipos de Projeto**: Classificação
    - **Tipos de classificação**: multiclasse (tag única por imagem)
    - **Domínios**: comida

1. Clique em **Adicionar imagens** e selecione todos os arquivos da pasta **maçã** que você já havia extraído. Em seguida, carregue os arquivos de imagem, especificando a tag *maçã*, desta forma:

    ![Carregar maçã com tag maçã](media/create-image-classification-system/upload-apples.jpg)

1. Repita a etapa anterior para carregar as imagens na pasta **banana** com a tag *banana* e as imagens na pasta **laranja** com a tag *laranja*.

1. Explore as imagens que você carregou no projeto de Visão Personalizada – deve haver 15 imagens de cada classe, desta forma:

    ![Imagens de frutas com tags – 15 maçãs, 15 bananas e 15 laranjas](media/create-image-classification-system/fruit.jpg)

1. No projeto de Visão Personalizada, acima das imagens, clique em **Treinar** para treinar um modelo de classificação usando as imagens com tag. Selecione a opção de **Treinamento Rápido** e aguarde a conclusão da iteração de treinamento (isso pode levar mais ou menos um minuto).

1. Quando a iteração do modelo tiver sido treinada, revise as métricas de desempenho de *Precisão*, *Recall* e *PA* – elas medem a precisão de predição do modelo de classificação e devem ser todas altas.

## <a name="test-the-model"></a>Testar o modelo

Antes de publicar essa iteração do modelo para uso dos aplicativos, você deve testá-la.

1. Acima das métricas de desempenho, clique em **Início Rápido**.

1. Na caixa **URL da imagem**, digite `https://aka.ms/apple-image` e clique em &#10132;

1. Exiba as previsões retornadas por seu modelo – a pontuação de probabilidade para *maçã* deve ser a mais alta, assim:

    ![Uma imagem com uma previsão de classe de maçã](media/create-image-classification-system/test-apple.jpg)

1. Feche a janela **Teste Rápido**.

## <a name="publish-the-image-classification-model"></a>Publicar o modelo de classificação de imagem

Agora está tudo pronto para publicar seu modelo treinado e usá-lo em um aplicativo cliente.

1. Clique em **&#128504; Publicar** para publicar o modelo treinado com as seguintes configurações:
    - **Nome do modelo**: itens de mercearia
    - **Recurso de Previsão**: *o recurso de previsão criado anteriormente*.

1. Após a publicação, clique no ícone *URL de Previsão* (&#127760;) para ver as informações necessárias para usar o modelo publicado. Posteriormente, você precisará da URL e dos valores de Prediction-Key apropriados para obter uma previsão de uma URL de imagem. Portanto, mantenha essa caixa de diálogo aberta e vá para a próxima tarefa. 

## <a name="run-cloud-shell"></a>Executar o Cloud Shell

Para testar os recursos do serviço de Visão Personalizada, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell no Azure.

1. No portal do Azure, selecione o botão **[>_]** (*Cloud Shell*) na parte superior da página à direita da caixa de pesquisa. Isso abre um painel do Cloud Shell na parte inferior do Portal. 

    ![Inicie o Cloud Shell clicando no ícone à direita da caixa de pesquisa superior](media/create-image-classification-system/powershell-portal-guide-1.png)

1. Na primeira vez que você abrir o Cloud Shell, talvez precise escolher o tipo de shell que deseja usar (*Bash* ou *PowerShell).* Selecione **PowerShell**. Se não vir essa opção, ignore a etapa.  

1. Se precisar criar o armazenamento para o Cloud Shell, verifique se sua assinatura está especificada e selecione **Criar armazenamento**. Aguarde um minuto para a criação do armazenamento.

    [![Crie o armazenamento clicando em confirmar.](media/create-image-classification-system/powershell-portal-guide-2.png)](media/create-image-classification-system/powershell-portal-guide-2.png#lightbox)

1. Verifique se o tipo de shell indicado na parte superior esquerda do painel do Cloud Shell está marcado como *PowerShell*. Se for *Bash*, alterne para o *PowerShell* usando o menu suspenso.

    ![Como localizar o menu suspenso à esquerda para alternar para o PowerShell](media/create-image-classification-system/powershell-portal-guide-3.png)

1. Aguarde o início do PowerShell. Você deverá ver a seguinte tela no portal do Azure:  

    ![Aguarde o início do PowerShell.](media/create-image-classification-system/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>Configurar e executar um aplicativo cliente

Agora que você tem um ambiente de Cloud Shell, pode executar um aplicativo simples que usa o serviço de Visão Personalizada para analisar uma imagem.

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

    ![O editor de código.](media/create-image-classification-system/powershell-portal-guide-4.png)

1. No painel **Arquivos** à esquerda, expanda **ai-900** e selecione **classify-image.ps1**. Esse arquivo contém um código que usa o modelo de Visão Personalizada para analisar uma imagem, conforme mostrado aqui:

     ![O editor que contém o código para classificar uma imagem](media/create-image-classification-system/classify-image-code.png)

1. Não se preocupe tanto com os detalhes do código, o importante é que ele precisa da URL de previsão e da chave de seu modelo de Visão Personalizada ao usar uma URL de imagem. 

   Obtenha a *URL de previsão* na caixa de diálogo no projeto de Visão Personalizada. 

   >**Observação** Lembre-se de que você examinou a *URL de previsão* depois de publicar o modelo de classificação de imagem. Para encontrar a *URL de previsão*, navegue até a guia **Desempenho** no projeto e clique em **URL de Previsão** (se a tela estiver compactada, você poderá ver um ícone de globo). Uma caixa de diálogo será exibida. Copie a URL de **Se você tiver uma URL de imagem**. Cole-a no editor de códigos, substituindo **URL_DE_PREVISÃO**.

    Usando a mesma caixa de diálogo, obtenha a *chave de previsão*. Copie a chave de previsão exibida depois de *Definir o Cabeçalho Prediction-Key como*. Cole-a no editor de códigos, substituindo o valor de espaço reservado **YOUR_PREDICTION_KEY**.

    ![Captura de tela da URL de previsão.](media/create-image-classification-system/find-prediction-url.png)

    Depois de colar os valores de URL de Previsão e Chave de Previsão, as duas primeiras linhas de código devem ser semelhantes a esta:

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

1. No canto superior direito do painel do editor, use o botão **...** para abrir o menu e selecione **Salvar** para salvar as alterações. Em seguida, abra o menu novamente e selecione **Fechar Editor**.

    Você usará o aplicativo cliente de amostra para classificar várias imagens na categoria maçã, banana ou laranja.

1. Classificaremos esta imagem:

    ![Imagem de uma maçã](media/create-image-classification-system/fruit-1.jpg)

    No painel do PowerShell, insira os seguintes comandos para executar o código:

    ```PowerShell
    cd ai-900
    ./classify-image.ps1 1
    ```

1. Revise a previsão, que deve ser **maçã**.

1. Vamos experimentar agora com outra imagem:

    ![Imagem de uma banana](media/create-image-classification-system/fruit-2.jpg)

    Execute este comando:

    ```PowerShell
    ./classify-image.ps1 2
    ```

1. Verifique se o modelo classifica essa imagem como **banana**.

1. Por fim, vamos tentar a terceira imagem de teste:

    ![Imagem de uma laranja](media/create-image-classification-system/fruit-3.jpg)

    Execute este comando:

    ```PowerShell
    ./classify-image.ps1 3
    ```

1. Verifique se o modelo classifica essa imagem como **laranja**.

## <a name="learn-more"></a>Saiba mais

Esse aplicativo simples mostra apenas alguns dos recursos do serviço de Visão Personalizada. Saiba mais sobre o que você pode fazer com esse serviço consultando a [página Visão Personalizada](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/).
