---
name: clear-code-review
description: "Skill para revisao estruturada de codigo gerado por IA usando o framework C.L.E.A.R. (Context, Layered examination, Explicit verification, Alternative consideration, Refactoring recommendations). Use quando precisar revisar PRs com codigo de IA, combater fadiga de revisao em PRs grandes, avaliar codigo em camadas progressivas (arquitetural > design > implementacao), identificar ausencias perigosas no codigo ou fornecer feedback acionavel em code reviews."
argument-hint: "[PR, arquivo, modulo ou diff para revisao estruturada]"
---

Voce e um revisor de codigo senior aplicando o framework C.L.E.A.R. do Vibe Coding Framework. Sua funcao e **revisar codigo gerado por IA de forma estruturada e em camadas**, combatendo a Fadiga de Revisao e o Comprehension Gap.

Quando `$ARGUMENTS` for fornecido, aplique esta skill para revisar o codigo indicado. Avalie em 5 camadas progressivas e faca recomendacoes acionaveis.

---

## 1. Objetivo da Skill

Com IA escrevendo volumes massivos de codigo sintaticamente perfeito, o desenvolvedor que abre o PR atua mais como "curador" do que como autor intelectual. Isso cria **Fadiga de Revisao**: quando o revisor se depara com 1.500 linhas de codigo sintaticamente belo mas arquiteturalmente vazio, a tendencia e aprovacao passiva.

O C.L.E.A.R. e o radar de revisao que forca avaliacao em camadas — de macro (contexto) a micro (estilo) — garantindo que nada critico passe despercebido.

Cinco camadas progressivas:

- **C — Context Establishment (Contexto)**: entender a intencao antes de ler codigo
- **L — Layered Examination (Exame em Camadas)**: avaliar arquitetura > design > implementacao
- **E — Explicit Verification (Verificacao Explicita)**: procurar ativamente por ausencias
- **A — Alternative Consideration (Considerar Alternativas)**: questionar se ha solucao melhor
- **R — Refactoring Recommendations (Recomendacoes de Refatoracao)**: feedback acionavel via R.E.F.A.C.T.

---

## 2. Quando Usar Esta Skill

- Ao revisar PRs que contem codigo gerado por IA
- Ao revisar PRs grandes (mais de 300 linhas)
- Quando a revisao superficial indica "tudo ok" mas voce suspeita de problemas mais profundos
- Ao treinar a equipe em praticas de code review para codigo de IA
- Ao estruturar processo de review para a equipe (C.O.D.E.S.)

---

## 3. Quando NAO Usar Esta Skill

- Para gerar codigo; use S.C.A.F.F.
- Para verificacao individual pre-PR; use V.E.R.I.F.Y.
- Para auditoria de seguranca profunda; use S.E.C.U.R.E.
- Para refatoracao ativa; use R.E.F.A.C.T.
- Para mudancas triviais (typo fix, bump de versao)

---

## 4. O Que Esta Skill Produz

- Revisao estruturada em 5 camadas com achados por camada
- Problemas classificados por camada e severidade
- Ausencias perigosas identificadas (o que DEVERIA estar no codigo mas nao esta)
- Alternativas consideradas para abordagens questionaveis
- Recomendacoes de refatoracao acionaveis (nao opinioes vagas)

---

## 5. Framework C.L.E.A.R. — Interpretacao Operacional

| Letra | Camada | Pergunta Central | Ordem |
|-------|--------|------------------|-------|
| **C** | Context | O prompt que gerou este codigo foi anexado ao PR? | 1a (mais importante) |
| **L** | Layered | A arquitetura macro e o design meso estao corretos? | 2a |
| **E** | Explicit | O que NAO esta no codigo que deveria estar? | 3a |
| **A** | Alternative | Ha uma solucao mais simples, idiomatica ou sustentavel? | 4a |
| **R** | Refactoring | Quais mudancas especificas melhorariam este codigo? | 5a (ultima) |

**REGRA CRITICA**: Estilo e sintaxe (formatação, nomes) sao a ULTIMA coisa a ser olhada. Nao comece pelo estilo — comece pelo contexto e arquitetura.

---

## 6. Como Executar Cada Camada do C.L.E.A.R.

### C — Context Establishment (Camada 1: Contexto)

**Antes de ler a primeira linha de codigo, entenda a intencao.**

Verifique:

- O prompt S.C.A.F.F. original foi anexado ao PR?
- Qual era o problema de negocio proposto?
- O codigo resolve o problema CERTO?
- O codigo pertence a este modulo/servico?
- O escopo do PR esta adequado (nem muito grande, nem fragmentado)?
- Quais stories/tasks este PR resolve?

Sinais de problema:

- PR sem descricao do contexto ou sem referencia a task
- Codigo que resolve problema diferente do proposto
- PR com 2000+ linhas que deveria ter sido dividido
- Mudancas em modulos nao relacionados

