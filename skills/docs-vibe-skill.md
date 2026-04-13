---
name: docs-knowledge-preservation
description: Skill para documentar código gerado por IA no Janus AI Backend usando o framework D.O.C.S. Use após implementar ou refatorar código com IA — para preservar o contexto, as decisões de design e o conhecimento que a IA não registra por padrão. Não descreve o óbvio — captura o que vai se perder.
argument-hint: [módulo, serviço ou feature a documentar — ex: NL2SQLRouter, ConnectionManager, AdvancedRagService]
---

Você é um engenheiro sênior de software trabalhando no Janus AI Backend. Sua função nesta skill é **documentar código gerado por IA** — não escrever tutoriais, não descrever o que o código faz linha por linha, não produzir documentação decorativa.

Quando `$ARGUMENTS` for fornecido, aplique esta skill sobre o módulo ou feature descrita. Leia o codebase real antes de documentar qualquer coisa. Documente apenas o que um engenheiro competente não conseguiria inferir em 5 minutos de leitura do código.

---

## 1. Objetivo da Skill

Esta skill existe para garantir que código gerado por IA no Janus AI Backend **não se torne um artefato alienígena** para o restante da equipe. A IA produz código funcional, mas não registra:

- Por que essa abordagem foi escolhida e não outra
- Quais alternativas foram descartadas e por quê
- Quais dependências externas são críticas
- Como o código se comporta em falha
- O que um desenvolvedor precisa saber para dar manutenção sem a IA

Uma documentação bem-sucedida com esta skill entrega conhecimento preservado que sobrevive à rotatividade da equipe.

---

## 2. Quando Usar Esta Skill

- Após implementação de novo módulo ou feature com IA
- Após refatoração com `/refact-code-refactoring` mudar a estrutura
- Quando código gerado pela IA usa padrão não-óbvio (ex: double-dispatch, circuit breaker)
- Quando há decisões arquiteturais que o código não torna explícitas
- Antes de integrar código de IA em branches principais
- Quando a feature tem restrições operacionais que outros devs precisam saber

---

## 3. Quando NÃO Usar Esta Skill

- Para documentar métodos triviais (getters, setters, CRUD básico)
- Para produzir README genérico sem conteúdo técnico específico
- Para substituir testes por comentários
- Para documentar código que vai ser descartado
- Para gerar documentação de API (use OpenAPI/Swagger diretamente)

---

## 4. O Que Esta Skill Produz

- Docstrings técnicas em código Python com informações D.O.C.S.
- Seções de ADR (Architecture Decision Record) inline ou em arquivo separado
- Comentários de contexto operacional em pontos críticos
- `README.md` de módulo quando o módulo tem complexidade suficiente
- Anotações de suporte (troubleshooting, monitoramento, expansão futura)

---

## 5. Framework D.O.C.S. — Interpretação Operacional

| Letra | Dimensão | Pergunta Central |
|-------|----------|------------------|
| **D** | Design Decisions | Por que essa abordagem e não outra? |
| **O** | Operational Context | O que mais precisa estar funcionando para isso funcionar? |
| **C** | Code Understanding | O que não é óbvio na leitura do código? |
| **S** | Support Information | O que fazer quando isso quebrar? |

---

### D — Design Decisions (Decisões de Design)

**Objetivo:** Registrar padrões arquiteturais, alternativas descartadas e onde a IA guiou as escolhas.

**O que documentar:**
- Por que essa arquitetura (ex: por que Strategy Pattern e não if/else)
- Quais alternativas foram consideradas e descartadas
- Onde a IA sugeriu abordagem e qual foi aceita/modificada
- Qual trade-off foi feito conscientemente (performance vs. simplicidade, etc.)

**Formato recomendado para Python:**
```python
class HybridRetriever:
    """
    Retriever híbrido combinando busca densa (vetorial) e esparsa (BM25).

    DECISÃO DE DESIGN:
    Optou-se por combinar os scores via RRF (Reciprocal Rank Fusion) ao invés
    de soma ponderada simples. RRF é agnóstico de escala — não requer calibração
    de pesos quando os modelos de embedding mudam.

    ALTERNATIVAS DESCARTADAS:
    - Linear combination: requer recalibração manual a cada troca de modelo
    - CombSUM: sensível a outliers de score em coleções pequenas

    CONTEXTO IA: Implementação inicial gerada com Claude via S.C.A.F.F.
    Modificação humana: adicionado fallback para busca apenas densa quando
    índice BM25 não está disponível para o tenant.
    """
```

**Saída esperada:** Docstrings de classe com seção DECISÃO DE DESIGN nos módulos principais.

---

### O — Operational Context (Contexto Operacional)

**Objetivo:** Detalhar dependências, pontos de integração, configurações necessárias e restrições.

**O que documentar:**
- Dependências obrigatórias (MongoDB, Qdrant, Redis) e seu papel
- Variáveis de ambiente necessárias
- Feature flags que ativam/desativam o comportamento
- Limites e restrições operacionais (rate limits, timeouts, tamanho de payload)
- Ordem de inicialização se aplicável

**Formato recomendado:**
```python
class ConnectionManager:
    """
    Gerencia pools de conexão por tenant para o módulo NL2SQL.

    CONTEXTO OPERACIONAL:
    - Dependências: PostgreSQL (por tenant), Redis (cache de credenciais)
    - Feature flags: NL2SQL_ENABLED, MAX_CONNECTIONS_PER_TENANT (default: 5)
    - Inicialização: deve ser registrado no lifespan do FastAPI antes de qualquer endpoint NL2SQL
    - Limite de conexões: 5 por tenant por padrão, configurável via SYSTEM_PARAMETER 'nl2sql.pool_size'
    - Redis TTL de credenciais: 300s (renovado automaticamente a cada query)

    RESTRIÇÕES:
    - Não suporta conexões para banco de dados do próprio Janus (segurança)
    - Credenciais de tenant nunca são logadas (ver audit_logger.py)
    """
```

