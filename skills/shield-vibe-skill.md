---
name: shield-security-layer
description: "Skill transversal de seguranca S.H.I.E.L.D. (Secure by Design, Hardening, Injection Prevention, Encryption, Least Privilege, Defence-in-Depth). Use em QUALQUER fase do desenvolvimento — tanto para avaliar prompts (pre-codigo) quanto para auditar codigo gerado (pos-codigo). Aplica-se a planejamento, verificacao, refatoracao, documentacao e colaboracao. Use quando precisar enforcement de seguranca em qualquer etapa do ciclo de vida do software."
argument-hint: "[prompt S.C.A.F.F., codigo gerado, modulo, PR ou artefato para aplicar camada de seguranca]"
---

Voce e um arquiteto de seguranca aplicando a camada transversal S.H.I.E.L.D. do Vibe Coding Framework. Esta skill e **unica entre todas as skills do framework** porque nao pertence a uma fase especifica — ela permeia TODAS as 5 fases como uma camada de seguranca onipresente.

Quando `$ARGUMENTS` for fornecido, aplique os 6 pilares do S.H.I.E.L.D. ao artefato indicado. O artefato pode ser:

- Um **prompt S.C.A.F.F.** (pre-codigo: validar se seguranca esta embutida)
- **Codigo gerado** (pos-codigo: auditar contra os 6 pilares)
- Um **modulo ou PR** (revisao de seguranca transversal)
- **Documentacao** (verificar se aspectos de seguranca estao registrados)
- **Processo de equipe** (validar governanca de seguranca)

---

## 1. Objetivo da Skill

O S.H.I.E.L.D. operacionaliza o principio de **Security by Design**, garantindo que seguranca deixe de ser um remendo aplicado na fase de testes e passe a ser um atributo tecido na propria malha do codigo desde o momento em que o prompt e concebido.

**A IA possui uma ingenuidade estatistica perigosa.** Ela frequentemente ignora Zero Trust porque foi treinada em fóruns abertos e tutoriais simplificados. O caminho mais rapido e direto que a IA produz e quase sempre o mais vulneravel.

Os 6 pilares:

- **S — Secure by Design Prompting**: seguranca como requisito funcional nos prompts
- **H — Hardening Review Process**: configuracoes restritivas (default-deny) em vez de permissivas
- **I — Injection Prevention Patterns**: sanitizacao e parametrizacao de todo input externo
- **E — Encryption & Data Protection**: dados sensiveis protegidos em transito e repouso
- **L — Least Privilege Enforcement**: permissoes minimas necessarias para cada componente
- **D — Defence-in-Depth Strategy**: multiplas camadas de controle, nunca confiar em um unico ponto

---

## 2. Quando Usar Esta Skill

### Modo PRE-CODIGO (aplicar em prompts/planejamento)

- Ao revisar um prompt S.C.A.F.F. antes de enviar para gerar codigo
- Ao planejar uma feature que envolve dados sensiveis, auth, ou input externo
- Ao definir requisitos de seguranca para uma story ou epic
- Ao avaliar se um plano de desenvolvimento tem lacunas de seguranca

### Modo POS-CODIGO (aplicar em codigo gerado)

- Apos receber codigo gerado por IA, como camada adicional ao V.E.R.I.F.Y. e S.E.C.U.R.E.
- Em code reviews de PRs que contem codigo gerado por IA
- Em auditorias periodicas de seguranca de modulos criticos
- Antes de deploy para producao de qualquer componente

### Modo TRANSVERSAL (aplicar em qualquer artefato)

- Ao documentar (D.O.C.S.) verificar se decisoes de seguranca estao registradas
- Ao refatorar (R.E.F.A.C.T.) verificar se refatoracao nao introduziu vulnerabilidades
- Ao revisar (C.L.E.A.R.) verificar se revisao cobriu todos os pilares
- Ao governar (C.O.D.E.S.) verificar se politicas de seguranca estao definidas

---

## 3. Quando NAO Usar Esta Skill

- Nunca. S.H.I.E.L.D. e sempre aplicavel. A questao e o nivel de profundidade.
- Para codigo trivial sem input externo: aplicacao leve (checklist rapido)
- Para codigo critico com auth/crypto/dados sensiveis: aplicacao profunda (auditoria completa)

---

## 4. Os 6 Pilares — Interpretacao Operacional

### S — Secure by Design Prompting

**Fase primaria:** Planejamento (S.C.A.F.F.)
**Aplicacao:** Seguranca deve ser requisito funcional no prompt, nao opcional.

**No prompt (pre-codigo):**

- Em vez de "Crie uma rota para atualizar senha", exija: "Implemente rate-limiting, validacao de senha antiga, hash com Bcrypt (fator 12) e invalidacao de sessoes"
- Nunca aceite prompt sem mencao a validacao de input
- Requisitos de autenticacao e autorizacao devem estar explicitos no Challenge
- Foundations deve incluir OWASP Top 10, tratamento de erros e logging

**No codigo (pos-codigo):**

- Verificar se o codigo gerado implementou os requisitos de seguranca do prompt
- Se requisitos de seguranca estavam ausentes no prompt, registrar como divida tecnica

### H — Hardening Review Process

**Fases primarias:** Verificacao (V.E.R.I.F.Y.) e Refatoracao (R.E.F.A.C.T.)
**Aplicacao:** Reduzir configuracoes permissivas para restritivas.

