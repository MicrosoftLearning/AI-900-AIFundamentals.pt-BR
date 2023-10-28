---
lab:
  title: Explore a IA generativa com o Bing Copilot
---

Neste exercício, você explorará a IA generativa com o Bing Copilot. 

## Antes de começar
Você precisa de uma conta pessoal da Microsoft. Se não tiver uma, acesse [signup.live.com](https://signup.live.com/signup?azure-portal=true) para criar uma conta.

# Explore a IA generativa com o Bing

1. Abra o site [Bing.com](https://www.bing.com?azure-portal=true) e entre com sua conta pessoal da Microsoft.

**Observação**: embora você possa entrar com sua conta corporativa ou de estudante, a experiência do usuário será um pouco diferente da experiência ao se conectar com sua conta pessoal. Usando sua conta corporativa ou de estudante, você verá o chat do Bing Enterprise. 

1. Selecione **Chat** no menu na parte superior da tela. O chat leva você ao Bing Copilot, que usa IA generativa para fornecer os resultados das pesquisas. Isso significa que, ao contrário de uma pesquisa isolada, que retorna os conteúdos existentes, o Bing Copilot pode criar novas respostas com base na modelagem de linguagem natural e nas informações da Web.  
    
1. Na parte inferior da tela, você verá uma janela chamada **Pergunte-me qualquer coisa**. Ao inserir prompts na janela, o Bing Copilot usa toda a conversa para retornar as respostas. Por exemplo, vamos tentar fazer uma série de perguntas sobre viagens. 

## Usar prompts para gerar respostas

1. Digite em um prompt: `What are 3 pros and cons of traveling in the winter?`. Você verá as mensagens *Procurando por: ...* e *Gerando...* aparecerem antes da resposta. O modelo usa as respostas pesquisadas como informações de base para gerar respostas originais. Observe que o final da resposta contém links para as respectivas fontes. 

![Uma captura de tela da resposta do Bing Copilot a um prompt de viagem com três tópicos de vantagens e três tópicos de desvantagens.](../media/generative-ai/bing-copilot-response-traveling.png) 

**Observação**: se você não vir uma mensagem **Gerando...* ou uma resposta de lista de tópicos, você ainda não viu o Bing Copilot em ação. Você precisa retornar ao menu de entrada e conectar a conta atual que está usando com uma conta pessoal. 
 
1. Digite um prompt: `Find me 3 more pros`. O que você quer dizer com esse prompt é que gostaria de ver mais 3 vantagens de viajar no inverno que ainda não foram listadas. Observe que, com esse prompt, você está pedindo que o Bing Copilot faça duas coisas que uma pesquisa isolada não faz: usar a resposta do chat anterior para excluir o que é retornado na nova resposta e usar o tópico do chat anterior sem mencioná-lo explicitamente. 

1. Digite um prompt: `Where are 3 places I can go to find fewer crowds?`. 

**Observação**: observe que, embora o Bing Copilot seja capaz de dar uma resposta relacionada, ele pode abandonar as "memórias" anteriores da conversa à medida que ela continua. Como resultado, as respostas que você recebe talvez não estejam diretamente relacionadas a viagens no inverno. Isso se deve, em grande parte, às limitações da entrada de tokens. Quando o chat se "lembra" de partes anteriores de uma conversa, é porque ele salvou uma determinada quantidade de tokens da conversa. À medida que novos tokens são introduzidos por meio de novos prompts e respostas, o chat deixará de usar os tokens mais antigos. 

1. O botão **Novo Tópico** ao lado da janela de chat é útil para que o Bing Copilot limpe o thread da conversa anterior para que as respostas relacionadas ao novo tópico não tenham como base o tópico anterior. Use o ícone **Novo Tópico** próximo à janela de chat para limpar o histórico de mensagens. 

## Experimente a geração de imagens

1. Agora vamos ver um exemplo de geração de imagem. Digite um prompt: `Create an image of an elephant eating a hamburger`. Observe que a mensagem *Vou tentar criar isso...* será exibida antes que o Bing Copilot retorne uma resposta. 

![Uma captura de tela de elefantes comendo hambúrguer.](../media/generative-ai/dall-e-elephant.png)

É importante observar que a resposta pode ser semelhante, mas não igual. Isso ocorre porque as respostas são variadas.  

1. Na resposta, há um texto na parte inferior que diz "Da plataforma DALL-E". Considere como o DALL-E se baseia em grandes modelos de linguagem à medida que sua entrada em linguagem natural gera imagens. 

1. Volte ao chat do Bing Copilot clicando no ícone do Microsoft Bing no canto superior direito da tela. 

## Experimente a geração de códigos

1. Vejamos agora um exemplo de geração e tradução de código. Digite um prompt: `Use Python to create a list`. 

1. Digite no prompt: `Translate that into C#`. Observe como você não precisou especificar o que era "isso", já que o Bing Copilot sabe conferir o histórico da conversa. 

## Bonus 

1. Digite um prompt: `What are 3 examples of generative AI helping people?`. Você pode usar isso como uma forma de se inspirar para pensar nas suas próprias ideias para usar o copiloto!  

