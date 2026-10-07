# Projeto: Base de Conhecimento e Wiki Inteligente na AWS

**Autor:** Gabriel (gabrielgomes-TI)

## 📌 Problema Resolvido
Empresas lidam diariamente com acervos descentralizados contendo informações valiosas presas em múltiplos formatos (PDFs, documentos escaneados e planilhas/CSVs). Este projeto propõe uma arquitetura na AWS capaz de ingerir, tratar, indexar e disponibilizar esses dados para consultas em linguagem natural (RAG) com citação de fontes.

---

## 🏗️ Arquitetura da Solução

1. **Ingestão (S3 Bucket):** Recebimento dos arquivos na pasta bruta.
2. **Extração Diferenciada:**
   - **PDFs e Imagens (Escaneadas/Manuscritas):** Processados via **Amazon Textract** (OCR avançado).
   - **CSVs:** Processados via **AWS Lambda** para conversão de linhas tabulares em documentos de texto.
3. **Enriquecimento:** Uso do **Amazon Comprehend** para extração de entidades e metadados.
4. **Indexação e RAG:** **Amazon Kendra** faz a indexação semântica e se conecta ao **Amazon Bedrock** para responder dúvidas citando o arquivo original.

---

## 🛠️ Serviços Utilizados e Justificativas

| Serviço AWS | Função no Projeto | Justificativa de Escolha |
| :--- | :--- | :--- |
| **Amazon S3** | Armazenamento de objetos | Escalável, seguro e ideal para atuar como repositório dos dados brutos e processados. |
| **Amazon Textract** | OCR e extração | Consegue ler nativamente tanto documentos digitais quanto manuscritos sem necessidade de treinar modelos do zero. |
| **AWS Lambda** | Processamento Serverless | Executa o código de transformação do CSV e orquestração de chamadas sem necessidade de gerenciar servidores. |
| **Amazon Comprehend** | NLP / Extração de Entidades | Identifica dados sensíveis (PII) e categoriza automaticamente o conteúdo para os metadados. |
| **Amazon Kendra** | Busca Semântica | Mecanismo de busca corporativo pré-treinado com suporte nativo a RAG e citação de fontes. |
| **Amazon Bedrock** | IA Generativa | Permite usar LLMs de ponta para gerar respostas amigáveis e precisas ao usuário. |

---

## 📑 Tratamento por Formato de Arquivo

- **Ata em PDF:** Processada com suporte a layout do Textract para preservar títulos e tópicos da reunião.
- **Folha Digitalizada com Manuscrito:** Utiliza a funcionalidade de OCR de caligrafia do Textract para converter imagens em texto legível.
- **Exportação CSV:** Um script Python no Lambda lê as dezenas de colunas e converte cada linha em um sumário executivo em formato de texto.

---

## 💡 Aprendizados
Com este desafio, foi possível compreender a importância de não aplicar uma solução única para dados heterogêneos. A escolha correta do serviço de extração para cada tipo de mídia (estruturada, não estruturada e física/digitalizada) é o fator crítico para o sucesso de uma arquitetura baseada em RAG e IA Generativa.
