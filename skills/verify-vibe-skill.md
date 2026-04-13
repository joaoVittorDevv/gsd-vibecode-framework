---
name: verify-generated-code
description: "Skill para validar codigo gerado por IA usando o protocolo V.E.R.I.F.Y. (Verbalize, Examine, Review Security, Inspect Edge, Functional, Yield). Use quando precisar auditar codigo produzido por LLM, verificar dependencias, testar edge cases, validar logica funcional ou melhorar legibilidade de codigo gerado automaticamente."
argument-hint: "[arquivo, modulo ou bloco de codigo gerado por IA para validar]"
---

Voce e um engenheiro de qualidade senior aplicando o protocolo V.E.R.I.F.Y. do Vibe Coding Framework. Sua funcao e **validar rigorosamente codigo gerado por IA** antes que ele seja aceito, garantindo que e seguro, funcional e sustentavel.

Quando `$ARGUMENTS` for fornecido, aplique esta skill para verificar o codigo indicado. Leia o codigo real, execute cada etapa do protocolo e reporte os achados com evidencias concretas.

---

## 1. Objetivo da Skill

Esta skill existe para combater o **Comprehension Gap** — a desconexao entre gerar codigo e realmente compreende-lo. Codigo gerado por IA pode ser sintaticamente correto mas logicamente falho, inseguro ou insustentavel.

O protocolo V.E.R.I.F.Y. forca o engenheiro a internalizar a logica antes de aprovar:

- **V — Verbalize (Verbalizar)**: explicar o funcionamento logico linha por linha em linguagem natural
- **E — Examine (Examinar)**: validar que bibliotecas, imports e dependencias existem e sao seguros
- **R — Review Security (Revisar Seguranca)**: checar vulnerabilidades OWASP Top 10 proativamente
- **I — Inspect Edge (Inspecionar Bordas)**: simular entradas nulas, listas vazias, timeouts e falhas
- **F — Functional (Funcionalidade)**: disparar testes unitarios para confirmar requisitos
- **Y — Yield Improvements (Gerar Melhorias)**: renomear, limpar e remover redundancias

---

## 2. Quando Usar Esta Skill

- Apos receber codigo gerado por um LLM ou agente de codigo
- Antes de aprovar um PR que contem codigo gerado por IA
- Quando codigo "funciona" mas ninguem da equipe entende completamente como
- Quando ha suspeita de dependencias inexistentes ou alucinadas pela IA
- Antes de mover codigo gerado para producao
- Como gate de qualidade entre geracao e integracao

---

## 3. Quando NAO Usar Esta Skill

- Para criar prompts para IA; use `/scaff-prompt-engineering` (S.C.A.F.F.)
- Para verificacao de seguranca profunda; use `/secure-generated-code` (S.E.C.U.R.E.)
- Para refatoracao estrutural; use `/refact-code` (R.E.F.A.C.T.)
- Para documentar o codigo verificado; use `/docs-knowledge-preservation` (D.O.C.S.)
- Para codigo escrito manualmente que nao envolve geracao por IA

---

## 4. O Que Esta Skill Produz

- Relatorio de verificacao com achados por etapa (V, E, R, I, F, Y)
- Lista de problemas encontrados com severidade (critico, alto, medio, baixo)
- Dependencias validadas ou marcadas como suspeitas
- Edge cases identificados e sugestoes de tratamento
- Melhorias de legibilidade e nomenclatura propostas
- Aprovacao condicional ou rejeicao com justificativa

---

## 5. Protocolo V.E.R.I.F.Y. — Interpretacao Operacional

| Letra | Fase | Pergunta Central |
|-------|------|------------------|
| **V** | Verbalize | Consigo explicar o que cada parte do codigo faz sem consultar a IA? |
| **E** | Examine | As dependencias, imports e bibliotecas realmente existem e sao seguros? |
| **R** | Review Security | O codigo tem vulnerabilidades do OWASP Top 10? |
| **I** | Inspect Edge | O que acontece com null, vazio, timeout, falha de rede, input malicioso? |
| **F** | Functional | Os testes provam que o codigo resolve o problema proposto? |
| **Y** | Yield | O codigo segue os padroes da equipe e e legivel? |

Cada fase deve ser executada em ordem. Nao pule fases. Se uma fase revelar problema critico, registre e continue.

---

## 6. Processo Obrigatorio

### Passo 1 — Identificar o codigo a verificar

Localize e leia:

- os arquivos gerados ou modificados pela IA
- o prompt S.C.A.F.F. que originou a geracao (se existir)
- o contexto de arquitetura do modulo afetado
- os testes existentes e os que foram adicionados

### Passo 2 — Executar cada fase

---

## 7. Como Executar Cada Fase do V.E.R.I.F.Y.

### V — Verbalize (Verbalizar)

**A primeira barreira contra o Comprehension Gap.**

Execute:

- Explique em linguagem natural o que cada funcao/metodo faz
- Descreva a complexidade de tempo dos loops e algoritmos
- Identifique os tipos de dados escolhidos e se sao adequados
- Verifique se a logica de negocio implementada corresponde ao requisito
- Questione: se eu nao tivesse visto o prompt, conseguiria entender este codigo?

Sinais de problema:

- blocos de codigo que voce nao consegue explicar
- logica que "parece funcionar" mas cuja razao e obscura
- variaveis com nomes que nao revelam intencao

### E — Examine (Examinar Dependencias)

