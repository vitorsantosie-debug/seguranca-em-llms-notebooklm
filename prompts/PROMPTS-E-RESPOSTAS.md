# Perguntas estratégicas e respostas

Registro das perguntas que fiz no NotebookLM, com as respostas que obtive (resumidas) e as referências que a ferramenta citou. Funciona como o diário de bordo da engenharia de prompts do projeto.

> Todas as respostas vieram do NotebookLM, a partir das cinco fontes do caderno. A própria ferramenta avisa que pode gerar respostas incorretas e pede para checar o conteúdo, então cada afirmação aparece com a fonte citada.

---

## Prompt 1: funções do NIST AI RMF

Pergunta:
> Quais são as quatro funções principais do NIST AI Risk Management Framework (GOVERN, MAP, MEASURE, MANAGE) e o que cada uma significa na prática? Responda em português e cite a fonte.

Resposta obtida (resumo). As quatro funções formam o "Core" do framework:
- GOVERN (governar): função transversal às outras. Cultiva a cultura de gestão de risco e estabelece políticas, processos e estruturas organizacionais. A liderança sênior define o tom.
- MAP (mapear): estabelece o contexto e identifica os riscos associados ao sistema de IA e a seus limites.
- MEASURE (mensurar): analisa, avalia e monitora os riscos com métricas e testes.
- MANAGE (gerenciar): prioriza e trata os riscos, aloca recursos e responde a incidentes.

Fonte citada: NIST AI 100-1.

---

## Prompt 2: OWASP Top 10 for LLMs (2025)

Pergunta:
> Liste os 10 riscos do OWASP Top 10 for LLM Applications 2025. Para cada um, escreva o código (LLM01 a LLM10), o nome oficial em inglês e uma descrição de uma frase em português. Cite a fonte.

Resposta obtida:

| Código | Nome | Descrição |
|--------|------|-----------|
| LLM01:2025 | Prompt Injection | Comandos inseridos por usuários alteram a execução ou o comportamento pretendido do modelo. |
| LLM02:2025 | Sensitive Information Disclosure | Revelação não intencional de dados confidenciais nas respostas ou via treinamento. |
| LLM03:2025 | Supply Chain | Vulnerabilidades na cadeia de IA: componentes de terceiros, modelos ou datasets comprometidos. |
| LLM04:2025 | Data and Model Poisoning | Manipulação dos dados de pré-treino, ajuste ou embeddings para introduzir falhas ou comportamento malicioso. |
| LLM05:2025 | Improper Output Handling | Validação e sanitização insuficientes da saída do modelo. |
| LLM06:2025 | Excessive Agency | Permissões ou autonomia excessivas para o sistema agir no mundo real. |
| LLM07:2025 | System Prompt Leakage | Exposição indevida de instruções internas e configurações do sistema. |
| LLM08:2025 | Vector and Embedding Weaknesses | Riscos em sistemas que dependem de bancos vetoriais e embeddings. |
| LLM09:2025 | Misinformation | Geração e disseminação de informações falsas ou imprecisas. |
| LLM10:2025 | Unbounded Consumption | Consumo ilimitado de recursos, causando DoS ou custos excessivos. |

Fonte citada: LLMRisks Archive, OWASP Gen AI Security Project.

---

## Prompt 3: injeção de prompt, direta e indireta

Pergunta:
> Explique a diferença entre injeção de prompt direta e injeção de prompt indireta. Dê um exemplo concreto de injeção indireta conforme descrito no paper de Greshake et al. (Not what you've signed up for). Responda em português.

Resposta obtida (resumo):
- Injeção direta: o usuário interage diretamente com o LLM e envia comandos para ignorar ou substituir as instruções e controles originais. O caso mais comum é o jailbreaking via chat.
- Injeção indireta: o invasor não tem acesso ao chat, mas coloca prompts maliciosos em fontes externas (sites, documentos, bancos de dados) que o sistema vai recuperar e processar. Ao ler esses dados para responder a uma consulta legítima, o LLM acaba executando as instruções do atacante.

Exemplo concreto (Greshake et al.): aplicações integradas a LLMs, como o Bing Chat (GPT-4) com acesso à internet, ficam vulneráveis a exploração remota sem interface de ataque direta. O invasor planta um comando malicioso num site e, quando o usuário pede ao LLM para resumir aquela página, o modelo lê e obedece às instruções do atacante. Isso pode, por exemplo, persuadir o usuário a clicar em links de phishing ou influenciar como outras APIs são chamadas.

Fonte citada: Not what you've signed up for (arXiv:2302.12173).

---

## Prompt 4: síntese entre fontes, MITRE ATLAS e Google SAIF

Pergunta:
> Compare a abordagem do MITRE ATLAS com a do Google SAIF para segurança de IA. Qual o foco de cada um e como se complementam na defesa de um sistema com LLM? Apresente em uma tabela e depois um parágrafo de síntese. Responda em português.

Resposta obtida (resumo):

| Característica | MITRE ATLAS | Google SAIF |
|---------------|-------------|-------------|
| Foco | Perspectiva do atacante | Perspectiva prática e defensiva |
| Estrutura | Matriz de conhecimento no estilo ATT&CK, com táticas e técnicas de ataques reais | Framework de governança: riscos inerentes e controles |
| Para LLMs | Métodos como injeção de prompt (direta e indireta), envenenamento de RAG, extração de system prompt | Segurança de agentes autônomos e escala de defesas |

Síntese: o ATLAS e o SAIF se complementam ao cobrir os lados de ataque e de defesa. O ATLAS ajuda na modelagem de ameaças, ou seja, a entender como um invasor exploraria a injeção indireta de Greshake et al. ou o contexto de um banco vetorial (RAG). O SAIF fornece a estrutura e os controles para mitigar esses ataques de forma sistemática. Em resumo, o ATLAS diz o que monitorar e o SAIF diz como integrar segurança ao ciclo de vida da IA.

Fontes citadas: MITRE ATLAS, SAIF (Google's Guide to Secure AI) e referência cruzada a Greshake et al.

---

## Prompt 5: cicatriz, versão vaga

Pergunta:
> como evitar ataques?

Resposta obtida: ampla e dispersa. Listou uma "defesa em camadas" que misturava governança (NIST), mapeamento de contexto (NIST e MITRE) e controles técnicos, sem aprofundar em nada e sem formato útil. Serve como visão geral, mas não como guia prático.

O que aprendi: prompt vago gera resposta vaga. O contraste aparece no Prompt 6.

---

## Prompt 6: cicatriz, versão específica

Pergunta:
> Atue como um engenheiro de segurança. Liste as 5 principais estratégias de mitigação especificamente para o risco LLM01 Prompt Injection, conforme o OWASP. Apresente como um checklist acionável (formato de caixas de marcar) para um time de engenharia, cada item com no máximo duas linhas. Responda em português.

Resposta obtida:
- [ ] Privilégio mínimo: restrinja o acesso do LLM apenas às APIs e bancos estritamente necessários.
- [ ] Human-in-the-loop: exija aprovação humana para ações críticas, como exclusão de dados ou transações.
- [ ] Segregação de contexto: use delimitadores claros separando as instruções do sistema do conteúdo do usuário ou externo.
- [ ] Sanitização de saída: trate toda resposta do LLM como dado não confiável e valide antes de processar.
- [ ] Monitoramento: registre e acompanhe as interações para detectar tentativas de injeção.

Fonte citada: OWASP Gen AI Security Project.

O que aprendi: juntar persona, tarefa específica, escopo ou fonte, e formato transforma uma resposta genérica em algo pronto para usar.
