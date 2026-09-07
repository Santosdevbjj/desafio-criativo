### 5. `docs/governanca_e_seguranca_dados.md`


# 🛡️ Governança de Dados, Cyber Security e LGPD

No setor bancário, o processamento de textos por Large Language Models (LLMs) requer camadas de controle para evitar vazamento de dados confidenciais e violação da LGPD (Lei Geral de Proteção de Dados - Lei nº 13.709/2018).

## 1. Proteção de Dados Pessoais (PII)
Antes de enviar os dados para a LLM, a pipeline deve garantir a remoção ou ofuscação de:
* CPF / CNPJ
* Números de Cartão de Crédito (PAN)
* Nomes completos e dados bancários (Agência/Conta)
* Endereços de IP e e-mails

## 2. Mitigação de Alucinações (Data Grounding)
A IA é instruída através de restrições negativas (*Negative Prompting*) para impedir que infira padrões inexistentes na amostra. Se um problema é citado por apenas 1 cliente, o modelo não deve reportá-lo como "falha sistêmica generalizada".

## 3. Segurança na Ingestão
* Sanitização do prompt contra ataques de *Prompt Injection*.
* Retenção zero de dados pelo fornecedor da LLM (Zero Data Retention SLA).