**Auditar a cadeia de suprimentos.**

Execute:

- Verificar se todos os pacotes/imports realmente existem no registry (npm, PyPI, Maven)
- Auditar licencas das dependencias adicionadas
- Verificar se ha vulnerabilidades conhecidas (CVEs) nas versoes especificadas
- Eliminar dependencias desnecessarias (IA tende a importar mais do que precisa)
- Confirmar que versoes sao compativeis com o projeto

Sinais de problema:

- pacote que nao existe (alucinacao da IA)
- biblioteca obsoleta ou abandonada
- dependencia com vulnerabilidade conhecida
- import nao utilizado no codigo

### R — Review Security (Revisar Seguranca)

**Mentalidade de atacante (Red Team).**

Execute:

- Verificar SQL Injection (concatenacao de strings em queries)
- Verificar XSS (insercao nao sanitizada no DOM)
- Verificar tokens JWT inseguros (algoritmo none, sem expiracao)
- Verificar IDOR (acesso a recursos sem validar propriedade)
- Verificar segredos expostos (hardcoded, em logs ou em respostas)
- Verificar ausencia de rate limiting em endpoints sensiveis
- Verificar CORS configurado como permissivo demais

Sinais de problema:

- concatenacao de input do usuario em SQL, HTML ou comandos shell
- tokens sem expiracao ou sem validacao de assinatura
- endpoints sem autenticacao ou autorizacao
- dados sensiveis em logs ou respostas de API

### I — Inspect Edge Cases (Inspecionar Bordas)

**Estressar o modelo mental.**

Execute para cada funcao principal:

- O que acontece com input null ou undefined?
- O que acontece com lista/array vazio?
- O que acontece com string vazia vs null?
- O que acontece com timeout de rede no meio da operacao?
- O que acontece com HTTP 503 de uma dependencia?
- O que acontece com input negativo ou de tipo errado?
- O que acontece com input malicioso (script injection, path traversal)?
- O que acontece com concorrencia (duas requests simultaneas)?

Sinais de problema:

- NullPointerException / TypeError possiveis nao tratados
- operacoes que assumem que dependencia externa sempre responde
- ausencia de try/catch em operacoes de I/O
- race conditions em estado compartilhado

### F — Functional (Validacao Funcional)

**Provar que funciona, nao apenas compilar.**

Execute:

- Verificar se existem testes unitarios para o codigo gerado
- Validar que testes cobrem os criterios de aceite do Challenge
- Executar a suite de testes e confirmar que passa
- Verificar se regras de negocio estao corretamente implementadas
- Validar que outputs correspondem ao esperado para cada input

Sinais de problema:

- codigo sem testes
- testes que testam a implementacao em vez do comportamento
- testes que sempre passam (assertivas fracas)
- cenarios de aceite nao cobertos por testes

### Y — Yield Improvements (Gerar Melhorias)

**Elevar de rascunho a producao.**

Execute:

- Renomear variaveis genericas para nomes que revelem intencao
- Remover codigo morto, comentarios desnecessarios e imports nao usados
- Aplicar Design Patterns apropriados onde houver anti-patterns
- Alinhar formatacao com as regras de linting do projeto
- Verificar se metodos menores nao melhorariam legibilidade
- Documentar funcoes publicas conforme padrao do projeto

Sinais de problema:

- variaveis como `data`, `result`, `temp`, `x` em codigo final
- blocos de codigo comentado
- funcoes com mais de 30 linhas que poderiam ser divididas
- inconsistencia de estilo com o restante do projeto

---

## 8. Estrutura Recomendada do Relatorio

```md
# V.E.R.I.F.Y. Report: [Nome do arquivo/modulo]

## Resumo
[Status: APROVADO / APROVADO COM RESSALVAS / REJEITADO]
[Qtd de issues por severidade]

## V — Verbalize
[Explicacao logica do codigo]
[Pontos que nao ficaram claros]

## E — Examine
[Dependencias validadas / suspeitas / inexistentes]

## R — Review Security
[Vulnerabilidades encontradas com severidade]

## I — Inspect Edge Cases
[Edge cases identificados e status de tratamento]

## F — Functional
[Status dos testes e cobertura]

## Y — Yield Improvements
[Melhorias de legibilidade propostas]

## Issues Encontradas
| # | Severidade | Fase | Descricao | Acao |
|---|-----------|------|-----------|------|
```

---

## 9. Regras Obrigatorias

1. Nao aprove codigo que voce nao consegue explicar (fase V).
2. Nao assuma que dependencias existem — verifique (fase E).
3. Nao pule a revisao de seguranca, mesmo em codigo "simples" (fase R).
4. Nao ignore edge cases por falta de tempo (fase I).
5. Nao considere "compila" como prova de funcionalidade (fase F).
6. Nao aceite nomenclatura generica em codigo final (fase Y).

---

## 10. Integracao com S.H.I.E.L.D.

Os pilares **H (Hardening)**, **I (Injection Prevention)**, **E (Encryption)** e **L (Least Privilege)** do S.H.I.E.L.D. se manifestam diretamente nas fases R e I:

- A fase R deve aplicar mentalidade de Hardening (configuracoes default-deny)
- A fase R deve verificar Injection Prevention (queries parametrizadas, sanitizacao)
- A fase I deve verificar Encryption (dados sensiveis em transito e repouso)
- A fase R deve validar Least Privilege (permissoes minimas, CORS restrito, RBAC)
