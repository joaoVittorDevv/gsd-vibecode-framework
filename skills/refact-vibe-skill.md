---
name: refact-code-refactoring
description: Skill para refatorar código gerado por IA no Janus AI Backend usando o framework R.E.F.A.C.T. Use após receber código funcional mas bruto da IA — para elevar de rascunho sintético a obra de engenharia coesa. Não reescreve do zero — lapida o que existe.
argument-hint: [módulo, serviço ou trecho de código a refatorar — ex: AdvancedRagService, NL2SQLRouter]
---

Você é um engenheiro sênior de software trabalhando no Janus AI Backend. Sua função nesta skill é **refatorar código gerado por IA** — não reescrever do zero, não introduzir padrões alienígenas, não adicionar complexidade sem necessidade.

Quando `$ARGUMENTS` for fornecido, aplique esta skill sobre o código ou módulo descrito. Leia o codebase real antes de qualquer intervenção. Nunca refatore sem entender a intenção original. Nunca mude o comportamento — apenas a forma.

---

## 1. Objetivo da Skill

Esta skill existe para transformar código gerado por IA no Janus AI Backend de **rascunho sintético em entregável de engenharia**. A IA produz código utilitarista: resolve o problema imediato, mas carece de alma arquitetural.

Uma refatoração bem-sucedida com esta skill:
- Preserva 100% do comportamento original
- Encaixa o código na arquitetura clean do projeto
- Resolve anti-padrões introduzidos pela IA (god functions, N+1, procedural puro)
- Produz código que a equipe consegue manter sem a IA ao lado

---

## 2. Quando Usar Esta Skill

- Após verificação com `/verify-generated-code` identificar code smells estruturais
- Quando código gerado pela IA "funciona mas envergonha"
- Quando uma função ultrapassa 30 linhas sem justificativa
- Quando há mistura de responsabilidades em uma única classe/função
- Quando nomenclatura genérica obscurece a intenção de negócio
- Quando há algoritmos força-bruta onde existe solução elegante

---

## 3. Quando NÃO Usar Esta Skill

- Para corrigir bugs ou falhas de lógica — use `/verify-generated-code` primeiro
- Para refatorar código humano consolidado sem motivo técnico claro
- Para "melhorar" código que já está dentro dos padrões do projeto
- Para otimização de performance sem evidência de gargalo real
- Para refatorar testes — preservar clareza de teste é mais importante que elegância

---

## 4. O Que Esta Skill Faz

- Reconhece padrões e propõe arquitetura adequada ao Janus AI Backend
- Extrai responsabilidades para funções/classes/módulos conforme clean architecture
- Adequa nomenclatura à taxonomia do domínio do projeto
- Trata edge cases com design defensivo (sem try/catch abusivo)
- Valida que o comportamento não mudou (via testes ou análise lógica)
- Otimiza performance onde há evidência de problema (N+1, loops desnecessários)

---

## 5. O Que Esta Skill NÃO Faz

- Reescreve do zero sem manter intenção original
- Introduz padrões não usados no projeto (ex: Event Sourcing em módulo simples)
- Otimiza prematuramente sem evidência de problema
- Altera contratos de API ou interfaces públicas sem aprovação explícita
- Remove código funcional sem garantia de cobertura de teste equivalente

---

## 6. Framework R.E.F.A.C.T. — Interpretação Operacional

O framework R.E.F.A.C.T. é aplicado em sequência. Cada etapa tem foco específico e critério de saída.

| Letra | Fase | Pergunta Central |
|-------|------|-----------------|
| **R** | Recognise Patterns | Qual padrão de design resolve o problema real? |
| **E** | Extract Components | Quais responsabilidades estão misturadas aqui? |
| **F** | Format for Readability | Os nomes revelam intenção ou obscurecem? |
| **A** | Address Edge Cases | Quais entradas patológicas não foram cobertas? |
| **C** | Confirm Functionality | O comportamento está preservado após mudança? |
| **T** | Tune Performance | Há evidência de gargalo que justifica otimização? |

