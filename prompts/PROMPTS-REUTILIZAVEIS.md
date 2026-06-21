# Biblioteca de prompts reutilizáveis

Prompts prontos para revisões futuras sobre segurança em LLMs, ou sobre qualquer tema de estudo no NotebookLM. Substitua os campos entre `{ }`.

> Anatomia de um bom prompt: persona, tarefa específica, escopo ou fonte, e formato de saída. Quanto mais específico, melhor a resposta.

---

## Estudo e compreensão

### 1. Resumo rápido
```
Resuma o conceito de {TEMA} em até 5 bullets, em português,
citando a fonte de cada afirmação.
```

### 2. Explique como para um iniciante
```
Explique {CONCEITO} como se eu fosse um desenvolvedor júnior,
usando uma analogia do dia a dia e um exemplo concreto das fontes.
```

### 3. Aprofundamento progressivo
```
Explique {TEMA} em três níveis: (1) uma frase para leigos,
(2) um parágrafo para um dev, (3) detalhes técnicos para um
especialista em segurança. Cite as fontes.
```

---

## Aplicação prática (segurança)

### 4. Checklist de mitigação
```
Atue como engenheiro de segurança. Liste as principais estratégias
de mitigação para o risco {LLM0X} segundo o OWASP, em formato de
checklist acionável, cada item com no máximo duas linhas.
```

### 5. Modelagem de ameaça
```
Com base no MITRE ATLAS, quais táticas e técnicas um atacante poderia
usar contra um sistema {DESCREVA O SISTEMA, ex: chatbot com RAG}?
Liste em ordem de criticidade.
```

### 6. Plano de resposta
```
Atue como líder de segurança. Monte um plano de resposta em 5 passos
para um incidente de {TIPO DE ATAQUE} em uma aplicação com LLM,
baseado nos frameworks das fontes.
```

---

## Análise e comparação

### 7. Comparação entre fontes
```
Compare como {FONTE A} e {FONTE B} abordam {TEMA}. Apresente em
tabela e finalize com um parágrafo de síntese sobre como se complementam.
```

### 8. Mapeamento cruzado
```
Mapeie cada risco do OWASP Top 10 para LLMs às funções do NIST AI RMF
(GOVERN, MAP, MEASURE, MANAGE) que ajudam a mitigá-lo. Apresente em tabela.
```

---

## Revisão e autoavaliação

### 9. Gerador de glossário
```
Crie um glossário com os {N} termos técnicos mais importantes sobre
{TEMA} mencionados nas fontes, ordenados alfabeticamente, com
definição de 1-2 frases cada.
```

### 10. Simulado de estudo
```
Crie 5 perguntas de múltipla escolha sobre {TEMA} com base nas fontes,
com gabarito comentado ao final, para eu testar meu conhecimento.
```

### 11. Flashcards
```
Gere 10 flashcards (pergunta no formato "Frente" / resposta "Verso")
sobre {TEMA}, prontos para revisão espaçada.
```

### 12. Mapa de revisão
```
Crie um roteiro de revisão de {TEMA} em ordem lógica de aprendizado,
do conceito mais básico ao mais avançado, indicando qual fonte estudar
em cada etapa.
```

---

> Dica que funcionou bem: quando a resposta vier genérica, nomeie a fonte no prompt (por exemplo, "conforme o OWASP" ou "segundo o NIST") e defina o formato (tabela, checklist, bullets). Foi o ajuste que mais melhorou a qualidade neste projeto.
