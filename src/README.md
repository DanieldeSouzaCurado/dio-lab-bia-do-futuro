# Código da Aplicação

Esta pasta contém o código do seu agente financeiro.

## Estrutura Sugerida

meu_projeto/
├── src/
│   ├── app.py              # Aplicação principal (Interface do Chatbot em Streamlit)
│   ├── agente.py           # Lógica do agente, montagem do prompt e chamada da API do LLM
│   ├── config.py           # Configurações do sistema e chaves de API
│   └── data_loader.py      # Scripts para leitura e formatação dos arquivos JSON/CSV
├── data/                   # Nossa Base de Conhecimento
│   ├── perfil_usuario.json
│   ├── historico_despesas.csv
│   ├── dicionario_guruzinho.json
│   └── pilares_financeiros.json
├── .env                    # Arquivo (oculto) para armazenar sua API Key com segurança
└── requirements.txt        # Lista de dependências do Python

## Exemplo de requirements.txt

streamlit>=1.32.0
google-generativeai       # Caso use a API do Gemini (ou 'openai' se for usar o GPT)
pandas                    # Essencial para manipular o historico_despesas.csv
python-dotenv             # Para carregar a chave de API do arquivo .env com segurança

## Como Rodar

```bash
# Clone o repositório e navegue até a pasta do projeto:
cd caminho/para/o/meu_projeto

# Crie e configure o arquivo de chaves de API:
# Exemplo de conteúdo do arquivo .env
API_KEY=sua_chave_de_api_aqui

#Instale as dependências:
pip install -r requirements.txt

# Rode a aplicação:
streamlit run src/app.py
```
