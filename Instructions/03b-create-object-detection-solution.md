---
lab:
  title: Explorar a detecção de objetos
---

# <a name="explore-object-detection"></a>Explorar a detecção de objetos

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free?azure-portal=true) na qual tenha acesso administrativo.

*A detecção de objetos* é uma forma de pesquisa visual computacional na qual um modelo de machine learning é treinado para classificar instâncias individuais de objetos em uma imagem e indicar uma *caixa delimitadora* que marca o local. Você pode pensar nisso como uma progressão da *classificação de imagem* (na qual o modelo responde à pergunta "de que se trata essa imagem?") para criar soluções em que podemos perguntar ao modelo "quais objetos estão nessa imagem e onde eles estão?".

Por exemplo, uma mercearia pode usar um modelo de detecção de objetos para implementar um sistema de check-out automatizado que examina uma esteira rolante usando uma câmera e pode identificar itens específicos sem a necessidade de colocar cada item na esteira e examiná-los individualmente.

O serviço cognitivo **Visão Personalizada** no Microsoft Azure fornece uma solução baseada em nuvem para criação e publicação de modelos de detecção de objetos personalizados. No Azure, você pode usar o serviço Visão Personalizada para treinar um modelo de classificação de imagem com base em imagens existentes. A criação de uma solução de classificação de imagem envolve dois elementos. Primeiro, você deve treinar um modelo para reconhecer classes diferentes usando imagens existentes. Depois, após o treinamento do modelo, você deverá publicá-lo como um serviço que pode ser consumido por aplicativos.

Para testar os recursos do serviço de Visão Personalizada a fim de detectar objetos em imagens, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell. Os mesmos princípios e funcionalidades se aplicam a soluções do mundo real, como sites ou aplicativos de telefone.

## <a name="create-a-cognitive-services-resource"></a>Criar um recurso dos *Serviços Cognitivos*

Você pode usar o serviço Visão Personalizada criando um recurso de **Visão Personalizada** ou um recurso dos **Serviços Cognitivos**.

> **Observação** Nem todos recursos estão disponíveis em todas as regiões. Se você criar um recurso de Visão Personalizada ou de Serviços Cognitivos, somente recursos criados em [determinadas regiões](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) podem ser usados para acessar serviços de Visão Personalizada. Para simplificar, uma região é pré-selecionada para você nas instruções de configuração abaixo.

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

