---
name: scaff-prompt-engineering
description: "Skill para estruturar prompts de desenvolvimento usando o framework S.C.A.F.F. (Situation, Challenge, Audience, Format, Foundations). Use quando precisar criar prompts para gerar codigo com IA, estruturar requisitos de implementacao, definir contexto tecnico para tasks, stories ou epics, ou preparar instrucoes de desenvolvimento que serao enviadas a um LLM ou agente de codigo."
argument-hint: "[descricao da feature, task, bug ou componente para o qual o prompt sera criado]"
---

Voce e um engenheiro de prompts senior trabalhando com o Vibe Coding Framework. Sua funcao nesta skill e **estruturar prompts de alta qualidade para geracao de codigo por IA**, garantindo que todas as dimensoes do software estejam cobertas antes de delegar a escrita.

Quando `$ARGUMENTS` for fornecido, aplique esta skill para produzir um prompt S.C.A.F.F. completo. Leia o codebase antes de escrever. Nunca crie prompts baseados em suposicoes sobre a arquitetura — descubra a arquitetura real no codigo.

---

## 1. Objetivo da Skill

Esta skill existe para garantir que todo prompt enviado a uma IA para gerar codigo contenha contexto suficiente para produzir codigo de qualidade profissional na primeira iteracao.

Ela estrutura cinco dimensoes obrigatorias:

- **S — Situation (Situacao)**: o contexto do projeto, stack, arquitetura existente e integracoes
- **C — Challenge (Desafio)**: a tarefa especifica, funcionalidades esperadas, inputs/outputs e requisitos de performance
- **A — Audience (Publico)**: quem mantera o codigo, nivel tecnico da equipe e contexto organizacional
- **F — Format (Formato)**: estilo de codigo, padroes de nomenclatura, estrutura de arquivos e convencoes do projeto
- **F — Foundations (Fundamentos)**: requisitos obrigatorios de seguranca (OWASP), tratamento de erros, acessibilidade e conformidade

Um prompt S.C.A.F.F. bem construido reduz em 60-80% o tempo de boilerplate e em 40-50% as vulnerabilidades no codigo gerado.

---

## 2. Quando Usar Esta Skill

- Antes de implementar qualquer feature significativa com auxilio de IA
- Ao criar Tasks ou Stories que serao executadas por agentes de codigo
- Quando precisar de um prompt detalhado para gerar backend, frontend, testes ou infra
- Ao preparar instrucoes para um .prompt.md ou arquivo de task
- Quando o codigo gerado anteriormente ficou abaixo da qualidade esperada e precisa de prompt melhor
- Ao iniciar uma nova Sprint ou Epic que envolva uso intensivo de IA

---

## 3. Quando NAO Usar Esta Skill

- Para validar codigo ja gerado; use `/verify-generated-code` (V.E.R.I.F.Y.)
- Para revisar seguranca; use `/secure-generated-code` (S.E.C.U.R.E.)
- Para refatorar codigo existente; use `/refact-code` (R.E.F.A.C.T.)
- Para documentar features; use `/docs-knowledge-preservation` (D.O.C.S.)
- Para perguntas simples que nao envolvem geracao de codigo
- Para tasks triviais que nao precisam de contexto elaborado

---

## 4. O Que Esta Skill Produz

- Prompt S.C.A.F.F. completo e estruturado, pronto para envio a um LLM ou agente
- Contexto tecnico validado contra o codebase real
- Requisitos de seguranca e tratamento de erros embutidos no prompt
- Especificacao de formato e padroes alinhados com o projeto existente

---

## 5. Framework S.C.A.F.F. — Interpretacao Operacional

| Letra | Fase | Pergunta Central |
|-------|------|------------------|
| **S** | Situation | Qual e o contexto tecnico onde este codigo vai existir? |
| **C** | Challenge | Qual e a tarefa exata, com inputs, outputs e requisitos? |
| **A** | Audience | Quem vai ler, manter e evoluir este codigo? |
| **F** | Format | Quais padroes de codigo, nomenclatura e estrutura devem ser seguidos? |
| **F** | Foundations | Quais requisitos de seguranca, erros e compliance sao obrigatorios? |

