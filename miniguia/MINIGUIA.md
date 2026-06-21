# Miniguia de estudo: segurança em LLMs

Versão independente do miniguia. Reúne num só lugar o essencial para revisar segurança em LLMs.

---

## Mapa do tema

```
                         SEGURANÇA EM LLMs
                                │
        ┌───────────────┬───────┴───────┬────────────────┐
        ▼               ▼               ▼                ▼
    RISCOS          ATAQUES        FRAMEWORKS         DEFESA
  (OWASP Top 10)  (Prompt Inj.)  (NIST/MITRE)      (SAIF + mitigação)
```

---

## 1. Os dez riscos (OWASP Top 10 for LLMs, 2025)

| Código | Risco | Resumo |
|--------|-------|--------|
| LLM01 | Prompt Injection | Comandos alteram o comportamento do modelo. |
| LLM02 | Sensitive Information Disclosure | Vazamento de dados confidenciais. |
| LLM03 | Supply Chain | Componentes, modelos ou datasets de terceiros comprometidos. |
| LLM04 | Data and Model Poisoning | Manipulação dos dados de treino. |
| LLM05 | Improper Output Handling | Saída do modelo usada sem validação. |
| LLM06 | Excessive Agency | Autonomia ou permissões demais. |
| LLM07 | System Prompt Leakage | Vazamento de instruções internas. |
| LLM08 | Vector and Embedding Weaknesses | Fraquezas em bancos vetoriais e RAG. |
| LLM09 | Misinformation | Geração de informação falsa. |
| LLM10 | Unbounded Consumption | Consumo ilimitado, levando a DoS ou custos. |

---

## 2. O ataque central: prompt injection

| | Direta | Indireta |
|---|--------|----------|
| Quem envia | O próprio usuário, no chat | O atacante, por uma fonte externa |
| Canal | Interface direta do LLM | Site, documento ou e-mail que o LLM lê |
| Exemplo | Jailbreaking | Comando oculto num site resumido pelo Bing Chat (GPT-4) |

> A indireta é mais perigosa porque permite exploração remota, sem o atacante tocar no sistema (Greshake et al., 2023).

---

## 3. Os frameworks

NIST AI RMF: gestão de risco em quatro funções. GOVERN cuida da cultura e das políticas, MAP do contexto e dos riscos, MEASURE das métricas, e MANAGE do tratamento.

MITRE ATLAS: matriz de táticas e técnicas usadas por atacantes contra IA, no estilo do ATT&CK e baseada em casos reais.

Google SAIF: framework defensivo com riscos inerentes e controles, focado em operar IA com segurança em escala.

> Para lembrar rápido: o ATLAS é sobre ataque (o que monitorar), o SAIF é sobre defesa (como proteger) e o NIST é sobre governança (como gerir).

---

## 4. Mitigação essencial (LLM01 Prompt Injection)

- [ ] Privilégio mínimo: só as permissões estritamente necessárias.
- [ ] Human-in-the-loop: aprovação humana para ações críticas.
- [ ] Segregação de contexto: separar as instruções do sistema do conteúdo externo.
- [ ] Sanitização de saída: tratar a resposta do LLM como dado não confiável.
- [ ] Monitoramento: registrar e detectar tentativas de injeção.

---

## 5. Glossário relâmpago

| Termo | Em uma frase |
|-------|--------------|
| Prompt injection | Comando malicioso que sequestra o comportamento do modelo. |
| Jailbreaking | Burlar as salvaguardas do LLM via prompt. |
| RAG | Recuperar dados externos para enriquecer respostas, o que também abre um vetor de ataque. |
| Embedding | Representação vetorial do significado dos dados. |
| Data poisoning | Envenenar os dados de treino. |
| Excessive agency | Dar autonomia demais à IA. |
| Supply chain | Risco vindo de terceiros, como modelos e datasets. |
| Red teaming | Simular ataques reais para achar falhas. |
| NIST AI RMF | Framework de gestão de risco de IA. |
| MITRE ATLAS | Matriz de ameaças contra IA. |
| SAIF | Framework de segurança de IA do Google. |

---

## 6. Checklist de domínio do tema

- [ ] Sei explicar os dez riscos do OWASP de cabeça.
- [ ] Diferencio injeção direta de indireta com um exemplo.
- [ ] Conheço as quatro funções do NIST AI RMF.
- [ ] Sei o papel de cada um: ATLAS, SAIF e NIST.
- [ ] Consigo listar cinco mitigações para prompt injection.

> Quando todos os itens estiverem marcados, você domina o essencial de segurança em LLMs.
