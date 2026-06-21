# Curadoria de fontes

Detalhamento das cinco fontes abertas selecionadas para o caderno sobre segurança em LLMs, todas carregadas no NotebookLM.

> Critério: poucas fontes, mas as mais respeitadas do mundo, escolhidas para cobrir ângulos diferentes. Em segurança, a credibilidade da fonte conta muito.

---

## 1. OWASP Top 10 for LLM Applications (2025)

- Instituição: OWASP (Open Worldwide Application Security Project)
- Tipo: página web aberta
- Link: https://genai.owasp.org/llm-top-10/
- Ângulo: taxonomia prática de riscos

O OWASP é o padrão global de segurança de aplicações há mais de vinte anos. O Top 10 para LLMs foi elaborado por uma comunidade internacional de centenas de especialistas e é citado por praticamente toda empresa e fornecedor que trabalha com IA. É o ponto de partida natural para quem estuda segurança em LLMs.

---

## 2. NIST AI Risk Management Framework 1.0

- Instituição: NIST (National Institute of Standards and Technology, governo dos EUA)
- Tipo: PDF oficial (NIST.AI.100-1)
- Link: https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf
- Ângulo: governança e gestão de risco

O NIST define padrões técnicos adotados por governos e empresas no mundo inteiro. O AI RMF é o framework de gestão de risco de IA mais reconhecido, estruturado em quatro funções (GOVERN, MAP, MEASURE, MANAGE). Ele traz a camada de governança que costuma faltar nas abordagens só técnicas.

---

## 3. MITRE ATLAS

- Instituição: MITRE Corporation
- Tipo: base de conhecimento web aberta
- Link: https://atlas.mitre.org/
- Ângulo: inteligência de ameaças

A MITRE mantém o ATT&CK, uma das principais bases de inteligência de ameaças em cibersegurança. O ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems) é a versão dedicada a sistemas de IA: uma matriz com táticas e técnicas baseadas em ataques reais observados contra modelos de ML e LLM.

---

## 4. Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection

- Autores: Kai Greshake et al. (2023)
- Tipo: paper acadêmico (arXiv)
- Link: https://arxiv.org/abs/2302.12173
- Ângulo: pesquisa de ataque

Este foi o primeiro paper a documentar a injeção indireta de prompt, provavelmente o vetor de ataque mais perigoso para sistemas de IA em produção. Ele mostrou que aplicações como o Bing Chat (GPT-4) podiam ser exploradas remotamente, e é citado na própria documentação do OWASP LLM Top 10.

---

## 5. SAIF, Secure AI Framework

- Instituição: Google
- Tipo: página web aberta
- Link: https://saif.google/
- Ângulo: defesa e boas práticas corporativas

O SAIF é o framework de segurança de IA do Google, voltado a quem constrói IA em escala. Traz a visão defensiva e corporativa: controles, riscos inerentes e, na versão mais recente, segurança de agentes autônomos. Fecha o ciclo com a perspectiva de quem opera IA em produção.

---

## Como as fontes se conectam

```
   AMEAÇA          RISCO           ATAQUE          DEFESA       GOVERNANÇA
     │               │               │               │              │
 MITRE ATLAS     OWASP Top 10    Greshake et al.  Google SAIF   NIST AI RMF
 (o que os       (o que pode     (como um ataque  (como me      (como gerir
  atacantes       dar errado)     real funciona)   defendo)      o risco)
  fazem)
```

Juntas, as cinco fontes cobrem o mesmo conjunto de ângulos que um time de segurança experiente usa para proteger um sistema de IA, do ataque à governança.
