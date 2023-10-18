---
lab:
  title: Explorar o machine learning automatizado no Azure ML
---

# Explorar o machine learning automatizado no Azure ML

Neste exercício, você usará o recurso de machine learning automatizado no Azure Machine Learning para treinar e avaliar um modelo de machine learning. Em seguida, você implantará e testará o modelo treinado.

Este exercício deve levar aproximadamente **30** minutos para ser concluído.

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free) na qual tenha acesso administrativo.

## Criar um workspace do Azure Machine Learning

Para usar o Azure Machine Learning, você precisa provisionar um workspace do Azure Machine Learning em sua assinatura do Azure. Em seguida, você poderá usar o Estúdio do Azure Machine Learning para trabalhar com os recursos em seu workspace.

> **Dica**: se você já tiver um workspace do Azure Machine Learning, poderá usá-lo e pular para a próxima tarefa.

1. Entre no [portal do Azure](https://portal.azure.com) em `https://portal.azure.com` usando suas credenciais da Microsoft.

1. Selecione **+ Criar um recurso**, pesquise *Machine Learning* e crie um recurso do **Azure Machine Learning** com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*.
    - **Grupo de recursos**: *crie ou selecione um grupo de recursos*.
    - **Nome**: *insira um nome exclusivo para o seu workspace*.
    - **Região**: *selecione a região geográfica mais próxima*.
    - **Conta de armazenamento**: *anote a nova conta de armazenamento padrão que será criada para o workspace*.
    - **Cofre de chaves**: *anote o novo cofre de chaves padrão que será criado para o workspace*.
    - **Application Insights**: *anote o novo recurso Application Insights padrão que será criado para o workspace*.
    - **Registro de contêiner**: nenhum (*será criado um automaticamente quando você implantar um modelo em um contêiner pela primeira vez*).

1. Selecione **Examinar + criar**e **Criar**. Aguarde até que o workspace seja criado (isso pode demorar alguns minutos) e acesse o recurso implantado.

1. Selecione **Iniciar o estúdio** (ou abra uma nova guia do navegador, acesse [https://ml.azure.com](https://ml.azure.com?azure-portal=true) e entre no Estúdio do Azure Machine Learning usando a conta Microsoft). Feche todas as mensagens exibidas.

1. No Estúdio do Azure Machine Learning, você verá o workspace recém-criado. Caso contrário, selecione **Todos os workspaces** no menu à esquerda e selecione o workspace que você acabou de criar.

## Habilitar versão prévia de recursos

Alguns recursos do Azure Machine Learning estão em versão prévia e precisam ser habilitados explicitamente em seu workspace.

1. No Estúdio do Azure Machine Learning, clique em **gerenciar versões prévias do recursos** (o ícone de alto-falante - &#128363;).

    ![Uma captura de tela do botão gerenciar versões prévias dos recursos no menu.](../instructions/media/use-automated-machine-learning/severless-compute-1.png)

1. Habilite a seguinte versão prévia do recurso:

    - *Experiência guiada para enviar trabalhos de treinamento com computação sem servidor*

## Usar aprendizado de máquina automatizado para treinar um modelo

O aprendizado de máquina automatizado permite que você experimente vários algoritmos e parâmetros para treinar vários modelos e identificar o melhor para seus dados. Nesse caso, você usa um conjunto de dados de detalhes históricos de aluguel de bicicletas para treinar um modelo que prevê o número de aluguéis de bicicletas que deve ser esperado em um determinado dia, com base em características sazonais e meteorológicas.

> **Citação**: *os dados usados neste exercício são derivados do [Capital Bikeshare](https://www.capitalbikeshare.com/system-data) e são usados de acordo com o [contrato de licença](https://www.capitalbikeshare.com/data-license-agreement)* de dados publicado.


1. No [estúdio do Azure Machine Learning](https://ml.azure.com?azure-portal=true), veja a página **ML Automatizado** (em **Criação**).

1. Crie um novo trabalho de ML automatizado com as seguintes configurações, usando **Avançar** conforme necessário para avançar dentro da interface do usuário:

    **Configurações básicas**:

    - **Nome do trabalho**: mslearn-bike-automl
    - **Nome do novo experimento**: mslearn-bike-rental
    - **Descrição**: machine learning automatizado para previsão de aluguel de bicicletas
    - **Marcas**: *nenhuma*

   **Tipo de tarefa e dados**:

    - **Selecionar tipo de tarefa**: regressão
    - **Selecionar conjunto de dados**: crie um novo conjunto de dados com as seguintes configurações:
        - **Tipo de dados**:
            - **Nome**: bike-rentals
            - **Descrição**: dados históricos de aluguel de bicicletas
            - **Tipo**: tabular
        - **Fonte de dados**:
            - Selecione **De arquivos da Web**
        - **URL da Web**: 
            - **URL da Web**: `https://aka.ms/bike-rentals`
            - **Ignorar validação de dados**: *não selecionar*
        - **Configurações**:
            - **Formato de arquivo**: delimitado
            - **Delimitador**: vírgula
            - **Codificação**: UTF-8
            - **Cabeçalhos de coluna**: somente o primeiro arquivo tem cabeçalhos
            - **Ignorar linhas**: Nenhum
            - **O conjunto de dados contém dados multilinhas**: *não selecione*
        - **Esquema**:
            - incluir todas as colunas que não sejam **Caminho**
            - Examinar os tipos detectados automaticamente

        Selecione o conjunto de dados de **aluguel de bicicletas** depois de criá-lo.

    **Configurações da tarefa**:

    - **Tipo de tarefa**: regressão
    - **Conjunto de dados**: bike-rentals
    - **Coluna de destino**: aluguéis (inteiro)
    - **Definições de configuração adicionais**:
        - **Métrica primária**: erro quadrático médio da raiz normalizada
        - **Explicar o melhor modelo**: *não selecionado*
        - **Usar todos os modelos com suporte**: <u>Não</u>selecionado. *Você restringirá o trabalho para experimentar apenas alguns algoritmos específicos.*
        - **Modelos permitidos**: *selecione apenas **RandomForest** e **LightGBM**. O ideal seria tentar usar o máximo possível, mas cada modelo adicionado aumenta o tempo necessário para executar o trabalho.*
    - **Limites**: *expanda esta seção*
        - **Avaliações máximas**: 3
        - **Máximo de avaliações simultâneas**: 3
        - **Máximo de nós**: 3
        - **Limite de pontuação da métrica**: 0,85 (*de modo que se um modelo atingir uma pontuação de métrica de raiz do erro quadrático médio normalizada de até 0,085, o trabalho será encerrado.* )
        - **Tempo limite**: 15
        - **Tempo limite de iteração**: 5
        - **Habilitar encerramento antecipado**: *selecionado*
    - **Validação e teste**:
        - **Tipo de validação**: divisão de validação de treinamento
        - **Percentual de dados de validação**: 10
        - **Conjunto de dados de teste**: nenhum

    **Computação**:

    - **Selecionar tipo de computação**: sem servidor
    - **Tipo de máquina virtual**: CPU
    - **Camada da máquina virtual**: dedicada
    - **Tamanho da máquina virtual**: Standard_DS3_V2
    - **Número de instâncias**: 1

1. Envie o trabalho de treinamento. Ele é iniciado automaticamente.

1. Aguarde a conclusão do trabalho. Isso pode demorar um pouco, então agora é um bom momento para um café.

## Examinar o melhor modelo

Quando o trabalho de machine learning automatizado for concluído, você poderá examinar o melhor modelo treinado.

1. Na guia **Visão geral** do trabalho de machine learning automatizado, observe o resumo do melhor modelo.
    ![Captura de tela do resumo do melhor modelo do trabalho de machine learning automatizado com uma caixa em torno do nome do algoritmo.](media/use-automated-machine-learning/complete-run.png)

    > **Note**: você pode ver uma mensagem sob o status "Aviso: pontuação de saída especificada pelo usuário atingida...". Essa é uma mensagem esperada. Continue na próxima etapa.
  
1. Selecione o texto em **Nome do algoritmo** do melhor modelo para exibir os respectivos detalhes.

1. Selecione a guia **Métricas** e selecione os gráficos **residuais** e **predicted_true** se eles ainda não estiverem selecionados. 

    Examine os gráficos que mostram o desempenho do modelo. O gráfico de **resíduos** mostra os *resíduos* (as diferenças entre valores previstos e reais) como um histograma. O gráfico **predicted_true** compara os valores previstos com os valores verdadeiros. 

## Implantar e testar o modelo

1. Na guia **Modelo** para obter o melhor modelo treinado pelo trabalho de machine learning automatizado, selecione **Implantar** e use a opção **serviço Web** para implantar o modelo com as seguintes configurações:
    - **Nome**: predict-rentals
    - **Descrição**: prever aluguéis de bicicleta
    - **Tipo de computação**: instância de Contêiner do Azure
    - **Habilitar autenticação**: *Selecionado*

1. Aguarde até o início da implantação – isso pode levar alguns segundos. O **Status de implantação** para o ponto de extremidade **predict-rentals** será indicado na parte principal da página como *Em execução*.
1. Aguarde até que o **Status de implantação** mude para *Bem-sucedida*. Isso pode levar de 5 a 10 minutos.

## Testar o serviço implantado

Agora você pode testar o serviço implantado.

1. Em Estúdio do Azure Machine Learning, no menu à esquerda, selecione **Pontos de Extremidade** e abra o ponto de extremidade em tempo real **predict-rentals**.

1. Na página do ponto de extremidade em tempo real de **previsão de aluguel**, exiba a guia **Teste**.

1. No painel **Dados de entrada para testar o ponto de extremidade**, substitua o modelo JSON pelos seguintes dados de entrada:

    ```JSON
    {
      "Inputs": { 
        "data": [
          {
            "day": 1,
            "mnth": 1,   
            "year": 2022,
            "season": 2,
            "holiday": 0,
            "weekday": 1,
            "workingday": 1,
            "weathersit": 2, 
            "temp": 0.3, 
            "atemp": 0.3,
            "hum": 0.3,
            "windspeed": 0.3 
          }
        ]    
      },   
      "GlobalParameters": 1.0
    }
    ```

1. Clique no botão**Testar**.

1. Revise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada – semelhante ao seguinte:

    ```JSON
    {
      "Results": [
        444.27799000000000
      ]
    }
    ```

    O painel de teste pegou os dados de entrada e usou o modelo que você treinou para retornar o número previsto de locações.

Vamos revisar o que você fez. Você usou um conjunto de dados históricos de locação de bicicletas para treinar um modelo. O modelo prevê o número de locações de bicicletas esperadas em um determinado dia, com base em *recursos *sazonais e meteorológicos.

## Limpar

O serviço Web que você criou está hospedado em uma *Instância de Contêiner do Azure*. Se você não pretender experimentá-lo ainda mais, exclua o ponto de extremidade para evitar o acúmulo de uso desnecessário do Azure.

1. No [estúdio do Azure Machine Learning](https://ml.azure.com?azure-portal=true), na guia **Pontos de extremidade**, selecione o ponto de extremidade **predict-rentals**. Depois, selecione **Excluir** e confirme que você deseja excluir o ponto de extremidade.

    Excluir sua computação garante que a assinatura não seja cobrada pelos recursos de computação. No entanto, você receberá a cobrança de uma pequena quantidade de armazenamento de dados, desde que o workspace do Azure Machine Learning exista em sua assinatura. Se tiver terminado de explorar o Azure Machine Learning, exclua o workspace do Azure Machine Learning e os recursos associados.

Para excluir seu workspace:

1. No [portal do Azure](https://portal.azure.com?azure-portal=true), na página **Grupos de recursos**, abra o grupo de recursos que você especificou ao criar seu Workspace do Azure Machine Learning.
2. Clique em **Excluir grupo de recursos**, digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione **Excluir**.