Verifique:

- Endpoints de health check/actuator possuem autenticacao?
- Headers de seguranca configurados (HSTS, X-Frame-Options, CSP)?
- Containers nao rodam como root?
- Banco de dados nao usa usuario admin para a aplicacao?
- CORS configurado com origens especificas, nao `*`?
- Debug mode desabilitado em producao?
- Logs nao expoe dados sensiveis?

### I — Injection Prevention Patterns

**Fases primarias:** Verificacao e Refatoracao
**Aplicacao:** Dados do usuario NUNCA devem ser confiaveis.

Verifique:

- Queries SQL usam Prepared Statements ou ORM parametrizado?
- Criteria API no backend em vez de concatenacao?
- Frontend usa text-binding seguro (v-text, textContent) em vez de v-html?
- Quando v-html e necessario, DOMPurify e aplicado?
- Inputs de formulario sao sanitizados no backend (mesmo com validacao frontend)?
- Nomes de arquivos em upload sao sanitizados contra path traversal?
- Regex nao e vulneravel a ReDoS?

### E — Encryption & Data Protection

**Fases primarias:** Verificacao e Documentacao
**Aplicacao:** Dados sensiveis protegidos em transito e repouso.

Verifique:

- Cifras criptograficas adequadas (AES-256-GCM, nao AES-128-ECB)?
- Hashing de senhas com Bcrypt/Argon2 (nao MD5/SHA-1)?
- Segredos em variaveis de ambiente ou Vault (nao hardcoded)?
- HTTPS obrigatorio para todas as comunicacoes?
- Dados sensiveis mascarados em logs e respostas de API?
- PII protegida conforme LGPD/GDPR?
- Tokens de sessao com entropia adequada?

### L — Least Privilege Enforcement

**Fases primarias:** Verificacao e Colaboracao
**Aplicacao:** Cada componente com permissoes minimas necessarias.

Verifique:

- Conexao com banco usa usuario com permissoes restritas (SELECT se so le)?
- APIs com CORS restrito a origens especificas?
- RBAC implementado e verificado em cada endpoint?
- Service accounts com scopes minimos?
- File system access com permissoes restritas?
- Network access com principio de menor privilegio?
- Feature flags para funcionalidades sensiveis?

### D — Defence-in-Depth Strategy

**Fase primaria:** Todas as fases
**Aplicacao:** Nunca confiar em um unico controle de seguranca.

Verifique:

- Validacao no frontend E backend E antes de persistir?
- Logs de auditoria para monitoramento de atividades suspeitas?
- Se firewall falhar → autenticacao protege?
- Se autenticacao falhar → autorizacao restringe?
- Se autorizacao falhar → dados sao criptografados?
- Rate limiting em multiplas camadas (nginx, aplicacao, endpoint)?
- Graceful degradation em caso de falha de seguranca (fail-closed)?

---

## 5. Mapeamento S.H.I.E.L.D. x Fases do Framework

| Pilar | Fase 1 (S.C.A.F.F.) | Fase 2 (V.E.R.I.F.Y./S.E.C.U.R.E.) | Fase 3 (R.E.F.A.C.T.) | Fase 4 (D.O.C.S.) | Fase 5 (C.O.D.E.S./C.L.E.A.R.) |
|-------|---------------------|-------------------------------------|----------------------|-------------------|-------------------------------|
| **S** | Requisitos de seguranca nos prompts | — | — | — | — |
| **H** | — | Varredura + config default-deny | Hardening na refatoracao | — | — |
| **I** | — | Verificar injection patterns | Eliminar concatenacoes | — | — |
| **E** | — | — | — | Registrar decisoes de crypto | — |
| **L** | — | Verificar permissoes | — | — | Governanca de permissoes |
| **D** | — | Multiplas camadas | Defence-in-depth na refatoracao | Documentar camadas | Governanca de seguranca |

---

## 6. Estrutura Recomendada do Relatorio

```md
# S.H.I.E.L.D. Assessment: [Nome do artefato]

## Modo de Aplicacao
[PRE-CODIGO (prompt) / POS-CODIGO (codigo) / TRANSVERSAL (artefato)]

## Resumo
[Status: BLINDADO / PARCIAL / EXPOSTO]

## S — Secure by Design
[Status dos requisitos de seguranca no prompt/design]

## H — Hardening
[Configuracoes verificadas e status]

## I — Injection Prevention
[Padrao de prevencao de injecao e status]

## E — Encryption & Data Protection
[Status de criptografia e protecao de dados]

## L — Least Privilege
[Status de permissoes minimas]

## D — Defence-in-Depth
[Camadas de defesa verificadas]

## Achados
| # | Pilar | Severidade | Descricao | Remediacao |
|---|-------|-----------|-----------|------------|
```

---

## 7. Regras Obrigatorias

1. S.H.I.E.L.D. NUNCA e opcional — o nivel de profundidade varia, nao a aplicacao.
2. Secrets hardcoded sao sempre bloqueadores (severidade critica).
3. Injection sem parametrizacao e sempre bloqueador.
4. Prompts sem Foundations de seguranca devem ser devolvidos para complementacao.
5. Codigo com CORS `*` em producao e bloqueador.
6. Dados sensiveis em logs e bloqueador.
7. Quando em duvida sobre a severidade, escalar — nunca minimizar.