1. Em uma nova guia do navegador, abra o portal de Visão Personalizada em [https://customvision.ai](https://customvision.ai?azure-portal=true) e entre usando a conta Microsoft associada à sua assinatura do Azure.

1. Crie um novo projeto com as seguintes configurações:
    - **Nome:** detecção de itens de mercearia
    - **Descrição:** detecção de objetos para itens de mercearia.
    - **Recurso**: *o recurso criado anteriormente*
    - **Tipos de projeto**: detecção de objetos
    - **Domínios**: Geral

1. Aguarde a criação e abertura do projeto no navegador.

## <a name="add-and-tag-images"></a>Adicionar e marcar imagens

Para treinar um modelo de detecção de objeto, você precisa carregar imagens que contenham as classes que serão identificadas pelo modelo e marcá-las para indicar caixas delimitadoras para cada instância de objeto.

1. Baixe e extraia as imagens de treinamento do https://aka.ms/fruit-objects. A pasta extraída contém uma coleção de imagens de frutas.

1. No portal de Visão Personalizada [https://customvision.ai](https://customvision.ai?azure-portal=true), verifique se você está trabalhando em seu projeto de detecção de objetos _Detecção de itens de mercearia_. Em seguida, selecione **Adicionar imagens** e carregue todas as imagens na pasta extraída.

    ![Carregue as imagens baixadas clicando em adicionar imagens.](media/create-object-detection-solution/fruit-upload.jpg)

1. Depois de carregar as imagens, selecione a primeira para abri-la.

1. Mantenha o mouse sobre qualquer objeto na imagem até que uma região detectada automaticamente seja exibida, como na imagem abaixo. Em seguida, selecione o objeto e, se necessário, reorganize a região para cercá-lo.

    ![A região padrão de um objeto](media/create-object-detection-solution/object-region.jpg)

    Como alternativa, você pode simplesmente arrastar o objeto para criar uma região.

1. Quando a região envolver o objeto, adicione uma nova marcação com o tipo de objeto apropriado (*maçã,**banana*ou *laranja*), conforme mostrado aqui:

    ![Um objeto marcado em uma imagem](media/create-object-detection-solution/object-tag.jpg)

1. Selecione e marque os outros objetos na imagem, redimensionando as regiões e adicionando novas marcações conforme o necessário.

    ![Dois objetos marcados em uma imagem](media/create-object-detection-solution/object-tags.jpg)

1. Use o link **>** à direita para ir para a próxima imagem e marcar os objetos. Em seguida, continue trabalhando em toda a coleção de imagens, marcando cada maçã, banana e laranja.

1. Quando terminar de marcar a última imagem, feche o editor **Detalhes da Imagem** e, na página **Imagens de Treinamento**, em **Marcações**, selecione **Marcado** para ver todas as suas imagens marcadas:

    ![Imagens marcadas em um projeto](media/create-object-detection-solution/tagged-images.jpg)

## <a name="train-and-test-a-model"></a>Treinar e testar um modelo

Agora que você marcou as imagens em seu projeto, está tudo pronto para treinar um modelo.

1. No projeto Visão Personalizada, clique em **Treinar** para treinar um modelo de detecção de objeto usando as imagens marcadas. Selecione a opção **Treinamento Rápido**.

1. Aguarde a conclusão do treinamento (pode demorar cerca de dez minutos) e, em seguida, revise as métricas de desempenho de *Precisão,*, *Recall* e *mAP*. Essas métricas medem a utilidade da previsão do modelo de detecção de objetos e devem ser altas.

1. No canto superior direito da página, clique em **Teste Rápido** e, na caixa **URL da Imagem**, insira `https://aka.ms/apple-orange` e veja a previsão gerada. Em seguida, feche a janela **Teste Rápido**.

## <a name="publish-the-object-detection-model"></a>Publicar o modelo de detecção de objeto

Agora está tudo pronto para publicar seu modelo treinado e usá-lo em um aplicativo cliente.

1. Clique em **&#128504; Publicar** para publicar o modelo treinado com as seguintes configurações:
    - **Nome do modelo**: detect-produce
    - **Recurso de previsão**: *o recurso que você já havia criado*.

1. Após a publicação, clique no ícone *URL de Previsão* (&#127760;) para ver as informações necessárias para usar o modelo publicado. Posteriormente, você precisará da URL e dos valores de Prediction-Key apropriados para obter uma previsão de uma URL de imagem. Portanto, mantenha essa caixa de diálogo aberta e vá para a próxima tarefa.

## <a name="run-cloud-shell"></a>Executar o Cloud Shell

Para testar os recursos do serviço de Visão Personalizada, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell no Azure.

1. No portal do Azure, selecione o botão **[>_]** (*Cloud Shell*) na parte superior da página à direita da caixa de pesquisa. Isso abre um painel do Cloud Shell na parte inferior do Portal. 

    ![Inicie o Cloud Shell clicando no ícone à direita da caixa de pesquisa superior](media/create-object-detection-solution/powershell-portal-guide-1.png)

1. Na primeira vez que você abrir o Cloud Shell, talvez precise escolher o tipo de shell que deseja usar (*Bash* ou *PowerShell).* Selecione **PowerShell**. Se não vir essa opção, ignore a etapa.  

1. Se precisar criar o armazenamento para o Cloud Shell, verifique se sua assinatura está especificada e selecione **Criar armazenamento**. Aguarde um minuto para a criação do armazenamento.

    ![Crie o armazenamento com um clique em Confirmar.](media/create-object-detection-solution/powershell-portal-guide-2.png)

1. Verifique se o tipo de shell indicado na parte superior esquerda do painel do Cloud Shell indica *PowerShell*. Se for *Bash*, alterne para o *PowerShell* usando o menu suspenso.

    ![Como localizar o menu suspenso à esquerda para alternar para o PowerShell](media/create-object-detection-solution/powershell-portal-guide-3.png) 

1. Aguarde o início do PowerShell. Você deverá ver a seguinte tela no portal do Azure:  

    ![Aguarde o início do PowerShell.](media/create-object-detection-solution/powershell-prompt.png) 

## <a name="configure-and-run-a-client-application"></a>Configurar e executar um aplicativo cliente

Agora que você tem um modelo personalizado, pode executar um aplicativo cliente simples que usa o serviço Visão Personalizada para detectar objetos em uma imagem.

1. No shell de comando, digite o comando a seguir para baixar o aplicativo de exemplo e salvá-lo em uma pasta chamada ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >**Observação** Se você já usou esse comando em outro laboratório para clonar o repositório *ai-900*, ignore esta etapa.

1. Os arquivos são baixados em uma pasta chamada **ai-900**. Agora queremos ver todos os arquivos em seu armazenamento do Cloud Shell e trabalhar com eles. Digite o seguinte comando no shell:

    ```PowerShell
    code .
    ```

    Observe como isso abre um editor como o da imagem abaixo: 

    ![O editor de código.](media/create-object-detection-solution/powershell-portal-guide-4.png)

1. No painel **Arquivos** à esquerda, expanda **ai-900** e selecione **detect-objects.ps1**. Esse arquivo contém um código que usa o serviço Visão Personalizada para detectar objetos em uma imagem, como mostrado aqui:

    ![O editor que contém o código para detectar itens em uma imagem](media/create-object-detection-solution/detect-image-code.png)

1. Não se preocupe tanto com os detalhes do código, o importante é que ele precisa da URL de previsão e da chave de seu modelo de Visão Personalizada ao usar uma URL de imagem. 

    Obtenha a *URL de previsão* na caixa de diálogo no projeto de Visão Personalizada. 

    >**Observação** Lembre-se de que você examinou a *URL de previsão* depois de publicar o modelo de classificação de imagem. Para encontrar a *URL de previsão*, navegue até a guia **Desempenho** no projeto e clique em **URL de Previsão** (se a tela estiver compactada, você poderá ver um ícone de globo). Uma caixa de diálogo será exibida. Copie a URL de **Se você tiver uma URL de imagem**. Cole-a no editor de códigos, substituindo **URL_DE_PREVISÃO**. 

    Usando a mesma caixa de diálogo, obtenha a *chave de previsão*. Copie a chave de previsão exibida depois de *Definir o Cabeçalho Prediction-Key como*. Cole-a no editor de códigos, substituindo o valor de espaço reservado **YOUR_PREDICTION_KEY**. 

    ![Captura de tela da URL de previsão.](media/create-object-detection-solution/find-prediction-url.png)

    Depois de colar os valores de URL de Previsão e Chave de Previsão, as duas primeiras linhas de código devem ser semelhantes a esta:

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

1. No canto superior direito do painel do editor, use o botão **...** para abrir o menu e selecione **Salvar** para salvar as alterações. Em seguida, abra o menu novamente e selecione **Fechar Editor**.

    Você usará o aplicativo cliente de exemplo para detectar objetos nesta imagem:

    ![Imagem de uma fruta](media/create-object-detection-solution/produce.jpg)

1. No painel do PowerShell, insira o seguinte comando para executar o código:

    ```PowerShell
    cd ai-900
    ./detect-objects.ps1 
    ```

1. Revise a previsão, que deve ser *maçã laranja banana*.

## <a name="learn-more"></a>Saiba mais

Esse aplicativo simples mostra apenas alguns dos recursos do serviço de Visão Personalizada. Saiba mais sobre o que você pode fazer com esse serviço consultando a [página Visão Personalizada](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/).
