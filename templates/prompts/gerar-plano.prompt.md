---
description: 'Gera plano técnico a partir de uma spec aprovada'
agent: 'ask'
---

A partir da spec referenciada nesta conversa, gere um plano técnico de implementação seguindo o template em `specs/_TEMPLATE/plan.md`.

## O plano deve incluir

### 1. Decisões Técnicas
Para cada decisão relevante:
- O que foi decidido
- Por quê (justificativa)
- Alternativas consideradas e por que descartadas

### 2. Mudanças por Camada
Liste mudanças em cada camada da arquitetura:
- **Domain:** entidades, value objects, exceptions novas
- **Persistence:** tabelas, migrations, repositories
- **Application:** services, casos de uso
- **Interface:** controllers REST, listeners Kafka, schedulers

### 3. Subtasks em Ordem de Execução
- Granularidade: cada subtask deve caber em 1-4h de trabalho
- Em ordem topológica (dependência antes de dependente)
- Marque subtasks que podem ser paralelizadas
- Cada subtask deve ser implementável com contexto mínimo (≤ 5-6 arquivos)

### 4. Pontos de Atenção
- Migrations backward-compatible (deploy sem downtime)?
- Feature flags necessárias?
- Compatibilidade com versões anteriores de APIs?
- Configurações novas em properties/Vault?

### 5. Riscos Identificados
- Performance (queries pesadas, N+1, etc)
- Dependências externas (SLAs, fallback)
- Concorrência (race conditions, deadlocks)
- Dados (volumetria, migração de dados existentes)

## Importante

- **NÃO implemente código.** Apenas o plano.
- Respeite os padrões em `.github/copilot-instructions.md`
- Se a spec tiver ambiguidades, liste-as antes do plano e peça esclarecimento
