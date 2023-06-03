---
lab:
  title: Explorar o recurso respostas às perguntas
---

# <a name="explore-question-answering"></a>Explorar o recurso respostas às perguntas

> **Observação** Para concluir este laboratório, você precisará de uma [assinatura do Azure](https://azure.microsoft.com/free?azure-portal=true) na qual tenha acesso administrativo.

Para cenários de suporte ao cliente, é comum criar um bot que possa interpretar e responder a perguntas frequentes por meio de uma janela de chat no site, email ou interface de voz. Sob a interface do bot há uma base de dados de conhecimento de perguntas e respostas apropriadas na qual o bot pode pesquisar em busca de respostas adequadas.

## <a name="create-a-custom-question-answering-knowledge-base"></a>Criar uma base de dados de conhecimento de respostas às perguntas personalizadas

O recurso de respostas às perguntas personalizadas do serviço de Linguagem permite a criação rápida de uma base de dados de conhecimento, por meio da inserção de pares de pergunta e resposta ou de um documento ou página da Web existente. Em seguida, ele pode usar alguns recursos de processamento de linguagem natural integrados para interpretar perguntas e encontrar as respostas certas.

1. Abra o portal do Azure em [https://portal.azure.com](https://portal.azure.com?azure-portal=true), entrando com a sua conta Microsoft.

1. Clique no botão **&#65291;Criar um recurso**, procure *Serviço de Linguagem* e crie um recurso de **serviço de Linguagem** com as seguintes configurações e clique em **Continuar para criar o recurso**: **Selecionar recursos adicionais**
    - **Recursos padrão**: *mantenha os recursos padrão*.
    - **Recursos personalizados**: *selecione a opção respostas às perguntas personalizadas*.

    ![Criação de um recurso de Serviço de Linguagem com o recurso de respostas às perguntas personalizadas habilitado.](media/create-a-bot/create-language-service-resource.png)

1. Na página **Criar linguagem**, especifique as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*.
    - **Grupo de recursos**: *selecione um grupo de recursos existente ou crie um novo*.
    - **Nome**: *um nome exclusivo para o recurso de Linguagem*.
    - **Tipo de preço**: S (mil chamadas por minuto)
    - **Localização do Azure Search**: *qualquer localização disponível*.
    - **Tipo de preço do Azure Search:** gratuito F (3 índices) – (*Se esse tipo não estiver disponível, selecione Standard S (50 índices)* )
    - **Ao marcar esta caixa, declaro que analisei e confirmo os termos do Aviso de IA responsável**: *selecionada*.

    > **Observação** Se você já tiver provisionado um recurso do **Azure Cognitive Search** de camada gratuita, talvez a cota não permita a criação de outro. Nesse caso, selecione uma camada diferente de **Free F**.

1. Clique em **Revisar e criar** e depois clique em **Criar**. Aguarde a implantação do serviço de Linguagem que dará suporte à sua base de dados de conhecimento de respostas às perguntas personalizadas.

1. Em uma nova guia do navegador, abra o portal do Language Studio em [https://language.azure.com](https://language.azure.com?azure-portal=true) e entre usando a conta Microsoft associada à sua assinatura do Azure.

1. Se for solicitado a escolher um recurso de idioma, selecione as seguintes configurações:
    - **Azure Directory**: o diretório do Azure que contém a assinatura.
    - **Assinatura do Azure**: sua assinatura do Azure.
    - **Recurso de idioma**: o recurso de idioma criado anteriormente.

1. Se você ***não*** foi solicitado a escolher um recurso de idioma, pode ser porque você tem vários recursos de idioma em sua assinatura; nesse caso:
    1. Na barra na parte superior da página, clique no botão **Configurações (&#9881;)**.
    2. Na página **Configurações**, exiba a guia **Recursos**.
    3. Selecione o recurso de idioma que você acabou de criar e clique em **Alternar recurso**.
    4. Na parte superior da página, clique em **Language Studio** para retornar à home page do Language Studio.

1. Na parte superior do portal do Language Studio, no menu **Criar**, selecione **Respostas às perguntas personalizadas**.

1. Na página **Escolher configuração de idioma para o *recurso***, selecione **Quero selecionar o idioma quando criar um projeto neste recurso** e clique em **Avançar**.

1. Na página **Inserir informações básicas**, insira os seguintes detalhes e clique em **Avançar**:
    - **Recurso de idioma**: *escolha o seu recurso de idioma*.  
    - **Recurso de pesquisa do Azure**: *escolha o recurso de pesquisa do Azure*.
    - **Nome**: MargiesTravel
    - **Descrição**: uma base de dados de conhecimento simples
    - **Idioma de origem**: inglês
    - **Resposta padrão quando nenhuma resposta é retornada**: Nenhuma resposta encontrada

1. Na página **Examinar e concluir**, clique em **Criar projeto**.

1. Você será levado à página **Gerenciar origens**. Clique em **&#65291;Adicionar origem** e selecione **URLs**.

1. Na caixa **Adicionar URLs**, clique em **+ Adicionar URL**. Digite o seguinte e selecione **Adicionar tudo**:
    - **Nome da URL**: MargiesKB
    - **URL**: `https://raw.githubusercontent.com/MicrosoftLearning/AI-900-AIFundamentals/main/data/qna/margies_faq.docx`
    - **Classificar estrutura de arquivo**: *Detectar automaticamente* 

## <a name="edit-the-knowledge-base"></a>Editar a base de dados de conhecimento

Sua base de dados de conhecimento se baseia nos detalhes contidos no documento de perguntas frequentes e em algumas respostas pré-definidas. Você pode adicionar pares personalizados de perguntas e respostas para complementá-los.

1. Clique em **Editar base de dados de conhecimento** no painel esquerdo. Em seguida, clique em **+ Adicionar par de perguntas**.

1. Na caixa **Perguntas**, digite `Hello`e clique em **Enviar alterações**.

1. Clique em **+ Adicionar frase alternativa**, digite `Hi` e clique em **Enviar alterações**.

1. Na caixa **Resposta e solicitações**, digite `Hello`. Mantenha a **Origem**: Editorial.

1. Clique em **Enviar**. Em seguida, na parte superior da página, clique em **Salvar alterações**. Talvez seja necessário alterar o tamanho da janela para ver o botão.

## <a name="train-and-test-the-knowledge-base"></a>Treinar e testar a base de dados de conhecimento

Agora que você tem uma base de dados de conhecimento, pode testá-la.

1. Na parte superior da página, clique em **Testar** para testar sua base de dados de conhecimento.

1. No painel de teste, na parte inferior, insira a mensagem *Olá*. A resposta **Olá** deve retornar.

1. No painel de teste, na parte inferior, insira a mensagem *Quero reservar um voo*. Uma resposta apropriada das perguntas frequentes deve retornar.

    > **Observação** A resposta inclui uma *resposta curta*, bem como uma *passagem de resposta* mais detalhada. A passagem de resposta mostra o texto completo no documento de perguntas frequentes da pergunta com a correspondência mais próxima, enquanto a resposta curta é extraída da passagem de maneira inteligente. Você pode controlar se quer mostrar a resposta curta usando a caixa de seleção **Exibir resposta curta** na parte superior do painel de teste.

1. Tente outra pergunta, por exemplo, *Como posso cancelar uma reserva?*

1. Quando terminar de testar a base de dados de conhecimento, clique em **Testar** para fechar o painel de teste.

## <a name="create-a-bot-for-the-knowledge-base"></a>Criar um bot para a base de dados de conhecimento

A base de dados de conhecimento fornece um serviço de back-end que os aplicativos cliente podem usar para responder a perguntas por meio de algum tipo de interface do usuário. Normalmente, esses aplicativos cliente são bots. Para disponibilizar a base de dados de conhecimento a um bot, você deve publicá-la como um serviço que possa ser acessado por HTTP. Então, você pode usar o Serviço de Bot do Azure para criar e hospedar um bot que usa a base de dados de conhecimento para responder a perguntas do usuário.

1. À esquerda da página do Language Studio, clique em **Implantar base de dados de conhecimento**.

1. Na parte superior da página, clique em **Implantar** e clique em **Implantar** novamente.

1. Após a implantação do serviço, clique em **Criar um bot**. Isso abre o portal do Azure em uma nova guia do navegador para que você possa criar um Bot de Aplicativo Web em sua assinatura do Azure.

1. No portal do Azure, crie um Bot de Aplicativo Web com as seguintes configurações (a maioria delas será preenchida previamente para você):
    - **Identificador do bot**: *insira um nome exclusivo para o bot*
    - **Assinatura**: *sua assinatura do Azure*
    - **Grupo de recursos**: *o grupo de recursos que contém seu recurso de Linguagem*
    - **Local**: *o mesmo local do serviço de linguagem*.
    - **Tipo de preço**: gratuito (F0)
    - **Nome do aplicativo**: *o mesmo que a **Alça do bot** com **.azurewebsites.net** anexado automaticamente*
    - **Linguagem do SDK**: *escolha C# ou Node.js*
    - **Chave de Recurso de Linguagem**: *gerada automaticamente, se você não a vir, precisará começar criando um projeto de respostas às perguntas no Language Studio* 
    - **Plano do serviço de aplicativo/Local**: *selecione a seta para criar um plano. Em seguida, crie um nome de plano de serviço de aplicativo exclusivo e escolha um local adequado*
    - **Aplicativo Insights**: desligado
    - **Senha e ID do Aplicativo da Microsoft**: *criar automaticamente ID e senha do aplicativo*

1. Aguarde a criação de seu bot (o ícone de notificação no canto superior direito, que se parece com um sino, passará a ser uma animação enquanto você aguarda). Em seguida, na notificação de conclusão da implantação, clique em **Ir para o recurso** (ou, como alternativa, na página inicial, clique em **Grupos de recursos**,abra o grupo de recursos em que você criou o bot do aplicativo Web e clique nele.)

1. No painel à esquerda do bot, procure **Configurações**, clique em **Testar no Webchat** e aguarde até que o bot exiba a mensagem **Olá e bem-vindo(a)** (a inicialização pode demorar alguns segundos).

1. Use a interface de chat de teste para garantir que o bot responda às perguntas da sua base de dados de conhecimento conforme o esperado. Por exemplo, tente enviar *Preciso cancelar meu hotel*.

Faça experimentos com o bot. Provavelmente, você verá que ele pode responder a perguntas frequentes com bastante precisão, mas terá capacidade limitada de interpretar perguntas para as quais não foi treinado. Use, a qualquer momento, o Language Studio para editar a base de dados de conhecimento a fim de aprimorá-la e, depois, publique-a novamente.

## <a name="learn-more"></a>Saiba mais

- Para saber mais sobre o serviço de Respostas às Perguntas, confira [a documentação](https://docs.microsoft.com/azure/cognitive-services/language-service/question-answering/overview).
- Para saber mais sobre o Serviço de Bot da Microsoft, veja [a página do Serviço de Bot do Azure](https://azure.microsoft.com/services/bot-service/).
