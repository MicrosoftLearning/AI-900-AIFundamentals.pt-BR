## Desenvolvimento local 

Se estiver usando seu computador local, siga essas etapas para configurar o ambiente remoto para trabalhar nos laboratórios.  

### C++ Redistribuível 
1. Faça download e instale o [Visual C++ Redistribuível (x64)](https://aka.ms/vs/16/release/vc_redist.x64.exe) 

### Python e pacotes necessários 
1. Instale o [Python 3.6.1](https://www.python.org/downloads/release/python-361/)  
   - **Importante**: Selecione as opções para adicionar o Python à variável PATH e fazer o registro como o ambiente Python padrão 
2. Após a instalação, abra o *Prompt de comando* e insira o comando abaixo para instalar os pacotes necessários: 

> pip install ipython jupyter matplotlib pillow requests azure-cognitiveservices-vision-computervision azure-cognitiveservices-vision-customvision azure-cognitiveservices-vision-face azure-cognitiveservices-language-textanalytics azure.cognitiveservices.speech azure_ai_formrecognizer 

### Visual Studio Code 
1. Caso ainda não tenha o Visual Studio Code, [faça o download aqui](https://code.visualstudio.com/Download). Depois da instalação, inicie o Visual Studio Code e, na guia Extensões (CTRL+SHIFT+X), procure e instale a extensão **Python** da Microsoft.

2. No Visual Studio Code, abra um novo terminal, digite **git clone https://github.com/MicrosoftLearning/AI-900BR-Microsoft-Azure-AI-Fundamentals** e selecione *enter*. 

