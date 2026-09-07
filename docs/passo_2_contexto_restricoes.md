# Passo 2: Contexto, Dados e Guardrails de Segurança

## 📋 Bloco de Contexto e Restrições Preenchido

**Contexto:** Estou trabalhando com feedbacks de clientes bancários referentes ao ecossistema digital (aplicativo iOS/Android, transações Pix, limite/fatura de cartão de crédito e atendimento via ChatBot/humano).

**Dados disponíveis:** A base consiste em um array JSON contendo: `id_feedback`, `data_registro`, `canal`, `produto`, `nota_satisfacao` (1 a 5) e `texto_feedback`.

**Critérios de análise:** A IA deve classificar os feedbacks aplicando:
1. **Tema Principal**: (ex: Instabilidade, UX/UI, Cobrança, Atendimento, Performance).
2. **Sentimento**: (Positivo, Neutro, Negativo).
3. **Nível de Urgência**: (Baixa, Média, Alta, Crítica).
4. **Evidência**: Citação direta do comentário.

**Cuidados e restrições:**
* **Grounding Estrito**: Use estritamente os dados fornecidos. Não invente estatísticas, percentuais ou causas não explicitadas.
* **Privacidade & PII**: Caso identifique qualquer dado pessoal (CPF, cartão, telefone), mascare imediatamente com `[DADO_PROTEGIDO]`.
* **Tratamento de Inconsistência**: Se os dados forem insuficientes ou ambíguos para um diagnóstico, declare a limitação explicitamente.
* **Estilo de Linguagem**: Executiva, direta, focada em métricas de produto (SLA, UX, NPS, Churn).
