
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

1. Estrutura de Arquivos

sua_pasta_projeto/
├── central_de_ferramentas UX e Alth1.4.2.py
├── config.yaml
├── requirements.txt
└── Dockerfile

2. Crie o Arquivo requirements.txt

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

FROM python:3.9-slim-buster
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip &&     pip install -r requirements.txt
COPY . .
RUN mkdir -p /app/user_files
EXPOSE 8501
HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health || exit 1
ENTRYPOINT ["streamlit", "run", "central_de_ferramentas UX e Alth1.4.2.py", "--server.port=8501", "--server.enableCORS=false", "--server.enableXsrfProtection=false"]

4. Construindo a Imagem

docker build -t minha-plataforma-ia .

5. Executando o Contêiner

docker run -p 8501:8501 minha-plataforma-ia

6. Acessando a Aplicação

Acesse: http://localhost:8501

==================================================
🔒 Segurança e Persistência

- Para persistir dados:
docker run -p 8501:8501 -v /caminho/host/user_data:/app/user_files minha-plataforma-ia

- Uso de Variáveis de Ambiente para chaves de API:

Exemplo de função Python:

import os
import streamlit as st

def get_api_key(key_name):
    env_var_name = key_name.upper().replace('KEY', '_KEY').replace('ID', '_ID').replace('GSEARCH_CX', 'GSEARCH_CX')
    env_value = os.getenv(env_var_name)
    if env_value:
        return env_value
    st.warning(f"Chave '{key_name}' não encontrada como variável de ambiente '{env_var_name}'. Verifique config.yaml.")
    return None

Executando com variáveis:

docker run -p 8501:8501 \
-e GEMINI_KEY="sua_chave_gemini" \
-e GCP_PROJECT_ID="seu_projeto_gcp" \
-e GCP_LOCATION="us-central1" \
-e GSEARCH_KEY="sua_chave_gsearch" \
-e GSEARCH_CX="seu_cx_gsearch" \
minha-plataforma-ia

==================================================
📣 Contribuição

Sugestões e melhorias são bem-vindas! Esta plataforma é feita para evoluir com a comunidade.

==================================================
🧠 Licença

Distribuído sob a licença MIT.