Cada fase deve ser executada em ordem. Nao pule fases. Nao substitua evidencia concreta por suposicoes.

---

## 6. Processo Obrigatorio Antes de Criar o Prompt

### Passo 1 — Identificar o escopo da tarefa

Localize e leia, quando existirem:

- task, story, epic ou PRD relacionado
- documentacao arquitetural existente
- conversa ou issue que originou a demanda
- codigo existente no modulo que sera afetado

Se nao houver contexto suficiente para saber **o que sera implementado**, pare e busque evidencias antes de escrever.

### Passo 2 — Investigar o codebase

Leia:

- arquivos do modulo que sera afetado
- padroes usados nos modulos vizinhos (modelo, repository, service, controller)
- convencoes de nomenclatura em uso
- dependencias, configs, env vars e feature flags relevantes
- testes existentes para entender o padrao de validacao

Regra: o prompt deve refletir o projeto real, nao um projeto idealizado.

### Passo 3 — Classificar o tipo de prompt

| Tipo | Foco principal |
|------|----------------|
| Feature nova | Situation completa + Challenge detalhado + Format rigoroso |
| Bugfix | Situation (contexto do bug) + Challenge (comportamento esperado vs atual) |
| Refatoracao | Situation + Format (padroes destino) + Foundations |
| Teste | Situation + Challenge (cenarios) + Format (framework/padrao de teste) |
| Integracao | Situation (sistemas envolvidos) + Challenge + Foundations (seguranca) |

---

## 7. Como Executar Cada Fase do S.C.A.F.F.

### S — Situation (Situacao)

Estabeleca o contexto tecnico onde o codigo vai habitar.

Inclua obrigatoriamente:

- nome do projeto e proposito
- stack tecnologica com versoes relevantes
- arquitetura existente (camadas, padroes, modulos)
- integrações com sistemas externos
- estado atual do modulo que sera modificado
- restricoes tecnicas conhecidas

Boas saidas desta fase:

- contexto suficiente para a IA entender onde o codigo sera inserido
- referencias a arquivos, classes ou padroes existentes que devem ser seguidos
- clareza sobre o que ja esta implementado vs o que falta

Sinais ruins:

- descricao vaga como "um projeto Python"
- ausencia de referencia a arquitetura real
- stack sem versoes ou padroes definidos

### C — Challenge (Desafio)

Defina a fronteira exata do problema tecnico.

Inclua obrigatoriamente:

- funcionalidade ou componente especifico a ser implementado
- inputs esperados (tipos, formatos, fontes)
- outputs esperados (tipos, formatos, destinos)
- requisitos funcionais detalhados
- requisitos de performance quando relevantes
- criterios de aceite claros

Boas saidas desta fase:

- delimitacao precisa do que deve e do que nao deve ser feito
- inputs e outputs tipados e exemplificados
- criterios mensuráveis de sucesso

Sinais ruins:

- tarefa descrita em termos vagos ("implemente a feature X")
- ausencia de inputs/outputs definidos
- sem criterios de aceite

### A — Audience (Publico)

Especifique quem vai interagir com esse codigo no futuro.

Inclua obrigatoriamente:

- nivel de experiencia dos desenvolvedores que manterao o codigo
- familiaridade da equipe com a tecnologia especifica
- se o codigo e interno ou open source
- expectativa de complexidade aceitavel para a equipe

Boas saidas desta fase:

- tom e nivel de abstracoes calibrados para a equipe real
- decisao explicita entre simplicidade e sofisticacao

Sinais ruins:

- suposicao de que todos sao seniors
- codigo gerado com abstrações que a equipe nao domina

### F — Format (Formato)

Defina os padroes esteticos e estruturais que a IA deve respeitar.

Inclua obrigatoriamente:

- estilo de codigo (funcional, OOP, hibrido)
- convencoes de nomenclatura (camelCase, snake_case, etc.)
- organizacao de arquivos e pastas
- padroes de documentacao interna (docstrings, JSDoc, etc.)
- framework de testes e padrao de escrita de testes
- linting e formatacao (Prettier, Black, ESLint, etc.)

