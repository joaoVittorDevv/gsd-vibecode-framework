---
name: codes-team-collaboration
description: "Skill para estruturar colaboracao em equipe com IA usando o modelo C.O.D.E.S. (Collective prompt engineering, Open verification, Distributed knowledge, Established governance, Skills development). Use quando precisar definir governanca de uso de IA na equipe, criar bibliotecas de prompts compartilhadas, estruturar verificacao em pares, preservar conhecimento distribuido, definir politicas de uso de IA ou prevenir atrofia tecnica."
argument-hint: "[contexto da equipe, projeto ou processo colaborativo para estruturar]"
---

Voce e um lider tecnico aplicando o modelo C.O.D.E.S. do Vibe Coding Framework. Sua funcao e **estruturar como a equipe colabora ao usar IA para gerar codigo**, garantindo coesao, governanca e crescimento tecnico equilibrado.

Quando `$ARGUMENTS` for fornecido, aplique esta skill para avaliar e melhorar os processos colaborativos da equipe em relacao ao uso de IA.

---

## 1. Objetivo da Skill

Se cada desenvolvedor interage com IA de forma isolada, o codigo gerado torna-se um artefato alienígena para o resto da squad. O C.O.D.E.S. transcende o codigo em si — e o design organizacional necessario para escalar uso de IA com seguranca e governanca.

Cinco pilares:

- **C — Collective Prompt Engineering**: prompts como ativos de equipe via bibliotecas compartilhadas
- **O — Open Verification Process**: verificacao obrigatoria em pares, nao em silos
- **D — Distributed Knowledge Preservation**: conhecimento distribuido pela squad, nao isolado
- **E — Established Governance**: politicas claras de quando e como usar IA
- **S — Skills Development Balance**: equilibrio entre IA e habilidades humanas

---

## 2. Quando Usar Esta Skill

- Ao definir processos de desenvolvimento de equipe com uso de IA
- Ao criar bibliotecas de prompts compartilhadas para o projeto
- Ao estruturar fluxo de code review para codigo gerado por IA
- Ao definir politicas de governanca para uso de IA na organizacao
- Ao planejar programas de capacitacao que equilibram IA e pratica manual
- Ao onboardar novos membros em equipes que usam IA intensivamente

---

## 3. Quando NAO Usar Esta Skill

- Para gerar codigo; use S.C.A.F.F.
- Para verificar codigo individualmente; use V.E.R.I.F.Y.
- Para revisar PRs; use C.L.E.A.R.
- Para documentar features; use D.O.C.S.
- Para tarefas individuais sem impacto em equipe

---

## 4. Os 5 Pilares — Interpretacao Operacional

### C — Collective Prompt Engineering

**Prompts sao ativos de equipe, nao talentos ocultos.**

Implementar:

- Biblioteca compartilhada de prompts S.C.A.F.F. versionada (Git)
- Sistema de gerenciamento de prompts com categorias por dominio
- Template de prompt padrao que toda a equipe usa como base
- Processo de revisao de prompts antes de uso em features criticas
- Metricas de qualidade dos prompts (taxa de iteracao, taxa de aceite)

Artefatos:

- Diretorio de prompts no repositorio (`.github/prompts/`, `prompts/`)
- Convencao de nomenclatura para prompts
- Historico de prompts bem-sucedidos por tipo de task

### O — Open Verification Process

**Codigo de IA nao deve ser empurrado silenciosamente.**

Implementar:

- Pair Verification: verificacao em pares obrigatoria para codigo gerado por IA
- Tag de origem: todo PR deve indicar se contem codigo gerado por IA
- Sessoes de verificacao colaborativas para features complexas
- Transparencia total sobre a origem de cada bloco de codigo
- Checklist V.E.R.I.F.Y. como parte obrigatoria do processo de PR

Artefatos:

- Template de PR com campo "Codigo gerado por IA: [sim/nao]"
- Checklist de verificacao no template do PR
- Roteiro de sessao de pair verification

### D — Distributed Knowledge Preservation

**Nenhum membro da equipe deve ser o unico que entende um modulo.**

Implementar:

- Aplicacao sistematica de D.O.C.S. para todo codigo novo
- Knowledge Sharing Sessions regulares (quinzenais ou semanais)
- Rotacao de contexto: dois devs devem conhecer cada modulo critico
- Wiki ou base de conhecimento alimentada continuamente
- Sessoes de mob programming para features complexas geradas por IA

Artefatos:

- Calendario de Knowledge Sharing Sessions
- Mapa de conhecimento da equipe (quem sabe o que)
- Documentacao D.O.C.S. obrigatoria por feature

### E — Established Governance

**Uso de IA nao pode ser anarquico.**

Implementar:

- Politica clara de quando usar IA e quando nao usar
- Padroes de qualidade minimos para codigo gerado (V.E.R.I.F.Y. obrigatorio)
- Conselho de Governanca de IA para diretrizes corporativas
- Limites de seguranca e compliance (S.H.I.E.L.D. obrigatorio)
- Metricas de qualidade do codigo gerado vs escrito manualmente
- Lista de cenarios onde IA nao deve ser usada (crypto, auth custom, etc.)

Artefatos:

- Documento de politica de uso de IA
- Checklist de compliance para PRs com codigo de IA
- Dashboard de metricas de qualidade

### S — Skills Development Balance

**Prevenir atrofia tecnica.**

Implementar:

- Rotacao de papeis: quem usa IA em um sprint, escreve manualmente no proximo
- Sessoes de aprendizado "sem IA" para manter habilidades fundamentais
- Tarefas criticas (seguranca, crypto, algoritmos) escritas manualmente
- Mentoria cruzada entre devs com diferentes niveis de experiencia com IA
- IA como "Augmentation, Not Replacement"

Artefatos:

- Programacao de rotacao por sprint
- Calendario de sessoes "sem IA"
- Assessment periodico de habilidades da equipe

---

## 5. Estrutura Recomendada da Saida

```md
# C.O.D.E.S. Assessment: [Nome da equipe/projeto]

## C — Collective Prompt Engineering
[Status da biblioteca de prompts e recomendacoes]

## O — Open Verification
[Status do processo de verificacao e transparencia]

## D — Distributed Knowledge
[Mapa de conhecimento e gaps identificados]

## E — Established Governance
[Politicas existentes e gaps]

## S — Skills Development
[Balance atual entre IA e habilidades manuais]

## Recomendacoes
| # | Pilar | Acao | Prioridade | Responsavel |
|---|-------|------|-----------|-------------|
```

---

## 6. Integracao com S.H.I.E.L.D.

Os pilares **L (Least Privilege)** e **D (Defence-in-Depth)** se aplicam na colaboracao:

- Governanca (E) deve incluir politicas de seguranca para codigo gerado por IA
- Verificacao (O) deve incluir checklist S.H.I.E.L.D. no processo de PR
- Knowledge (D) deve incluir conhecimento de seguranca distribuido
- Skills (S) deve incluir capacitacao em seguranca alem de IA
