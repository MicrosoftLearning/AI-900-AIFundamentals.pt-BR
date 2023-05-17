---
lab:
  title: 'Explorar os Serviços Cognitivos'
---

# <a name="explore-cognitive-services"></a>Explorar os Serviços Cognitivos

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free?azure-portal=true) na qual tenha acesso administrativo.

Os Serviços Cognitivos do Azure encapsulam a funcionalidade de IA comum que pode ser categorizada em quatro pilares principais: visão, fala, linguagem e serviços de decisão. Neste exercício, você examinará um dos serviços de decisão para ter uma noção geral de como provisionar e usar um recurso de Serviços Cognitivos em um aplicativo de software.

O serviço cognitivo específico que você vai explorar neste exercício é o *Detector de Anomalias*. O Detector de Anomalias é usado para analisar valores de dados ao longo do tempo e para detectar valores incomuns que possam indicar um problema, permitindo uma investigação mais aprofundada. Por exemplo, um sensor em uma instalação de armazenamento controlada por temperatura pode monitorar a temperatura a cada minuto e registrar os valores medidos. Você pode usar o serviço Detector de Anomalias para analisar os valores de temperatura registrados e sinalizar qualquer um que fique significativamente fora da faixa normal de temperaturas esperadas.

Para testar os recursos do serviço de Detecção de Anomalias, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell. Os mesmos princípios e funcionalidades se aplicam a soluções do mundo real, como sites ou aplicativos de telefone.

> **Observação** A meta deste exercício é obter uma noção geral de como os Serviços Cognitivos são provisionados e usados. O Detector de Anomalias é usado como exemplo, mas não se espera obter um conhecimento abrangente da detecção de anomalias neste exercício.

## <a name="create-an-anomaly-detector-resource"></a>Criar um recurso do *Detector de Anomalias*

Vamos começar criando um recurso do **Detector de Anomalias** em sua assinatura do Azure:

1. Em outra guia do navegador, abra o portal do Azure em [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e entre com sua conta Microsoft.

1. Clique no botão **&#65291;Criar um recurso**, pesquise *Detector de Anomalias* e crie um recurso do **Detector de Anomalias** com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*.
    - **Grupo de recursos**: *selecione um grupo de recursos existente ou crie um novo*.
    - **Região**: *escolha uma região disponível*.
    - **Nome**: *insira um nome exclusivo*.
    - **Tipo de preço**: F0 gratuito

1. Revise e crie o recurso. Aguarde a conclusão da implantação e acesse o recurso implantado.

1. Exiba a página **Chaves e Ponto de Extremidade** do recurso do Detector de Anomalias. Você precisará do ponto de extremidade e das chaves para se conectar em aplicativos cliente.

## <a name="run-cloud-shell"></a>Executar o Cloud Shell

Para testar os recursos do serviço do Detector de Anomalias, usaremos um aplicativo de linha de comando simples que é executado no Cloud Shell do Azure.

1. No portal do Azure, selecione o botão **[>_]** (*Cloud Shell*) na parte superior da página à direita da caixa de pesquisa. Isso abre um painel do Cloud Shell na parte inferior do Portal.

    ![Inicie o Cloud Shell clicando no ícone à direita da caixa de pesquisa superior](media/anomaly-detector/powershell-portal-guide-1.png)

1. Na primeira vez que você abrir o Cloud Shell, talvez precise escolher o tipo de shell que deseja usar (*Bash* ou *PowerShell).* Selecione **PowerShell**. Se não vir essa opção, ignore a etapa.  

1. Se precisar criar o armazenamento para o Cloud Shell, verifique se sua assinatura está especificada e selecione **Criar armazenamento**. Aguarde um minuto para a criação do armazenamento.

    ![Crie o armazenamento com um clique em Confirmar.](media/anomaly-detector/powershell-portal-guide-2.png)

1. Verifique se o tipo de shell indicado na parte superior esquerda do painel do Cloud Shell indica *PowerShell*. Se for *Bash*, alterne para o *PowerShell* usando o menu suspenso.

    ![Como localizar o menu suspenso à esquerda para alternar para o PowerShell](media/anomaly-detector/powershell-portal-guide-3.png)

1. Aguarde o início do PowerShell. Você deverá ver a seguinte tela no portal do Azure:  

    ![Aguarde o início do PowerShell.](media/anomaly-detector/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>Configurar e executar um aplicativo cliente

Agora que você tem um ambiente de Cloud Shell, pode executar um aplicativo simples que usa o serviço do Detector de Anomalias para analisar uma imagem.

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

    ![O editor de código.](media/anomaly-detector/powershell-portal-guide-4.png)

1. No painel **Arquivos** à esquerda, expanda **ai-900** e selecione **detect-anomalies.ps1**. Esse arquivo contém algum código que usa o serviço de Detecção de Anomalias, como mostrado aqui:

    ![O editor que contém o código para detectar anomalias](media/anomaly-detector/detect-anomalies-code.png)

1. Não se preocupe muito com os detalhes do código, o importante é que ele precisa da URL do ponto de extremidade e de uma das chaves do seu recurso do Detector de Anomalias. Copie-os da página **Chaves e Pontos de Extremidade** do seu recurso (que ainda deve estar na área superior do navegador) e cole-os no editor de códigos, substituindo os valores de espaço reservado **YOUR_KEY** e **YOUR_ENDPOINT**, respectivamente.

    > **Dica** Talvez seja necessário usar a barra separadora para ajustar a área da tela durante o trabalho com os painéis **Chaves e ponto de extremidade** e **Editor**.

    Depois de colar os valores de chave e ponto de extremidade, as duas primeiras linhas de código devem ser semelhantes a esta:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. No canto superior direito do painel do editor, use o botão **...** para abrir o menu e selecione **Salvar** para salvar as alterações. Em seguida, abra o menu novamente e selecione **Fechar Editor**.

    A detecção de anomalias é uma técnica de inteligência artificial usada para determinar se os valores de uma série estão dentro dos parâmetros esperados. O aplicativo cliente de exemplo usará seu serviço do Detector de Anomalias para analisar um arquivo que contém uma série de datas/horas e valores numéricos. O aplicativo deve retornar resultados que indicam em cada ponto de tempo se o valor numérico está dentro dos parâmetros esperados.

1. No painel do PowerShell, insira os seguintes comandos para executar o código:

    ```PowerShell
    cd ai-900
    .\detect-anomalies.ps1
    ```

1. Revise os resultados, notando que a coluna final nos resultados é **True** ou **False** para indicar se o valor registrado em cada data/hora é considerado uma anomalia ou não. Considere como podemos usar essas informações em uma situação da vida real. Que ação o aplicativo poderia acionar se os valores fossem de temperatura da geladeira ou pressão arterial e anomalias fossem detectadas?  

## <a name="learn-more"></a>Saiba mais

Esse aplicativo simples mostra apenas alguns dos recursos do serviço do Detector de Anomalias. Saiba mais sobre o que você pode fazer com esse serviço consultando a [página Detector de Anomalias](https://azure.microsoft.com/services/cognitive-services/anomaly-detector/).

## <a name="clean-up"></a>Limpar

Recomendamos identificar no final do projeto se os recursos criados ainda serão necessários. Recursos deixados em execução podem custar dinheiro. 

Se você for continuar em outros módulos do curso Conceitos básicos de IA, mantenha os recursos para usá-los em outros laboratórios.

Caso tenha finalizado seu aprendizado, será possível excluir o grupo de recursos ou recursos individuais de sua assinatura do Azure:

1. No [portal do Azure](https://portal.azure.com/), na página **Grupos de recursos**, abra o grupo de recursos especificado durante a criação do recurso.

2. Clique em **Excluir grupo de recursos**, digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione **Excluir**. Também é possível optar por excluir recursos individuais ao selecionar os recursos, clicar nos três pontos para conferir mais opções, depois clicar em **Excluir**.