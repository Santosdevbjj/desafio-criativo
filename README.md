## Bootcamp Bradesco - GenAI, Dados & Cyber.



<img width="106" height="120" alt="bradesco-bootcamp005" src="https://github.com/user-attachments/assets/f3383c92-c76e-40ef-8842-8a7bd0116102" />

---

# 🏦 GenAI Banking Insights Engine: Desafio Criativo - Bootcamp Bradesco

![GenAI](https://img.shields.io/badge/Domain-GenAI%20%26%20Data-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Compliance](https://img.shields.io/badge/LGPD-Compliant-green?style=for-the-badge)

## 📌 Visão Geral do Projeto

Este repositório contém a solução completa para o **Desafio Criativo: Extraindo Insights do Feedback de Clientes Bancários**, integrante do **Bootcamp Bradesco - GenAI, Dados & Cyber Security** em parceria com a **DIO**.

O objetivo principal é projetar, documentar e estruturar uma pipeline de engenharia de prompt para Inteligência Artificial Generativa (LLMs), capaz de ingerir feedbacks desestruturados de clientes bancários, anonimizar dados sensíveis e gerar relatórios acionáveis de tomada de decisão para equipes de **Customer Experience (CX)** e **Engenharia de Produtos Digitais**.

---

## 🏗️ Arquitetura da Solução

```text
[ Feedbacks Brutos (JSON) ] 
            │
            ▼
[ Anonymization / LGPD Guardrail ] 
            │
            ▼
[ System Prompt + Few-Shot Prompting ] 
            │
            ▼
[ Large Language Model (LLM) ] 
            │
            ▼
[ Relatório Executivo + Matrix de Priorização ]

```

---

## 📂 Estrutura do Repositório

```
desafio-criativo/
├── README.md
├── docs/
│   ├── passo_1_intencao.md
│   ├── passo_2_contexto_restricoes.md
│   ├── passo_3_prompt_final.md
│   └── governanca_e_seguranca_dados.md
├── data/
│   └── raw_customer_feedbacks.json
├── prompts/
│   ├── system_prompt.txt
│   └── analysis_prompt.txt
└── examples/
    └── expected_ai_output.md
```




docs/: Documentação detalhada da construção em 3 passos, prompt refinado e diretrizes de governança de dados.

data/: Dataset sintético mockado representando interações de clientes via aplicativo, Pix, cartão e chat.

prompts/: Arquivos de prompt prontos para produção em LLMs (ChatGPT, Claude, Gemini, Azure OpenAI).

examples/: Exemplo exato da resposta esperada gerada pela IA.

🚀 Como Executar este Projeto



Acesse a pasta prompts/.

Copie o conteúdo de system_prompt.txt e configure a instrução de sistema na sua LLM.

Copie o conteúdo de analysis_prompt.txt adicionando os dados de data/raw_customer_feedbacks.json.

Execute o prompt para obter a análise padronizada de CX.

🛡️ Governança e Segurança de Dados (Cyber & LGPD)
Em conformidade com as diretrizes de cibersegurança e proteção de dados bancários:

Zero PII Exposure: Nenhum dado PII (CPF, número de conta, nome completo, telefone) é enviado ao modelo.

Hallucination Containment: Instruções estritas de restrição (Grounding) garantem que a IA não invente métricas não presentes na base.




---

**Autor:** Sérgio Santos — Cientista de Dados | Ambientes Críticos e Governança de Dados

[![Portfólio Sérgio Santos](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)
[![LinkedIn Sérgio Santos](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)


