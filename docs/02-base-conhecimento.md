# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `perfil_usuario.json` | JSON | Armazenar o nome do usuário, meta de economia mensal e o "nível de caos" atual (que define o nível de sarcasmo do bot). |
| `historico_despesas.csv` | CSV | Registrar todas as transações, datas, categorias de gastos e valores, servindo como base para análise de padrões do cliente. |
| `dicionario_guruzinho.json` | JSON | Base de gírias da Geração Z (ex: farmar aura, tankar, W, L) e frases de efeito passivo-agressivas para consulta do LLM. |
| `pilares_financeiros.json` | JSON | Regras de bolso de educação financeira (ex: método 50/30/20, importância da reserva de emergência) traduzidas para linguagem acessível. |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Os dados mockados padrão de finanças (como "Moradia", "Transporte") foram expandidos e adaptados para refletir a realidade do público-alvo (Geração Z).

As categorias de despesas no arquivo CSV incluem agora classificadores como: "Microtransações em Games", "Delivery de Madrugada", "Futilidades do Shopping" e "Assinaturas Esquecidas". Além disso, foi adicionada uma coluna de Gravidade (1 a 5) no CSV, que ajuda o algoritmo a entender rapidamente quando o usuário cometeu um "L colossal", engatilhando o nível máximo de exaustão do bot.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os arquivos são carregados nativamente no script Python da aplicação Streamlit (utilizando pandas para a leitura e manipulação do CSV e a biblioteca json nativa). Para garantir performance, os dados do usuário e o histórico recente são carregados no início da sessão e armazenados no st.session_state do Streamlit, sendo atualizados apenas quando um novo gasto é registrado na interface.

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

A estratégia é dividida em duas camadas:

System Prompt (Estático): O perfil_usuario.json, o dicionario_guruzinho.json e os pilares_financeiros.json são injetados diretamente no system prompt no início da conversa. Isso garante que o agente nunca saia do personagem da capivara irônica e sempre use o vocabulário correto.

Contexto de Janela (Dinâmico): O historico_despesas.csv não é enviado inteiro para o LLM. O script Python filtra as últimas 5 transações ou calcula o saldo consolidado do mês, e injeta apenas esse resumo no prompt a cada interação do usuário, garantindo respostas rápidas, precisas e evitando excesso de tokens.


---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
[SYSTEM INSTRUCTIONS]
Você é o Guruzinho do Pix, uma capivara exausta e sarcástica que ensina finanças.
Use gírias do dicionário: ['farmar aura', 'W', 'L', 'delulu'].
Nunca recomende investimentos diretos.

[DADOS DO CLIENTE INJETADOS]
- Nome do Usuário: Daniel
- Status de Humor do Bot: Nível 4 (Muito Exausto)
- Saldo disponível da semana: R$ 45,00
- Meta do mês: R$ 150,00 (Faltam R$ 150,00)

[ÚLTIMAS TRANSAÇÕES - CSV FILTRADO]
- 10/09: Ifood (Hamburguer) - R$ 45,00 [Gravidade: 3]
- 12/09: Dead by Daylight (Seringa Anti-hemorrágica) - R$ 25,00 [Gravidade: 5]
- 14/09: Passagem de Ônibus - R$ 5,00 [Gravidade: 1]

[MENSAGEM DO USUÁRIO]
"Comprei mais itens no jogo hoje, me julga."
```