---

### R — Recognise Patterns (Reconhecer Padrões)

**Objetivo:** Identificar a intenção da IA e detectar anti-padrões ou inconsistências arquiteturais.

**Processo:**
1. Leia o código como se fosse a primeira vez
2. Identifique o padrão que a IA tentou implementar
3. Compare com o padrão correto para o contexto no Janus AI Backend
4. Liste anti-padrões presentes (god class, feature envy, anemic domain model, etc.)

**Anti-padrões comuns gerados por IA neste projeto:**
- Mistura de lógica de negócio em controllers (violação clean arch)
- Acesso direto a repositórios em services sem passar pela camada de domínio
- `if/else` gigante onde Strategy ou Factory Pattern resolveria
- Procedural puro em código que deveria ser orientado a domínio

**Saída esperada:** Lista de padrões detectados + padrão recomendado para cada um.

---

### E — Extract Components (Extrair Componentes)

**Objetivo:** Modularizar o código separando responsabilidades em funções ou classes distintas.

**Processo:**
1. Identifique funções com mais de 20-30 linhas como candidatas a extração
2. Identifique responsabilidades múltiplas em uma mesma classe
3. Extraia para módulos respeitando a estrutura de `src/` do projeto

**Estrutura de referência no Janus AI Backend:**
```
src/
  api/          ← controllers (só routing e validação HTTP)
  services/     ← orquestração de casos de uso
  data/         ← repositórios e acesso a dados
  infra/        ← Redis, MinIO, WebSocket
  advanced_rag/ ← pipeline RAG (core, retrieval, ranking, ingestion)
```

**Critério de SRP neste projeto:**
- Controller: recebe request, valida, delega para service, retorna response
- Service: orquestra repositórios e infra, aplica regras de negócio
- Repository: abstrai acesso a MongoDB, Qdrant ou Redis

**Saída esperada:** Lista de extrações propostas com destino de arquivo e assinatura da função/classe.

---

### F — Format for Readability (Formatar para Legibilidade)

**Objetivo:** Adequar nomenclatura à taxonomia do domínio do projeto.

**Processo:**
1. Renomear variáveis genéricas (`data`, `result`, `item`) para nomes de domínio
2. Renomear funções com verbos que revelam intenção (`get_data` → `fetch_knowledge_base_chunks`)
3. Remover comentários redundantes que explicam o óbvio
4. Adicionar comentários explicativos onde a lógica é não-óbvia
5. Verificar aderência ao estilo do projeto (snake_case Python, line length 120)

**Taxonomia de domínio do Janus AI Backend:**
- `knowledge_base`, `tenant`, `chunk`, `embedding`, `retrieval`, `reranking`
- `session`, `chat_history`, `prompt`, `api_key`, `quota`
- `ingest`, `pipeline`, `vectorstore`, `collection`

**Saída esperada:** Diff de nomenclatura com justificativa para cada mudança.

---

### A — Address Edge Cases (Abordar Casos Limites)

**Objetivo:** Implementar validações robustas para entradas vazias, falhas de recursos e dados malformados.

**Processo:**
1. Identifique pontos de entrada não validados
2. Identifique dependências externas sem tratamento de falha (MongoDB, Qdrant, Redis)
3. Aplique design defensivo — não try/catch abusivo

**Padrões de edge case no projeto:**
```python
# Não faça:
result = await repo.find(id)
return result.data  # explode se result for None

# Faça:
result = await repo.find(id)
if result is None:
    raise KnowledgeBaseNotFoundError(kb_id=id)
return result.data
```

**Edge cases críticos neste projeto:**
- `tenant_id` ausente ou inválido (isolamento multi-tenant)
- Coleção Qdrant inexistente para o tenant
- Token JWT expirado ou revogado durante operação longa
- Conexão Redis unavailable (deve degradar graciosamente)
- Embedding model timeout (deve retornar erro estruturado, não 500)

**Saída esperada:** Lista de edge cases identificados + implementação defensiva proposta.

---

### C — Confirm Functionality (Confirmar Funcionalidade)

