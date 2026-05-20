# Plano: [Nome da Feature]

> **Status:** rascunho | em revisão | aprovado | em execução | concluído
> **Spec relacionada:** `spec.md` (mesma pasta)
> **Autor:** [seu nome]
> **Data:** YYYY-MM-DD

## Decisões Técnicas

Para cada decisão relevante de design/arquitetura:

### Decisão 1: [Título curto]
- **O que:** [decisão tomada]
- **Por quê:** [justificativa]
- **Alternativas consideradas:**
  - [Alternativa A] — descartada porque [razão]
  - [Alternativa B] — descartada porque [razão]
- **Trade-offs:** [o que ganhamos e o que abrimos mão]

### Decisão 2: ...

## Mudanças por Camada

### Domain
- Entidade `[Nome]` com campos: ...
- Value object `[Nome]`: ...
- Exception `[Nome]Exception` para [caso]

### Persistence
- Tabela `[nome_tabela]` (migration: `V2_X__add_xxx.sql`)
- Repository `[Nome]Repository` com métodos: ...
- Índices: ...

### Application (Services)
- Service `[Nome]Service` com casos de uso:
  - `[metodoA]`: [descrição]
  - `[metodoB]`: [descrição]

### Interface
- Controller `[Nome]Controller`:
  - `POST /api/v1/...`
- DTOs: `[Nome]Request`, `[Nome]Response`

### Infraestrutura
- Configurações novas em `application.yml`
- Properties: ...
- Feature flag: `feature.xxx.enabled`

## Subtasks (em ordem de execução)

Granularidade: cada subtask deve caber em 1-4h. Marque dependências quando houver.

1. [ ] **Migration Flyway** — `V2_X__add_xxx.sql` (sem dependências)
2. [ ] **Entity + Repository** — depende de #1
3. [ ] **Service + testes unitários** — depende de #2
4. [ ] **DTOs** — sem dependências (paralelo com #2 e #3)
5. [ ] **Controller + tratamento de erros** — depende de #3 e #4
6. [ ] **Testes de integração** — depende de #5
7. [ ] **Atualizar OpenAPI** — depende de #5
8. [ ] **Documentar feature flag** — sem dependências (paralelo)

## Pontos de Atenção

- [ ] Migration é backward-compatible (deploy sem downtime)?
- [ ] Feature flag configurada em todos os ambientes?
- [ ] Volumetria estimada validada com índices propostos?
- [ ] Configurações de timeout/retry alinhadas com SLAs dos serviços chamados?
- [ ] Logs e métricas atendem ao runbook de operação?

## Riscos Identificados

| Risco | Probabilidade | Impacto | Mitigação |
|---|---|---|---|
| [Risco 1] | Alta/Média/Baixa | Alto/Médio/Baixo | [como mitigar] |
| [Risco 2] | | | |

## Validação Pós-Implementação

Como validar que a feature está funcionando em produção:
- Métricas a monitorar: ...
- Alertas a configurar: ...
- Testes manuais (smoke test): ...

## Log de Mudanças no Plano

Mudanças significativas no plano durante a execução:
- YYYY-MM-DD: [o que mudou e por quê]
