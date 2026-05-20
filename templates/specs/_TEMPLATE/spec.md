# Spec: [Nome da Feature]

> **Status:** rascunho | em revisão | aprovada
> **Autor:** [seu nome]
> **Data:** YYYY-MM-DD

## Contexto de Negócio

Por que esta feature existe? Que problema resolve? Quem são os usuários impactados?

## Comportamento Esperado

Descrição em linguagem natural do que o sistema deve fazer.

## Critérios de Aceite

Use o formato Gherkin (DADO/QUANDO/ENTÃO) ou lista numerada.

1. **DADO** que [estado inicial]
   **QUANDO** [ação ocorre]
   **ENTÃO** [resultado esperado]

2. **DADO** ...

## Casos de Borda

- O que acontece se [input inválido]?
- O que acontece se [serviço externo cair]?
- Comportamento com [valor limite]?
- Comportamento com [coleção vazia / null]?

## Contratos

### Endpoint (se aplicável)
- **Método:** POST | GET | PUT | DELETE
- **Path:** `/api/v1/...`
- **Autenticação:** [requerida / pública]
- **Request:**
  ```json
  { ... }
  ```
- **Response 200/201:**
  ```json
  { ... }
  ```
- **Response 400 (validação):**
  ```json
  { ... }
  ```
- **Response 422 (regra de negócio):**
  ```json
  { ... }
  ```

### Eventos Publicados (se houver)
- **Tópico:** `nome-do-topico`
- **Schema:** [link ou inline]
- **Quando publicar:** [condição]

### Eventos Consumidos (se houver)
- **Tópico:** `nome-do-topico`
- **Comportamento esperado:** [descrição]

## Dependências

- **Tabelas afetadas:** ...
- **Serviços externos chamados:** ...
- **Configurações novas:** ...
- **Feature flags:** ...

## Não-Funcionais

- **Latência esperada:** p99 < ___ ms
- **Volumetria:** ~___ req/s pico
- **Idempotência:** SIM | NÃO
- **Auditoria:** [o que registrar]
- **SLA da feature:** [se aplicável]

## Fora de Escopo

Liste explicitamente o que esta feature **NÃO** faz, para evitar scope creep.

## Decisões e Riscos

- **[Decisão tomada]** — justificativa
- **[Risco conhecido]** — mitigação ou aceitação

## Glossário

- **[Termo do domínio]:** definição
