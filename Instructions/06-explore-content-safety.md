O Content Safety Studio permite explorar como o conteúdo de texto e imagem pode ser moderado. Execute testes em textos ou imagens de exemplo e obtenha uma pontuação de severidade que varia de seguro a alto para cada categoria. Descubra rapidamente como o serviço de IA de Segurança de Conteúdo funciona e do que ele é capaz. 

Neste exercício de laboratório, você criará um recurso de Serviços de IA do Azure de vários serviços no portal do Azure e inspecionará seu ponto de extremidade e chaves. Em seguida, usará o Content Safety Studio para explorar a funcionalidade do serviço de IA de Segurança de Conteúdo. 

## Criar um recurso dos Serviços de IA

1.  Em outra guia do navegador, abra o portal do Azure em [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e entre com sua conta Microsoft.
1.  Clique no botão **&#65291;Criar um recurso** e pesquise por *serviços de IA do Azure*. Selecione **criar** um plano de **serviços de IA do Azure**. Você será levado para uma página para criar um recurso dos serviços de IA do Azure. Defina-o com as seguintes configurações:
- **Assinatura**: Sua assinatura do Azure.
- **Grupo de recursos**: selecione ou crie um grupo de recursos adequado.
- **Região**: escolha qualquer região disponível.
- **Nome**: Insira um nome exclusivo.
- **Tipo de preço**: F0 
2.  Examine e crie o recurso e aguarde a conclusão da implantação. 

## Associar o recurso ao Content Safety Studio 
Em uma guia separada do navegador, abra o Content Safety Studio e conecte-se. A tela Introdução é exibida.

1.  Clique na engrenagem Configurações no menu superior direito.
2.  Selecione o recurso de serviço de IA do Azure recém-criado e selecione Usar recurso.
3.  Exiba o ponto de extremidade e a chave do recurso, rolando, se necessário. Caso realize check-in no Portal do Azure, você verá que esse é o ponto de extremidade e a chave do recurso, mostrando que você associou com êxito o recurso ao Content Safety Studio.
4.  Selecione Content Safety Studio para retornar à página inicial. Em Conteúdo do texto moderação, selecione Experimentar.
5.  Em Executar um teste simples, selecione Conteúdo Seguro. Observe que o texto é exibido na caixa abaixo. 
6.  Clique em Executar teste. 
7.  No painel Resultados, inspecione os resultados. Há quatro níveis de gravidade, de seguro a alto, e quatro tipos de conteúdo prejudicial. O serviço de IA de Segurança de Conteúdo considera esse exemplo aceitável ou não? 
8.  Agora, tente outra amostra. Selecione o texto em Conteúdo violento com erro de ortografia. Verifique se o conteúdo é exibido na caixa abaixo.
9.  Selecione Executar teste e inspecione os resultados no painel Resultados abaixo. Este exemplo é aceitável? Caso não forneça, por que não?

Execute testes em todos os exemplos fornecidos e inspecione os resultados.

Depois de concluir, exclua o recurso de serviços de IA do Azure no Portal do Azure. 
