## Bootcamp Bradesco - GenAI, Dados & Cyber.

<img width="106" height="120" alt="bradesco-bootcamp005" src="https://github.com/user-attachments/assets/f3383c92-c76e-40ef-8842-8a7bd0116102" />

---

# 🏦 GenAI Banking Insights Engine: Desafio Criativo - Bootcamp Bradesco

![GenAI](https://img.shields.io/badge/Domain-GenAI%20%26%20Data-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Compliance](https://img.shields.io/badge/LGPD-Compliant-green?style=for-the-badge)

## 1. Problema de Negócio

Instituições financeiras de grande porte recebem diariamente um volume alto de feedbacks de clientes distribuídos em canais heterogêneos — app mobile, Pix, cartão de crédito, chat — em formato de texto livre e desestruturado. Sem um processo analítico padronizado, esse conteúdo permanece subaproveitado: reclamações críticas de infraestrutura (ex.: falha no Pix) competem por atenção com atritos pontuais de UX, e times de CX e Produto não têm um critério objetivo para decidir o que entra no backlog do próximo trimestre.

O problema central que este projeto resolve é: como transformar feedback textual não estruturado em decisões de priorização de backlog, de forma auditável, sem alucinação e sem expor dados sensíveis de clientes.

## 2. Contexto

O projeto foi desenvolvido como Desafio Criativo do **Bootcamp Bradesco - GenAI, Dados & Cyber Security**, em parceria com a DIO. O cenário simulado é o de uma instituição financeira que já registra feedbacks de clientes em base estruturada (`id_feedback`, `canal`, `produto`, `nota_satisfacao`, `texto_feedback`), mas não possui pipeline analítico para extrair sinal acionável desses dados — cada área lê os comentários manualmente, sem padronização de tema, sentimento ou urgência.

Os canais e produtos cobertos pela amostra são: aplicativo iOS/Android, transações Pix, cartão de crédito (limite/fatura) e atendimento via chatbot ou humano.

## 3. Baseline

Hoje, a leitura de feedback é manual e não estruturada: cada área (CX, Produto, Engenharia) interpreta os comentários por conta própria, sem categorização de tema ou urgência, sem correlação sistemática entre nota de satisfação e causa relatada, e sem um critério comum para decidir o que é "quick win" e o que é melhoria estrutural. Essa ausência de padronização é o que a pipeline de prompt engineering deste projeto substitui.

## 4. Premissas

- O status declarado em `nota_satisfacao` (1 a 5) é tratado como proxy oficial de satisfação do cliente.
- A base de dados fornecida (`raw_customer_feedbacks.json`) é sintética/mockada, representando o formato real de um banco de dados de CX.
- Um problema relatado por apenas um cliente na amostra não é generalizado como falha sistêmica — a IA é instruída a não inferir causalidade além do que os dados sustentam.
- Qualquer PII (CPF, cartão, telefone, nome completo) identificada no texto livre deve ser mascarada antes ou durante o processamento, nunca enviada em claro ao modelo.

## 5. Estratégia da Solução

A solução foi construída em três etapas de engenharia de prompt, documentadas individualmente em `docs/`:

1. **Definição de Intenção** (`passo_1_intencao.md`): alinhamento sobre o que será analisado, quem consome o resultado (times de CX e Product Owners) e qual decisão o output apoia (priorização de backlog e redução de churn).
2. **Contexto e Guardrails** (`passo_2_contexto_restricoes.md`): definição do schema de dados, critérios de classificação (tema, sentimento, urgência, evidência) e restrições de grounding e privacidade.
3. **Prompt Final Production-Ready** (`passo_3_prompt_final.md`): consolidação em um master prompt estruturado, testável em qualquer LLM (ChatGPT, Claude, Gemini, Azure OpenAI), com formato de saída fixo (resumo executivo, tabela de análise, top 3 prioridades).

Ferramentas: LLM via prompt engineering (model-agnostic), JSON como formato de ingestão, Markdown como formato de documentação e saída.

## 6. Arquitetura

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
[ Relatório Executivo + Matriz de Priorização ]
```

### Estrutura do Repositório

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

- `docs/`: construção em 3 passos, prompt refinado e diretrizes de governança.
- `data/`: dataset sintético representando interações via app, Pix, cartão e chat.
- `prompts/`: arquivos prontos para produção em qualquer LLM.
- `examples/`: exemplo exato da resposta esperada.

## 7. Decisões Técnicas e Trade-offs

- **Prompt engineering em vez de fine-tuning**: optei por uma solução baseada inteiramente em engenharia de prompt (system prompt + prompt de análise), sem treinar ou ajustar modelo. Para o escopo do desafio — validar viabilidade analítica com dado sintético — essa abordagem entrega resultado equivalente com custo e complexidade de implementação muito menores. O trade-off é menor controle fino sobre o comportamento do modelo em produção real, o que exigiria avaliação adicional de robustez a prompt injection e drift entre provedores de LLM.
- **Grounding estrito via negative prompting em vez de RAG**: como a base é pequena e enviada integralmente no prompt, optei por restringir alucinação via instrução explícita ("não invente causas não presentes nos dados") em vez de montar uma pipeline de RAG. Essa escolha é adequada ao volume desta amostra, mas não escalaria para uma base de feedbacks em produção — nesse cenário, RAG ou pré-processamento com classificação supervisionada seriam necessários.
- **Mascaramento de PII declarativo, não determinístico**: a instrução de mascarar PII (`[DADO_PROTEGIDO]`) está no prompt, não em uma camada de sanitização de código antes do envio. Essa é a limitação mais relevante de segurança do projeto: em um ambiente bancário real, a anonimização não pode depender apenas de o modelo obedecer a instrução — precisa haver uma camada determinística (regex/NER) antes dos dados chegarem à LLM.
- **Model-agnostic em vez de vínculo a um provedor**: os prompts foram escritos para funcionar em ChatGPT, Claude, Gemini ou Azure OpenAI, evitando lock-in de fornecedor, ao custo de não explorar recursos proprietários (ex.: function calling nativo) que poderiam simplificar a extração estruturada.

## 8. Resultados

Com a base sintética de 4 feedbacks (`FB-001` a `FB-004`), a pipeline de prompt produziu, de forma consistente e auditável:

- Classificação individual por tema, sentimento, urgência e produto — por exemplo, a instabilidade do Pix (`FB-001`) e a queda de conexão no atendimento humano (`FB-004`) foram corretamente marcadas como urgência **Crítica**, enquanto a nova interface de Investimentos (`FB-003`) foi classificada como sentimento **Positivo**.
- Evidências textuais extraídas diretamente da fonte, sem paráfrase que alterasse o sentido original — o que preserva rastreabilidade para auditoria.
- Um top 3 de prioridades imediatas coerente com a urgência classificada: erro de infraestrutura no Pix, ajuste de timeout/transbordo no chat e re-treinamento de NLU do chatbot para segunda via de fatura.

O output completo de referência está em `examples/expected_ai_output.md`.

## 9. Impacto / Business Performance

Em uma base real de feedbacks (tipicamente milhares por mês em um banco de grande porte), esse tipo de pipeline substitui a leitura manual linha a linha por um relatório executivo gerado em minutos, com critério padronizado de priorização. O ganho de negócio não está no dado sintético em si, mas na redução do tempo entre "cliente relata um problema" e "time técnico prioriza a correção" — especialmente relevante em falhas críticas como indisponibilidade do Pix, onde cada hora de atraso na priorização tem custo direto em churn e reclamação regulatória.

A quantificação financeira exata (custo por hora de indisponibilidade, custo de churn por ponto de NPS) depende de dados reais de produção que não fazem parte do escopo deste desafio — fica registrado como limitação e como próximo passo natural de evolução do projeto.

## 🛡️ Governança e Segurança de Dados (Cyber & LGPD)

Documentado em detalhe em `docs/governanca_e_seguranca_dados.md`. Em síntese:

- **Zero PII Exposure**: nenhum CPF, CNPJ, número de cartão (PAN), nome completo, e-mail ou IP deve ser enviado à LLM sem mascaramento.
- **Hallucination Containment**: negative prompting explícito impede que a IA reporte como "falha sistêmica" um problema relatado por um único cliente.
- **Segurança na ingestão**: sanitização do prompt contra *prompt injection* e premissa de retenção zero de dados por parte do fornecedor da LLM (Zero Data Retention SLA).

## 🚀 Como Executar este Projeto

1. Acesse a pasta `prompts/`.
2. Copie o conteúdo de `system_prompt.txt` e configure como instrução de sistema na sua LLM.
3. Copie o conteúdo de `analysis_prompt.txt`, inserindo os dados de `data/raw_customer_feedbacks.json` no ponto indicado.
4. Execute o prompt para obter a análise padronizada de CX.

## 10. Próximos Passos

- Substituir o mascaramento de PII declarativo (via prompt) por uma camada determinística de sanitização (regex/NER) antes do dado chegar à LLM.
- Validar a pipeline com volume real de feedbacks e medir tempo de processamento e taxa de acerto de classificação contra rótulos humanos.
- Quantificar o Business Performance com dados reais de custo de churn e SLA de indisponibilidade.
- Avaliar RAG para bases de feedback maiores, onde o envio integral do dataset no prompt deixa de ser viável.
- Integrar o output estruturado (tabela de análise) diretamente a uma ferramenta de backlog (Jira/Azure DevOps) via API.

---


**Autor:** Sérgio Santos — Cientista de Dados | Ambientes Críticos e Governança de Dados

[![Portfólio Sérgio Santos](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)
[![LinkedIn Sérgio Santos](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)
