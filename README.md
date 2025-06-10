# Chatbot LLM para Análise de Dados em Bucket da AWS S3

Este projeto implementa uma interface inteligente para análise de dados a partir de um dataset armazenado na AWS S3. Utilizando um modelo de linguagem da OpenAI, o sistema interpreta perguntas em linguagem natural, converte-as em queries SQL, executa as consultas localmente com `pandas` + `sqlite3`, gera insights automáticos e exibe gráficos interativos via Matplotlib.

## Tecnologias Utilizadas:

- **OpenAI API** – para geração de SQL e insights a partir de prompts do usuário.
- **Boto3** – integração com o AWS S3 para leitura do dataset.
- **Pandas** – tratamento e manipulação de dados tabulares.
- **SQLite3 (em memória)** – execução local de consultas SQL.
- **Matplotlib** – geração automática de gráficos com base nos dados retornados.
- **Gradio** – interface web simples e interativa.

##  Estrutura de Dados:

O projeto opera sobre um dataset hospedado na AWS S3 com as seguintes colunas:

- `REF_DATE` – Data de referência
- `TARGET` – Indicador de inadimplência (0 = adimplente, 1 = inadimplente)
- `VAR2` – Sexo (M/F)
- `IDADE` – Idade do indivíduo
- `VAR4` – Flag de óbito
- `VAR5` – Unidade Federativa (UF)
- `VAR8` – Classe social estimada

O nome da tabela considerada nas consultas é `"s3object"`.

## Funcionamento Geral:

1. O usuário digita um prompt, como por exemplo:  
   `"Qual a taxa de inadimplência por UF?"`

2. O modelo da OpenAI interpreta a pergunta e gera a query SQL adequada.

3. O script detecta e baixa automaticamente o arquivo `train.csv.gz` (ou `train.csv`, caso não comprimido) no bucket `bucket-desafio-ds`.

4. A consulta é executada localmente e retorna um DataFrame.

5. O modelo da OpenAI gera insights automáticos com base nos dados retornados.

6. Um gráfico é gerado automaticamente com base nos dados analisados.

7. O resultado da consulta pode ser baixado como `.csv`.

## Observações Importantes:

- **Ajuste de Amostragem para Insights**  
  A quantidade de linhas do DataFrame analisadas para geração de insights pode ser ajustada facilmente no trecho `df.head(1000)` dentro da função `generate_insights`.

- **Credenciais**  
  É necessário fornecer as credenciais da AWS e da OpenAI no terceiro bloco do notebook para funcionamento adequado. Arquivo .txt com credenciais enviado para Nayara Batista do RH.

- **Melhorias**  
  É possível otimizar o tempo de resposta realizando a consulta via query diretamente no AWS S3 ou utilizando o AWS Athena para uma solução mais escalável, mas por gap técnico não consegui realizar neste formato.

- **Execução Recomendada**  
  O projeto foi construído e testado no ambiente do Google Colab. A instalação das bibliotecas pode ser feita diretamente via:

  !pip install boto3 pandas openai gradio matplotlib --quiet

