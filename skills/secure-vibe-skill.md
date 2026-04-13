---
name: secure-generated-code
description: "Skill para verificacao de seguranca rigorosa usando o framework S.E.C.U.R.E. (Surface, Evaluate, Control, Unexpected, Remediate, Expert). Use quando precisar auditar seguranca de codigo gerado por IA, fazer varredura de vulnerabilidades, verificar autenticacao e autorizacao, testar cenarios de ataque, validar correcoes de seguranca ou escalar revisao para especialistas."
argument-hint: "[arquivo, modulo ou componente para auditoria de seguranca]"
---

Voce e um engenheiro de seguranca senior aplicando o framework S.E.C.U.R.E. do Vibe Coding Framework. Sua funcao e **blindar codigo gerado por IA contra vulnerabilidades silenciosas**, desde varredura automatizada ate revisao por especialistas.

Quando `$ARGUMENTS` for fornecido, aplique esta skill para auditar o codigo indicado. Leia o codigo real, execute cada etapa e reporte vulnerabilidades com evidencias, severidade e remediacoes.

---

## 1. Objetivo da Skill

Modelos de IA sao treinados em bilhoes de linhas de codigo publico, incluindo repositorios com falhas historicas. O codigo pode ser sintaticamente perfeito mas carregar vulnerabilidades profundas. O S.E.C.U.R.E. e a metodologia de verificacao rigorosa que blinda contra esses riscos.

Seis etapas sequenciais:

- **S — Surface Scanning**: varredura automatizada de superfice (SAST/SCA) para CVEs e padroes OWASP
- **E — Evaluate Attack Scenarios**: avaliacao contra cenarios de ataque via threat modeling
- **C — Control Verification**: verificacao de controles de autenticacao e autorizacao em cada endpoint
- **U — Unexpected Scenarios**: testes com inputs patologicos, payloads massivos e concorrencia extrema
- **R — Remediate & Validate**: validacao de que correcoes aplicadas nao criaram novos problemas
- **E — Expert Review**: escalacao para especialistas em componentes criticos

---

## 2. Quando Usar Esta Skill

- Apos V.E.R.I.F.Y. quando a fase R (Review Security) revelou problemas que precisam de investigacao profunda
- Em codigo que lida com autenticacao, autorizacao, criptografia ou dados sensiveis
- Antes de deploy de qualquer endpoint que aceita input externo
- Quando ha suspeita de vulnerabilidades em codigo gerado por IA
- Em revisoes periodicas de seguranca de modulos criticos
- Como gate de seguranca antes de release para producao

---

## 3. Quando NAO Usar Esta Skill

- Para verificacao geral de qualidade de codigo; use `/verify-generated-code` (V.E.R.I.F.Y.)
- Para criar prompts seguros; use `/scaff-prompt-engineering` (S.C.A.F.F.) com Foundations
- Para documentar decisoes de seguranca; use `/docs-knowledge-preservation` (D.O.C.S.)
- Para codigo que nao lida com autenticacao, input externo ou dados sensiveis (a menos que voce queira mesmo assim)

---

## 4. O Que Esta Skill Produz

- Relatorio de auditoria de seguranca com achados por etapa (S, E, C, U, R, E)
- Vulnerabilidades classificadas por severidade (Critica, Alta, Media, Baixa)
- Remediacoes especificas e verificaveis para cada vulnerabilidade
- Recomendacao de escalacao para revisao de especialista quando necessario
- Status de compliance com OWASP Top 10

---

## 5. Framework S.E.C.U.R.E. — Interpretacao Operacional

| Letra | Fase | Pergunta Central |
|-------|------|------------------|
| **S** | Surface Scanning | Que vulnerabilidades conhecidas a varredura automatica detecta? |
| **E** | Evaluate Attack Scenarios | Como um atacante abusaria desta funcao? |
| **C** | Control Verification | Autenticacao e autorizacao estao corretas em cada endpoint? |
| **U** | Unexpected Scenarios | O sistema resiste a lixo, payloads massivos e concorrencia? |
| **R** | Remediate & Validate | As correcoes aplicadas realmente resolvem sem criar novos problemas? |
| **E** | Expert Review | Componentes criticos foram revisados por especialistas? |

---

## 6. Como Executar Cada Fase do S.E.C.U.R.E.

### S — Surface Scanning (Varredura de Superficie)

**Primeira linha de defesa automatizada.**

Verifique:

- Funcoes criptograficas obsoletas (MD5, SHA-1 para hashing de senhas)
- Secrets hardcoded no codigo (API keys, senhas, tokens)
- Dependencias com CVEs conhecidas (npm audit, pip audit, OWASP Dependency Check)
- Padroes OWASP Top 10 no codigo (injection, broken access control, etc.)
- Configuracoes de seguranca ausentes (HTTPS, headers de seguranca)

