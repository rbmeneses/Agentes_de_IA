
🚀 Minha Plataforma de IA – O Canivete Suíço da Produtividade e Inovação

Bem-vindo(a) à Minha Plataforma de IA, uma suíte completa e modular desenvolvida em Python com Streamlit, projetada para elevar sua produtividade, criatividade e inovação.

Integrada com Google Gemini e Vertex AI, esta plataforma oferece um ecossistema robusto para criação de conteúdo, automação, desenvolvimento, visualização de dados e muito mais.

==================================================
✨ Principais Funcionalidades

- 🧠 Gerador de Exercícios
  Crie treinamentos personalizados com apoio de IA para capacitação interna e educação corporativa.

- 🪄 Otimizador de Prompts
  Aperfeiçoe suas interações com modelos de linguagem, gerando prompts mais eficazes, criativos e específicos.

- 🖼️ Análise Visual de Imagens
  Extraia descrições, gere código ou transforme imagens em insights acionáveis com suporte à visão computacional.

- ⚙️ Criador de Aplicativos
  Prototipe rapidamente soluções internas usando IA e código gerado automaticamente a partir de descrições ou imagens.

- 🎨 Fábrica de Spritesheets 2D
  Ideal para quem trabalha com jogos, design ou experiências visuais. Gere spritesheets com qualidade e agilidade.

- 📊 Análise de Logs
  Ferramenta avançada para equipes de TI que desejam extrair padrões, detectar erros e automatizar diagnósticos.

- 🧠 Espelho da Mente
  Visualize ideias e conceitos abstratos em formato artístico, explorando criatividade de maneira inusitada.

- 🔎 Buscador de Vagas
  Busque oportunidades usando Google Dorks e APIs avançadas para automatizar e refinar processos de recrutamento.

==================================================
🐳 Executando com Docker
Para facilitar a execução e garantir um ambiente consistente, a "Minha Plataforma de IA" está pronta para ser executada via Docker.

1. Estrutura de Arquivos
Certifique-se de que seus arquivos estejam organizados da seguinte forma:

sua_pasta_projeto/
├── central_de_ferramentas UX e Alth1.4.2.py
├── config.yaml
├── requirements.txt
└── Dockerfile
2. Crie o Arquivo requirements.txt
Este arquivo lista todas as dependências Python da sua aplicação. Crie-o na raiz do seu projeto com o seguinte conteúdo:

streamlit
requests
Pillow
python-docx
reportlab
google-cloud-aiplatform
google-api-python-client
pyyaml
streamlit-authenticator
3. Crie o Arquivo Dockerfile
Na raiz do seu projeto, crie um arquivo chamado Dockerfile (sem extensão) com o seguinte conteúdo:

Dockerfile

 Usa uma imagem base oficial do Python.
 A versão '3.9-slim-buster' é uma boa escolha pois é leve.
FROM python:3.9-slim-buster

 Define o diretório de trabalho dentro do contêiner.
 Todos os comandos subsequentes serão executados a partir deste diretório.
WORKDIR /app

 Copia o arquivo requirements.txt para o diretório de trabalho no contêiner.
COPY requirements.txt .

 Instala as dependências Python listadas no requirements.txt.
 O '--no-cache-dir' e '--upgrade pip' são boas práticas para builds otimizados.
RUN pip install --no-cache-dir --upgrade pip && \
    pip install -r requirements.txt

 Copia todos os outros arquivos do diretório atual (no host) para o diretório de trabalho (/app no contêiner).
 Isso inclui seu script Python principal, config.yaml, etc.
COPY . .

 Cria o diretório 'user_files' dentro do contêiner.
 É importante notar que, por padrão, o sistema de arquivos do Docker é efêmero.
 Para persistência real, você precisaria de volumes Docker ou armazenamento em nuvem.
RUN mkdir -p /app/user_files

 Expõe a porta que o Streamlit usa (padrão é 8501).
 Isso informa ao Docker que o contêiner escuta nesta porta.
EXPOSE 8501

 Define um comando de healthcheck.
 O Docker irá verificar se a aplicação está respondendo nesta URL.
HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health || exit 1

 Define o comando que será executado quando o contêiner for iniciado.
 - 'streamlit run' executa sua aplicação.
 - '--server.port=8501' garante que o Streamlit use a porta exposta.
 - '--server.enableCORS=false' e '--server.enableXsrfProtection=false' são para facilitar o acesso em alguns ambientes.
   Para produção, você pode querer reavaliar essas configurações ou usar um proxy reverso.
ENTRYPOINT ["streamlit", "run", "central_de_ferramentas UX e Alth1.4.2.py", "--server.port=8501", "--server.enableCORS=false", "--server.enableXsrfProtection=false"]
4. Construindo a Imagem Docker
Navegue até a pasta sua_pasta_projeto no seu terminal e execute o seguinte comando:

Bash

docker build -t minha-plataforma-ia .
Este processo pode levar alguns minutos na primeira vez, pois o Docker precisa baixar a imagem base do Python e instalar todas as dependências.

5. Executando o Contêiner Docker
Após a imagem ser construída com sucesso, você pode executar sua aplicação em um contêiner com o seguinte comando:

Bash

docker run -p 8501:8501 minha-plataforma-ia
6. Acessando a Aplicação
Abra seu navegador e vá para http://localhost:8501. Sua aplicação Streamlit deve estar rodando!

🔒 Considerações Adicionais
Persistência de Dados (user_files e config.yaml)
Os dados salvos dentro do contêiner (/app/user_files) serão perdidos se o contêiner for removido. Para persistência, você precisaria usar Volumes Docker.

Exemplo de uso de volume para user_files:

Bash

docker run -p 8501:8501 -v /caminho/no/seu/host/para/user_data:/app/user_files minha-plataforma-ia
Substitua /caminho/no/seu/host/para/user_data por um caminho real no seu sistema. Isso montará uma pasta do seu host dentro do contêiner, garantindo que os arquivos salvem nela e persistam.

Variáveis de Ambiente para Chaves de API
Para segurança, é altamente recomendável que suas chaves de API (Gemini, Google Cloud, Google Search) sejam passadas para o contêiner como variáveis de ambiente, em vez de estarem no config.yaml se você for compartilhar a imagem ou o Dockerfile.

Atualize get_api_key:

Modifique sua função get_api_key para tentar ler de variáveis de ambiente primeiro:

Python

import os
import streamlit as st # Certifique-se de importar streamlit se ainda não o fez

def get_api_key(key_name):
    """Busca a chave de API do perfil do usuário logado de forma segura, preferindo variáveis de ambiente."""
     Adaptação simples para nomes de variáveis de ambiente (ex: GEMINI_KEY, GCP_PROJECT_ID)
    env_var_name = key_name.upper().replace('KEY', '_KEY').replace('ID', '_ID').replace('GSEARCH_CX', 'GSEARCH_CX')
    env_value = os.getenv(env_var_name)
    if env_value:
        return env_value
    try:
         Se não encontrada em env, tenta do config.yaml
         Certifique-se de que 'config' e 'username' estejam acessíveis aqui no contexto de sua aplicação
         (Este trecho assume que 'config' e 'username' estão definidos em seu script principal)
         return config['credentials']['usernames'][username]['api_keys'][key_name]
        st.warning(f"Chave de API '{key_name}' não encontrada como variável de ambiente '{env_var_name}'. Verifique o arquivo config.yaml ou configure a variável de ambiente.")
        return None # Ou levantar uma exceção, dependendo da sua estratégia de erro
    except (KeyError, TypeError):
        st.error(f"Chave de API '{key_name}' não encontrada. Configure-a na página 'Perfil e Configurações' ou como variável de ambiente '{env_var_name}'.")
        return None
Passe as variáveis ao executar o contêiner:

Bash

docker run -p 8501:8501 \
-e GEMINI_KEY="sua_chave_gemini" \
-e GCP_PROJECT_ID="seu_projeto_gcp" \
-e GCP_LOCATION="us-central1" \
-e GSEARCH_KEY="sua_chave_gsearch" \
-e GSEARCH_CX="seu_cx_gsearch" \
minha-plataforma-ia
Substitua sua_chave_gemini, etc., pelas suas chaves reais.
==================================================
📣 Contribuição

Sugestões e melhorias são bem-vindas! Esta plataforma é feita para evoluir com a comunidade.

==================================================
🧠 Licença

Distribuído sob a licença MIT.