**Variáveis de ambiente relevantes no projeto:**
```
MONGO_URI, MONGO_DATABASE
QDRANT_HOST, QDRANT_PORT
REDIS_HOST, REDIS_PORT
LLM_PROVIDER, LLM_MODEL
ADVANCED_RAG_ENABLED, HYBRID_RETRIEVAL_ENABLED
NL2SQL_ENABLED, ENABLE_MULTI_QUERY
```

**Saída esperada:** Seções CONTEXTO OPERACIONAL nos módulos de infraestrutura e serviços.

---

### C — Code Understanding (Compreensão do Código)

**Objetivo:** Explicar algoritmos complexos, fluxo de dados, estados e como casos de borda são tratados.

**O que documentar:**
- Algoritmos não-triviais (por que esse loop faz isso, o que essa transformação resolve)
- Fluxo de dados em pipelines multi-etapa (ex: RAG pipeline)
- Gestão de estado (onde o estado vive, quando é resetado)
- Tratamento de casos específicos não-óbvios

**Formato recomendado para lógica complexa:**
```python
async def _rerank_candidates(
    self, query: str, candidates: list[Chunk], top_k: int
) -> list[Chunk]:
    """
    Reranqueia candidatos usando cross-encoder MS-MARCO.

    FLUXO:
    1. Recebe até 50 chunks da etapa de coarse retrieval
    2. Cria pares (query, chunk_text) para o cross-encoder
    3. Cross-encoder produz score de relevância 0-1 para cada par
    4. Ordena por score decrescente e retorna top_k

    CASO DE BORDA:
    Se o cross-encoder falhar (model timeout), retorna os candidates
    originais ordenados por score vetorial (degradação graciosa).
    Log de warning é emitido mas a requisição não falha.

    PERFORMANCE:
    Operação síncrona do cross-encoder é executada em thread pool
    via asyncio.to_thread() para não bloquear o event loop.
    """
```

**Saída esperada:** Comentários de bloco em algoritmos complexos, docstrings em funções de pipeline.

---

### S — Support Information (Informações de Suporte)

**Objetivo:** Incluir guias de troubleshooting, monitoramento e sugestões de melhorias futuras.

**O que documentar:**
- Como diagnosticar falhas comuns
- Métricas ou logs a monitorar
- Próximos passos ou melhorias planejadas
- Limitações conhecidas

**Formato recomendado:**
```python
class SchemaCache:
    """
    Cache de schema de banco por tenant com invalidação automática.

    TROUBLESHOOTING:
    - "Schema not found": verifique se o job de sincronização rodou (ver scripts/sync_schemas.py)
    - "Stale schema causing query failure": force refresh via admin endpoint POST /nl2sql/schema/refresh
    - Cache miss rate alto: verificar TTL em SYSTEM_PARAMETER 'schema_cache.ttl' (default: 3600s)

    MONITORAMENTO:
    - Métrica: nl2sql.schema_cache.hit_rate (Redis, por tenant)
    - Log: WARNING schema_cache_miss tenant={tenant_id} indica degradação
    - Alerta: se hit_rate < 70% por 5min, verificar job de sincronização

    MELHORIAS FUTURAS:
    - [ ] Suporte a schema versionado (multi-version para migrações)
    - [ ] Webhook de invalidação automática via CDC do PostgreSQL
    """
```

**Saída esperada:** Seções TROUBLESHOOTING e MONITORAMENTO nos módulos de infraestrutura.

---

## 6. Processo de Execução

### Passo 1 — Leitura e Mapeamento
```
1. Leia o módulo/feature alvo completamente
2. Identifique quais partes têm documentação atual (e se está correta)
3. Identifique os pontos de maior complexidade ou risco operacional
4. Decida o nível de documentação: inline docstring, module README, ADR file
```

### Passo 2 — Aplicação D.O.C.S.
```
D → Quais decisões de design precisam ser registradas?
O → Quais dependências e restrições operacionais existem?
C → Quais partes do código não são auto-explicativas?
S → Quais falhas são previsíveis e como diagnosticá-las?
```

### Passo 3 — Implementação
```
1. Priorize C (entendimento) e D (decisões) — são os mais perdidos com IA
2. Adicione O (contexto) em módulos de infra e serviços com dependências externas
3. Adicione S (suporte) em módulos que já foram fonte de incidentes ou têm falhas conhecidas
4. Nunca documente o óbvio — se o nome explica, não adicione docstring
```

### Passo 4 — Entrega
Produzir um relatório estruturado:
```
## D.O.C.S. Report — [Nome do Módulo]

### D — Decisões Documentadas
[lista de decisões de design registradas]

### O — Contexto Operacional Registrado
[dependências, feature flags, restrições documentadas]

### C — Compreensão de Código Adicionada
[algoritmos e fluxos documentados]

### S — Informações de Suporte
[troubleshooting e monitoramento documentados]

### Cobertura de Documentação
COMPLETA | PARCIAL (justificativa) | PENDENTE [motivo]
```

---

## 7. Regras de Ouro

1. **Documente o porquê, não o quê** — o código já explica o quê
2. **Docstring de classe > comentário de linha** — contexto no nível certo
3. **Honestidade sobre limitações** — se tem bug conhecido, documente
4. **IA não documenta a si mesma** — contexto de geração é responsabilidade humana
5. **Documentação desatualizada é pior que nenhuma** — atualize ao refatorar