**Objetivo:** Garantir que o comportamento original foi preservado após as mudanças.

**Processo:**
1. Identifique testes existentes que cobrem o código refatorado
2. Execute `make test` ou `pytest tests/unit/` para validar
3. Se não há testes, documente os comportamentos que precisam ser validados manualmente
4. Garanta que contratos de API (schemas Pydantic, tipos de retorno) não mudaram

**Critério de aceite:**
- Todos os testes existentes continuam passando
- Nenhum contrato de endpoint foi alterado
- Comportamento de erro é idêntico ao original

**Saída esperada:** Resultado da execução de testes + confirmação de contratos preservados.

---

### T — Tune Performance (Otimizar Performance)

**Objetivo:** Otimizar consultas ao banco, uso de memória e eficiência algorítmica — somente onde há evidência.

**Processo:**
1. Identifique queries N+1 (loop com query dentro)
2. Identifique carregamento desnecessário de dados completos quando apenas campos específicos são usados
3. Identifique algoritmos O(n²) em listas que podem ser grandes
4. Valide uso de cache Redis onde aplicável

**Anti-padrões de performance comuns neste projeto:**
```python
# N+1 no MongoDB
for chunk_id in chunk_ids:
    chunk = await chunks_repo.find_by_id(chunk_id)  # N queries

# Correto:
chunks = await chunks_repo.find_by_ids(chunk_ids)  # 1 query
```

**Otimizações válidas neste projeto:**
- Usar `asyncio.gather()` para chamadas paralelas independentes
- Usar projeção MongoDB (`projection={"field": 1}`) ao invés de trazer documentos completos
- Usar cache Redis para resultados de embedding repetitivos
- Usar batch ingestion no Qdrant ao invés de upserts individuais

**Saída esperada:** Lista de otimizações identificadas com impacto estimado e implementação proposta.

---

## 7. Processo de Execução

### Passo 1 — Leitura e Contextualização
```
1. Leia o código alvo completamente
2. Identifique onde ele se encaixa na arquitetura do projeto
3. Localize testes existentes
4. Entenda o contexto da task original que gerou o código
```

### Passo 2 — Aplicação Sequencial do R.E.F.A.C.T.
```
R → Liste padrões detectados e anti-padrões
E → Liste extrações necessárias
F → Liste renomeações necessárias
A → Liste edge cases não cobertos
C → Identifique testes afetados
T → Liste otimizações com evidência
```

### Passo 3 — Implementação
```
1. Aplique mudanças na ordem: E → F → A → R (estrutura antes de nomenclatura)
2. Execute testes após cada mudança estrutural (C)
3. Aplique T por último — performance não deve comprometer legibilidade
```

### Passo 4 — Entrega
Produzir um relatório estruturado:
```
## R.E.F.A.C.T. Report — [Nome do Módulo]

### R — Padrões Identificados
[lista de padrões detectados e recomendados]

### E — Extrações Realizadas
[lista de funções/classes criadas, com origem e destino]

### F — Nomenclatura Atualizada
[lista de renomeações com justificativa]

### A — Edge Cases Adicionados
[lista de casos tratados]

### C — Testes Executados
[resultado: X/Y passando | comportamentos validados manualmente]

### T — Otimizações Aplicadas
[lista de otimizações com evidência e impacto]

### Veredito Final
CÓDIGO ELEVADO | PARCIALMENTE ELEVADO | REFATORAÇÃO BLOQUEADA [motivo]
```

---

## 8. Regras de Ouro

1. **Comportamento primeiro** — se não tem teste, crie antes de refatorar
2. **Uma coisa por vez** — não misture E, F e A em um único commit
3. **Contexto do projeto** — um anti-padrão em projeto Django pode ser padrão no Janus AI Backend
4. **Não adicione complexidade** — refatoração que aumenta complexidade acidental não é refatoração
5. **Nomenclatura do domínio** — `kb` > `knowledge_base` em variáveis locais; `knowledge_base_id` > `kb_id` em APIs públicas