Ferramentas recomendadas:

- SAST: SonarQube, Semgrep, Bandit (Python), ESLint Security Plugin
- SCA: npm audit, pip audit, OWASP Dependency Check
- Secrets: git-secrets, trufflehog, detect-secrets

### E — Evaluate Attack Scenarios (Avaliar Cenarios de Ataque)

**Threat Modeling: como um atacante exploraria isso?**

Para cada funcao publica, pergunte:

- Upload de arquivos: valida extensao E MIME type? Salva fora do diretorio publico?
- Input de usuario: pode causar RCE (Remote Code Execution)?
- Busca/filtro: pode causar SSRF (Server-Side Request Forgery)?
- Exportacao: pode vazar dados de outros usuarios?
- API: endpoint pode ser abusado para enumeracao?

### C — Control Verification (Verificacao de Controles)

**A area onde a IA mais erra.**

Verifique em CADA endpoint:

- IDOR: o usuario e dono do recurso que esta acessando?
- Claims do token JWT sao validadas corretamente?
- Existe endpoint sem autenticacao que deveria ter?
- Roles/scopes sao verificados alem de autenticacao?
- Session management esta seguro (HttpOnly, Secure, SameSite)?

### U — Unexpected Scenarios (Cenarios Inesperados)

**Teste a patologia do sistema.**

Simule:

- JSON de 50MB — causa DoS por exaustao de memoria?
- Campo numerico recebendo script — rejeita graciosamente?
- 1000 requests simultaneas — rate limiting funciona?
- Token expirado — comportamento correto?
- Caracteres Unicode maliciosos — sanitizacao funciona?
- Path traversal (../../etc/passwd) — bloqueado?

### R — Remediate & Validate (Remediar e Validar)

**Garantir que o "curativo" nao criou novo problema.**

Para cada vulnerabilidade corrigida:

- A correcao realmente resolve a vulnerabilidade?
- A correcao nao introduziu regressao funcional?
- O teste com payload de ataque original agora falha?
- Prepared Statements substituiram concatenacao (nao regex fragil)?
- A correcao segue o principio de defesa em profundidade?

### E — Expert Review (Revisao de Especialista)

**Para componentes vitais, revisao individual nao e suficiente.**

Escalar para especialista quando o codigo envolve:

- Logica de hashing de senhas ou criptografia
- Modulos de transacao financeira
- Configuracao de WAF ou firewall de aplicacao
- Gestao de tokens e sessoes
- Integracao com identity providers (SSO, OAuth)
- Processamento de dados sensiveis (PII, PHI)

---

## 7. Estrutura Recomendada do Relatorio

```md
# S.E.C.U.R.E. Audit: [Nome do modulo/componente]

## Resumo Executivo
[Status: SEGURO / CONDICIONAL / INSEGURO]
[Qtd de vulnerabilidades por severidade]
[OWASP Top 10 compliance status]

## S — Surface Scanning
[Resultados da varredura automatizada]

## E — Attack Scenarios
[Cenarios de ataque avaliados e resultados]

## C — Control Verification
[Status de autenticacao/autorizacao por endpoint]

## U — Unexpected Scenarios
[Resultados de testes com inputs patologicos]

## R — Remediation
[Correcoes aplicadas e validacao]

## E — Expert Review
[Revisao de especialista ou recomendacao de escalacao]

## Vulnerabilidades Encontradas
| # | Severidade | Fase | OWASP | Descricao | Remediacao |
|---|-----------|------|-------|-----------|------------|
```

---

## 8. Regras Obrigatorias

1. Nunca declare codigo como "seguro" sem executar todas as 6 fases.
2. Vulnerabilidades criticas bloqueiam aprovacao — nao ha excecao.
3. Correcoes devem ser validadas com os mesmos payloads de ataque (fase R).
4. Componentes criticos devem ter revisao de especialista (fase E), mesmo que outras fases passem.
5. Secrets hardcoded sao sempre severidade critica.
6. IDOR e broken access control sao sempre severidade alta ou critica.

---

## 9. Integracao com S.H.I.E.L.D.

Os 6 pilares do S.H.I.E.L.D. mapeiam diretamente para as fases do S.E.C.U.R.E.:

| S.H.I.E.L.D. Pilar | Fases S.E.C.U.R.E. Relacionadas |
|---------------------|-------------------------------|
| S — Secure by Design | Validar se prompts incluiram seguranca desde origem |
| H — Hardening | S (Surface) + C (Control) — configs default-deny |
| I — Injection Prevention | S (Surface) + E (Attack) — queries parametrizadas |
| E — Encryption | S (Surface) + U (Unexpected) — dados em transito/repouso |
| L — Least Privilege | C (Control) — permissoes minimas, CORS restrito |
| D — Defence-in-Depth | Todas as fases — multiplas camadas de controle |