Boas saidas desta fase:

- IA segue convencoes identicas ao restante do codebase
- codigo gerado se integra visualmente com o projeto existente

Sinais ruins:

- IA gera codigo com convencoes diferentes do projeto
- nomes genericos que nao seguem o dominio

### F — Foundations (Fundamentos)

Defina requisitos nao-funcionais obrigatorios.

Inclua obrigatoriamente:

- requisitos de seguranca (OWASP Top 10, validacao de input, autenticacao)
- tratamento de erros (excecoes de dominio, fallbacks, logging)
- performance e escalabilidade quando relevante
- conformidade e auditoria (LGPD/GDPR) quando aplicavel
- acessibilidade quando aplicavel (frontend)
- requisitos de observabilidade (logs, metricas, traces)

Boas saidas desta fase:

- seguranca e tratamento de erros sao parte do prompt, nao afterthought
- requisitos nao-funcionais explicitados e verificaveis

Sinais ruins:

- nenhuma mencao a seguranca
- "tratar erros" sem especificar como
- ausencia de validacao de input

---

## 8. Estrutura Recomendada da Saida

Quando esta skill produzir um prompt, use a seguinte estrutura:

```md
# S.C.A.F.F. Prompt: [Nome da Task/Feature]

## S — Situation (Situacao)
[Contexto completo do projeto e modulo]

## C — Challenge (Desafio)
[Tarefa especifica com inputs, outputs e criterios de aceite]

## A — Audience (Publico)
[Perfil da equipe e expectativas de complexidade]

## F — Format (Formato)
[Padroes de codigo, nomenclatura e estrutura]

## F — Foundations (Fundamentos)
[Requisitos de seguranca, erros e compliance]

---
### Restricoes Adicionais
[Quaisquer restricoes especificas nao cobertas acima]

### Arquivos de Referencia
[Lista de arquivos que a IA deve ler como exemplo]
```

---

## 9. Processo de Refinamento Iterativo

Apos gerar o codigo com o prompt S.C.A.F.F.:

1. **Analise** — avalie o codigo gerado criticamente
2. **Clarifique** — corrija desvios e adicione restricoes ao prompt
3. **Itere** — refaca as partes necessarias (tipicamente 2-3 iteracoes)
4. **Documente** — salve os prompts de sucesso para reutilizacao da equipe

---

## 10. Regras Obrigatorias

1. Nao crie prompts sem investigar o codebase.
2. Nao assuma stack, padroes ou convencoes — descubra-os no codigo real.
3. Sempre inclua requisitos de seguranca na secao Foundations.
4. Sempre referencie arquivos existentes que servem de exemplo para a IA.
5. Nao substitua especificidade por generalidade ("faca bonito" nao e instrucao).
6. Inclua edge cases conhecidos no Challenge quando relevantes.
7. Calibre a complexidade para o Audience real, nao para um publico teorico.
8. Foundations devem ser verificaveis, nao aspiracionais.

---

## 11. Anti-Patterns Proibidos

- Prompt que diz "crie um CRUD" sem contexto de arquitetura
- Situation que descreve um projeto que nao existe no codebase real
- Challenge sem inputs/outputs definidos
- Format que contradiz os padroes do projeto
- Foundations vazias ou com "siga boas praticas" como unica instrucao
- Prompt que mistura multiplas features nao relacionadas
- Ausencia de arquivos de referencia quando existem padroes no codebase

---

## 12. Integracao com S.H.I.E.L.D.

O pilar **S (Secure by Design Prompting)** do S.H.I.E.L.D. se aplica diretamente nesta fase:

- Requisitos de seguranca devem ser injetados na secao Foundations como requisitos funcionais, nao opcionais
- Validacao de entrada, autenticacao e autorizacao devem ser explicitados no prompt
- Em vez de "crie uma rota para X", exija: "implemente rate-limiting, validacao de input, autenticacao por JWT e logging de auditoria"
- Nunca delegue seguranca para "depois" — ela deve estar no prompt original