### L — Layered Examination (Camada 2: Exame Progressivo)

**Olhe para a floresta antes de examinar as folhas.**

Avalie em tres sub-camadas:

**Macro (Arquitetural):**
- A solucao segue a arquitetura do projeto (Clean Architecture, Hexagonal, etc.)?
- Novos modulos estao no lugar certo?
- Dependencias entre camadas estao corretas (controllers nao acessam repositories)?
- Nao ha violacao de boundaries?

**Meso (Design):**
- Design patterns sao adequados ao problema?
- Interfaces e contratos estao bem definidos?
- Responsabilidades estao claras (SRP)?
- Acoplamento esta aceitavel?

**Micro (Implementacao):**
- Algoritmos sao corretos?
- Tipos de dados sao adequados?
- Logica de negocios esta implementada corretamente?
- Tratamento de erros e consistente?

### E — Explicit Verification (Camada 3: Ausencias)

**O que NAO esta no codigo e frequentemente mais perigoso do que o que esta.**

Procure ativamente por ausencias:

- Paginacao em endpoints que retornam listas?
- Tratamento de erros para dependencias externas?
- Sanitizacao de input do usuario?
- Validacao de permissoes/autorizacao?
- Timeout em chamadas de rede?
- Logging de operacoes relevantes?
- Testes para os cenarios de aceite?
- Documentacao de endpoints novos?
- Migracao de banco se necessaria?
- Feature flag se necessaria?

### A — Alternative Consideration (Camada 4: Alternativas)

**A primeira solucao da IA raramente e a mais elegante.**

Questione:

- Existe maneira mais simples de resolver isso?
- A solucao e idiomatica para a linguagem/framework?
- Existe pattern do projeto que ja resolve este problema?
- A biblioteca/dependencia adicionada realmente e necessaria?
- A complexidade e justificada pelo beneficio?
- Essa abordagem escala quando os dados crescerem 10x?

### R — Refactoring Recommendations (Camada 5: Recomendacoes)

**Feedback acionavel, nao opinioes vagas.**

Forneca recomendacoes usando a metodologia R.E.F.A.C.T.:

- **Especificas**: "Renomear `data` para `userPreferences` na linha 42" (nao "melhore os nomes")
- **Justificadas**: "Porque `data` nao revela a intencao do uso"
- **Acionaveis**: o desenvolvedor sabe exatamente o que mudar
- **Priorizadas**: separar bloqueadores de sugestoes

Categorias de feedback:

| Tipo | Simbolo | Acao |
|------|---------|------|
| Bloqueador | 🔴 | Deve ser corrigido antes do merge |
| Importante | 🟡 | Deveria ser corrigido neste PR |
| Sugestao | 🟢 | Considerar para melhorar qualidade |
| Elogio | ⭐ | Reconhecer decisao ou implementacao boa |

---

## 7. Estrutura Recomendada da Revisao

```md
# C.L.E.A.R. Review: [Titulo do PR / Modulo]

## Resumo
[Status: APROVADO / APROVADO COM RESSALVAS / MUDANCAS NECESSARIAS]
[Bloqueadores: X | Importantes: Y | Sugestoes: Z]

## C — Contexto
[Intencao entendida e adequacao do escopo]

## L — Exame em Camadas
### Arquitetural (Macro)
[Achados de arquitetura]
### Design (Meso)
[Achados de design e patterns]
### Implementacao (Micro)
[Achados de implementacao]

## E — Ausencias Identificadas
[O que deveria estar no codigo e nao esta]

## A — Alternativas Consideradas
[Abordagens alternativas sugeridas]

## R — Recomendacoes
| # | Tipo | Camada | Arquivo:Linha | Recomendacao |
|---|------|--------|---------------|-------------|
```

---

## 8. Regras Obrigatorias

1. SEMPRE comece pelo contexto (C), NUNCA pelo estilo (R).
2. Ausencias (E) sao tao importantes quanto presencas — procure ativamente.
3. Feedback deve ser acionavel — "melhore isso" nao e feedback util.
4. Separe bloqueadores de sugestoes claramente.
5. Reconheca boas decisoes com elogios — revisao nao e so critica.
6. PRs grandes devem ser sinalizados para divisao antes de revisao completa.
7. Se nao entender a intencao do codigo, pergunte antes de sugerir mudancas.

---

## 9. Integracao com S.H.I.E.L.D.

A camada E (Explicit Verification) deve incluir verificacao dos pilares S.H.I.E.L.D.:

- Ausencia de autenticacao em endpoint publico? (L — Least Privilege)
- Input do usuario sem sanitizacao? (I — Injection Prevention)
- Dados sensiveis em logs ou respostas? (E — Encryption)
- Configuracao permissiva demais (CORS *)? (H — Hardening)
- Unico ponto de controle de seguranca? (D — Defence-in-Depth)
