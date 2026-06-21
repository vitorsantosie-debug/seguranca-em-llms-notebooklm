# NotebookLM: Segurança em LLMs

> Caderno de estudos sobre segurança em modelos de linguagem (LLMs), construído com o NotebookLM. O tema fica na fronteira entre IA generativa e cibersegurança, os dois eixos centrais do bootcamp.

## Sumário

1. [Contexto e objetivos](#1-contexto-e-objetivos)
2. [Curadoria de fontes](#2-curadoria-de-fontes)
3. [Engenharia de prompts e "cicatrizes"](#3-engenharia-de-prompts-e-cicatrizes)
4. [Miniguia de estudo (entrega final)](#4-miniguia-de-estudo-entrega-final)
   - [4.1 Resumos estruturados](#41-resumos-estruturados)
   - [4.2 Glossário](#42-glossário)
   - [4.3 Prompts reutilizáveis](#43-prompts-reutilizáveis)
5. [Conclusão e aprendizados](#5-conclusão-e-aprendizados)
6. [Como reproduzir](#6-como-reproduzir-este-projeto)

---

## 1. Contexto e objetivos

### O assunto escolhido

Empresas estão colocando IA generativa em produção: chatbots, copilotos e assistentes com acesso a dados internos e APIs. Isso abre uma superfície de ataque que a segurança tradicional não cobre bem. Um LLM segue instruções escritas em linguagem natural, então qualquer texto que ele leia pode acabar sendo interpretado como comando.

Foi por isso que escolhi estudar segurança em LLMs. O tema junta os três eixos do Bootcamp Bradesco (GenAI, Dados e Cyber): usa IA generativa, lida com dados sensíveis e, no fundo, é um problema de segurança.

### Por que o tema importa

Qualquer empresa que coloca um LLM em produção precisa lidar com isso, de bancos a varejo e saúde. Hoje é assunto de diretoria, não só do time técnico. É também uma das habilidades de IA mais pedidas em vagas, e ainda tem pouco material em português. E não é teoria: ataques de prompt injection já aconteceram em produtos reais, como o Bing Chat rodando GPT-4, caso documentado mais adiante neste caderno.

### Objetivos de estudo

| # | Objetivo | Resultado esperado |
|---|----------|--------------------|
| 1 | Entender quais são os principais riscos de segurança em LLMs | Dominar o OWASP Top 10 for LLMs (2025) |
| 2 | Diferenciar os vetores de ataque mais críticos | Compreender prompt injection direto e indireto |
| 3 | Conhecer os frameworks de governança e ameaça usados pela indústria | NIST AI RMF, MITRE ATLAS e Google SAIF |
| 4 | Saber como mitigar os riscos na prática | Checklists aplicáveis por times de engenharia |
| 5 | Praticar engenharia de prompts como ferramenta de estudo | Biblioteca de prompts reutilizáveis |

---

## 2. Curadoria de fontes

O critério foi simples: poucas fontes, mas as mais respeitadas do mundo em segurança de IA, escolhidas para cobrir o ciclo completo, de ameaça e risco até ataque, defesa e governança. Preferi diversidade de ângulo a quantidade.

As cinco fontes abaixo foram carregadas no NotebookLM:

| # | Fonte | Instituição | Ângulo coberto | Link |
|---|-------|-------------|----------------|------|
| 1 | OWASP Top 10 for LLM Applications (2025) | OWASP | Taxonomia prática de riscos | [genai.owasp.org](https://genai.owasp.org/llm-top-10/) |
| 2 | AI Risk Management Framework 1.0 | NIST (gov. EUA) | Governança e gestão de risco | [PDF NIST.AI.100-1](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf) |
| 3 | MITRE ATLAS | MITRE | Inteligência de ameaças (táticas e técnicas) | [atlas.mitre.org](https://atlas.mitre.org/) |
| 4 | Not what you've signed up for: Indirect Prompt Injection (Greshake et al., 2023) | Pesquisa acadêmica (arXiv) | Pesquisa de ataque | [arXiv:2302.12173](https://arxiv.org/abs/2302.12173) |
| 5 | SAIF, Secure AI Framework | Google | Defesa e boas práticas corporativas | [saif.google](https://saif.google/) |

Por que essas e não outras:

- O OWASP é o padrão global de segurança de aplicações há mais de vinte anos. O Top 10 para LLMs foi escrito por centenas de especialistas e virou a referência inicial do setor.
- O NIST define padrões técnicos que governos e empresas adotam no mundo todo.
- A MITRE mantém o ATT&CK, base de inteligência de ameaças usada por times de segurança em todo lugar. O ATLAS é a versão dele para sistemas de IA.
- O paper de Greshake et al. foi o primeiro a documentar a injeção indireta de prompt, e é citado na própria documentação do OWASP.
- O Google SAIF traz a parte defensiva e corporativa, fechando o ciclo com a visão de quem opera IA em escala.

O detalhamento de cada fonte está em [`/fontes/FONTES.md`](fontes/FONTES.md).

> Evidência: as cinco fontes foram carregadas e ficaram ativas no NotebookLM (a tabela acima lista todas). Para anexar um print, salve-o como `assets/notebooklm-fontes.png` e descomente a linha abaixo no README:
>
> `<!-- ![Fontes carregadas no NotebookLM](assets/notebooklm-fontes.png) -->`

---

## 3. Engenharia de prompts e "cicatrizes"

Esta seção mostra o caminho até os resultados: as perguntas que fiz, as variações que testei, as respostas que obtive e, principalmente, as dificuldades que apareceram pelo caminho. Achei que isso valia mais do que só mostrar o resultado limpo.

### A principal cicatriz: prompt vago contra prompt específico

A lição que mais ficou foi sentir, na prática, o quanto a especificidade do prompt muda a resposta. Testei a mesma intenção ("como me defender de ataques?") de duas formas:

<table>
<tr>
<th width="50%">Prompt vago</th>
<th width="50%">Prompt específico</th>
</tr>
<tr>
<td valign="top">

```
como evitar ataques?
```

</td>
<td valign="top">

```
Atue como um engenheiro de segurança. Liste as 5
principais estratégias de mitigação especificamente
para o risco LLM01 Prompt Injection, conforme o OWASP.
Apresente como um checklist acionável (caixas de marcar)
para um time de engenharia, cada item com no máximo
duas linhas. Responda em português.
```

</td>
</tr>
<tr>
<td valign="top">

Resposta genérica e dispersa. Misturou governança, mapeamento e controles de várias fontes ao mesmo tempo, sem aprofundar em nada e sem formato útil para o dia a dia.

</td>
<td valign="top">

Checklist direto e aplicável: privilégio mínimo, human-in-the-loop, segregação de contexto, sanitização de saída e monitoramento. Com persona, escopo (LLM01) e formato definidos, virou algo pronto para entrar numa sprint.

</td>
</tr>
</table>

O que aprendi: um bom prompt costuma ter quatro partes, persona, tarefa específica, escopo ou fonte, e formato de saída.

### Outras cicatrizes (problemas reais que enfrentei)

| Dificuldade | Causa | Como resolvi |
|-------------|-------|--------------|
| O PDF do NIST entrou com a URL crua no lugar do título | O NotebookLM nem sempre gera título para PDFs importados por link | Confirmei que o conteúdo tinha sido importado fazendo uma pergunta específica do NIST. A resposta veio citando a fonte |
| A IA tendia a misturar as fontes numa resposta ampla | Perguntas genéricas puxam trechos de todas as fontes ao mesmo tempo | Passei a nomear a fonte no prompt, por exemplo "conforme o OWASP" ou "no paper de Greshake et al." |
| A importação só traz o texto visível dos sites | Limitação do NotebookLM, que não lê conteúdo pago ou dinâmico | Priorizei páginas oficiais e o PDF direto do NIST |

### Perguntas estratégicas executadas

As seis perguntas que deram forma ao caderno (respostas completas em [`/prompts/PROMPTS-E-RESPOSTAS.md`](prompts/PROMPTS-E-RESPOSTAS.md)):

1. Quais são as quatro funções do NIST AI RMF (GOVERN, MAP, MEASURE, MANAGE)?
2. Quais são os dez riscos do OWASP Top 10 for LLMs (2025)?
3. Qual a diferença entre injeção de prompt direta e indireta? (com exemplo de Greshake et al.)
4. Como MITRE ATLAS e Google SAIF se complementam na defesa? (síntese entre fontes)
5. Versão vaga, "como evitar ataques?", para servir de comparação.
6. Versão específica, o checklist de mitigação para o LLM01.

---

## 4. Miniguia de estudo (entrega final)

O resultado consolidado do caderno. Tudo abaixo saiu das cinco fontes pelo NotebookLM e foi organizado para revisão rápida.

### 4.1 Resumos estruturados

#### OWASP Top 10 for LLM Applications (2025)

| Código | Risco (oficial) | O que é |
|--------|-----------------|---------|
| LLM01 | Prompt Injection | Comandos inseridos por usuários ou por fontes externas alteram o comportamento pretendido do modelo. |
| LLM02 | Sensitive Information Disclosure | Revelação não intencional de dados confidenciais nas respostas ou via treinamento. |
| LLM03 | Supply Chain | Vulnerabilidades na cadeia de IA: componentes de terceiros, modelos pré-treinados ou datasets comprometidos. |
| LLM04 | Data and Model Poisoning | Manipulação dos dados de treino, ajuste ou embeddings para introduzir falhas ou comportamentos maliciosos. |
| LLM05 | Improper Output Handling | Validação e sanitização insuficientes da saída do modelo antes de usá-la em outros componentes. |
| LLM06 | Excessive Agency | Permissões ou autonomia excessivas concedidas ao sistema para agir no mundo real. |
| LLM07 | System Prompt Leakage | Exposição indevida de instruções internas e configurações que deveriam ficar ocultas. |
| LLM08 | Vector and Embedding Weaknesses | Riscos em sistemas que dependem de bancos vetoriais e embeddings, como o RAG. |
| LLM09 | Misinformation | Geração e disseminação de informações falsas ou imprecisas pelo modelo. |
| LLM10 | Unbounded Consumption | Consumo ilimitado de recursos, levando a negação de serviço (DoS) ou custos excessivos. |

Fonte: OWASP Gen AI Security Project.

#### NIST AI Risk Management Framework: as quatro funções do "Core"

- GOVERN (governar): função transversal. Cultiva a cultura de gestão de risco e define políticas, papéis e responsabilidades. A liderança sênior dá o tom.
- MAP (mapear): estabelece o contexto. Identifica onde o LLM opera, seus limites e os riscos associados.
- MEASURE (mensurar): analisa, avalia e monitora os riscos mapeados com métricas e testes.
- MANAGE (gerenciar): prioriza e trata os riscos, aloca recursos e responde a incidentes.

Fonte: NIST AI 100-1.

#### Prompt injection: direto e indireto

- Direto: o usuário fala direto com o LLM e tenta burlar as instruções e salvaguardas. O caso clássico é o jailbreaking via chat.
- Indireto: o atacante não toca no chat. Ele esconde comandos maliciosos em fontes externas (sites, documentos, e-mails) que o LLM vai ler e processar. O modelo acaba executando as instruções do atacante enquanto atende a um pedido legítimo do usuário.

> Exemplo real (Greshake et al.): aplicações como o Bing Chat (GPT-4), com acesso à internet, podem ser exploradas remotamente, sem que o atacante precise de interface direta. Ele planta um comando num site e, quando o usuário pede para o LLM resumir aquela página, o modelo lê e obedece às instruções ocultas. Isso pode, por exemplo, induzir o usuário a clicar em links de phishing.

#### MITRE ATLAS e Google SAIF (síntese entre fontes)

| Característica | MITRE ATLAS | Google SAIF |
|---------------|-------------|-------------|
| Foco | Perspectiva do atacante | Perspectiva defensiva e prática |
| Estrutura | Matriz de conhecimento no estilo ATT&CK: táticas e técnicas de ataques reais | Framework de governança: riscos inerentes e controles |
| Para LLMs | Cataloga métodos como prompt injection, envenenamento de RAG e extração de system prompt | Segurança de agentes autônomos e escala de defesas |

> Como se complementam: o ATLAS diz o que monitorar, ou seja, quais táticas os atacantes usam no mundo real. O SAIF diz como integrar segurança ao ciclo de desenvolvimento da IA. Um cobre o ataque, o outro cobre a defesa.

### 4.2 Glossário

| Termo | Definição |
|-------|-----------|
| Cadeia de suprimentos (supply chain) | Vulnerabilidades em componentes de terceiros (modelos, datasets, ferramentas) que comprometem o sistema de IA. |
| Data poisoning (envenenamento de dados) | Manipulação dos dados de treino, ajuste ou recuperação para introduzir falhas, backdoors ou comportamento malicioso. |
| Embedding (incorporação) | Representação numérica (vetorial) que captura relações semânticas. Pode ter fraquezas exploráveis se o banco vetorial não for protegido. |
| Excessive agency (agência excessiva) | Quando o LLM recebe autonomia ou permissões demais para agir em outros sistemas sem supervisão humana adequada. |
| Jailbreaking | Prompts elaborados para forçar o LLM a ignorar suas salvaguardas e gerar conteúdo proibido. |
| MITRE ATLAS | Matriz de táticas e técnicas adversárias contra sistemas de IA, baseada em ataques reais. |
| NIST AI RMF | Framework de gestão de risco de IA do NIST, organizado em GOVERN, MAP, MEASURE e MANAGE. |
| Prompt injection (injeção de prompt) | Comandos maliciosos, diretos ou indiretos, que alteram as instruções e o comportamento pretendido do modelo. |
| RAG (retrieval-augmented generation) | Técnica que recupera dados externos para enriquecer respostas. É suscetível a envenenamento de contexto. |
| Red teaming | Teste em que equipes simulam táticas reais de ataque para achar vulnerabilidades antes dos invasores. |
| SAIF (secure AI framework) | Framework do Google com riscos inerentes e controles para construir e operar IA com segurança. |

### 4.3 Prompts reutilizáveis

Biblioteca pronta para revisões futuras sobre o tema. Basta colar no NotebookLM ou em qualquer LLM com as fontes certas:

```text
1. RESUMO RÁPIDO
"Resuma o conceito de {TEMA} em até 5 bullets, em português,
citando a fonte de cada afirmação."

2. CHECKLIST DE MITIGAÇÃO
"Atue como engenheiro de segurança. Liste as principais estratégias
de mitigação para o risco {LLM0X} segundo o OWASP, em formato de
checklist acionável, cada item com no máximo duas linhas."

3. COMPARAÇÃO ENTRE FONTES
"Compare como {FONTE A} e {FONTE B} abordam {TEMA}. Apresente em
tabela e finalize com um parágrafo de síntese sobre como se complementam."

4. EXPLIQUE COMO PARA UM INICIANTE
"Explique {CONCEITO} como se eu fosse um desenvolvedor júnior,
usando uma analogia do dia a dia e um exemplo concreto das fontes."

5. GERADOR DE GLOSSÁRIO
"Crie um glossário com os {N} termos técnicos mais importantes sobre
{TEMA} mencionados nas fontes, ordenados alfabeticamente, com
definição de 1-2 frases cada."

6. SIMULADO DE ESTUDO
"Crie 5 perguntas de múltipla escolha sobre {TEMA} com base nas
fontes, com gabarito comentado ao final, para eu testar meu conhecimento."
```

A biblioteca completa e comentada está em [`/prompts/PROMPTS-REUTILIZAVEIS.md`](prompts/PROMPTS-REUTILIZAVEIS.md).

---

## 5. Conclusão e aprendizados

Montar este caderno me mostrou que dá para usar IA como ferramenta de estudo de verdade, e não como atalho. O NotebookLM acelerou a leitura de cinco fontes densas, mas o valor real veio de três coisas. A primeira foi escolher as fontes certas, porque se a entrada é ruim a saída também é. A segunda foi aprender a formular boas perguntas, já que a especificidade do prompt define a qualidade da resposta. A terceira foi conferir tudo com olhar crítico, sempre checando as citações, até porque a própria ferramenta avisa que pode errar.

Sobre o tema em si: segurança em LLMs deixou de ser teoria. Com empresas colocando IA em produção, entender prompt injection, o OWASP Top 10 e frameworks como NIST, MITRE e SAIF virou uma competência concreta e cada vez mais cobrada.

---

## 6. Como reproduzir este projeto

1. Acesse o [NotebookLM](https://notebooklm.google.com) e crie um novo notebook.
2. Adicione as cinco fontes listadas na [seção 2](#2-curadoria-de-fontes), usando "Adicionar fonte > Sites" e o PDF do NIST.
3. Execute as perguntas estratégicas da [seção 3](#3-engenharia-de-prompts-e-cicatrizes).
4. Compare prompts vagos com prompts específicos e registre as suas próprias cicatrizes.
5. Consolide os resultados no seu miniguia.

---

## Estrutura do repositório

```
.
├── README.md                          # Este arquivo (entrega principal)
├── fontes/
│   └── FONTES.md                      # Curadoria detalhada das 5 fontes
├── prompts/
│   ├── PROMPTS-E-RESPOSTAS.md         # Perguntas estratégicas e respostas reais
│   └── PROMPTS-REUTILIZAVEIS.md       # Biblioteca de prompts para revisão
├── miniguia/
│   └── MINIGUIA.md                    # Miniguia consolidado (versão standalone)
└── assets/
    └── LEIA-ME.md                     # Instruções para anexar o print do notebook
```

---

Projeto desenvolvido para o Desafio de Projeto do Bootcamp Bradesco (GenAI, Dados e Cyber), na Digital Innovation One (DIO).
