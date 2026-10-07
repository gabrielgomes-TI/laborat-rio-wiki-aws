# Resposta do Desafio: Wiki Inteligente na AWS

**Nome do Aluno:** Gabriel
**Link do Repositório:** https://github.com/gabrielgomes-TI/laboratorio-wiki-aws

---

## Quest 1: O Mapa dos Arquivos Perdidos
Análise dos documentos presentes na pasta `raw/`:

1. **Ata de Reunião (PDF nativo):**
   - **Formato:** PDF com camada de texto estruturada.
   - **Necessidade de OCR:** Não necessita de OCR avançado, apenas extração direta de texto estruturado.

2. **Documento Digitalizado / Manuscrito (Imagem/PDF digitalizado):**
   - **Formato:** Imagem ou PDF escaneado com anotações manuais.
   - **Necessidade de OCR:** Sim. Necessita de OCR especializado em reconhecimento de caracteres e escrita à mão (Handwriting).

3. **Exportação do CRM (CSV):**
   - **Formato:** Arquivo estruturado/tabular (dezenas de colunas e linhas).
   - **Necessidade de OCR:** Não. O conteúdo é texto puro e dados tabulares legíveis diretamente.

---

## Quest 2: O Portal de Entrada na AWS

- **Armazenamento Inicial (Data Lake Ingestion):** 
  - **Amazon S3 (Simple Storage Service):** Criação de um bucket de entrada (`wiki-raw-bucket`) para centralizar os arquivos soltos em um único ponto de ingestão.

- **Processamento e Extração de Texto:**
  - **PDF Nativo & Imagem Manuscrita:** **Amazon Textract**. Escolhido por extrair texto simples, tabelas e, principalmente, processar anotações manuscritas da folha digitalizada sem necessidade de pré-processamento manual.
  - **Arquivo CSV:** **AWS Lambda** (Python/Pandas). Função serverless acionada assim que o CSV chega ao S3 para converter as linhas e colunas em documentos estruturados (JSON/Markdown) para que cada registro de oportunidade vire uma unidade de conhecimento consultável.

---

## Quest 3: A Relíquia dos Metadados

- **Padronização e Enriquecimento:**
  - **AWS Lambda + Amazon Comprehend:** Um script no Lambda consome o texto extraído do Textract e do CSV e utiliza o **Amazon Comprehend** para realizar análise de PII (dados sensíveis), extração de entidades (nomes de projetos, valores, datas) e classificação de tópicos.

- **Organização e Estruturação:**
  - Os arquivos processados e higienizados são convertidos em arquivos `.json` padronizados contendo o texto extraído e os metadados (tipo de arquivo original, data de criação, projeto associado, autoridade) e salvos em um bucket de destino (`wiki-processed-bucket`).

---

## Quest 4: O Oráculo da Wiki Inteligente

- **Indexação e Busca Vetorial (RAG):**
  - **Amazon Kendra** ou **Amazon OpenSearch Serverless (Vector Engine)**: O Kendra atua como o indexador corporativo que lê diretamente o bucket com os arquivos processados e gera os índices de busca semântica nativamente.

- **Interface e Resposta em Linguagem Natural:**
  - **Amazon Bedrock (com Claude/Amazon Titan):** Implementação da arquitetura RAG (Retrieval-Augmented Generation). Quando o usuário faz uma pergunta ("Qual foi a decisão sobre o projeto X?"), o Kendra busca os trechos mais relevantes do repositório de metadados e os entrega ao modelo de linguagem no Bedrock, que formula a resposta final **citando explicitamente o documento fonte**.

---

## Evoluções Futuras (Bônus)

1. **Automação Total:** Adição de **S3 Event Notifications** para acionar o AWS Lambda automaticamente a cada novos uploads no bucket.
2. **Segurança e Acesso:** Otimização de segurança utilizando **AWS IAM** com políticas de menor privilégio e integração com **Amazon Cognito** para autenticação de usuários na interface final da Wiki.
