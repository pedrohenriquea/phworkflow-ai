# Playbook: Desenvolvimento com IA em Novos Projetos
## Java + Spring Boot | GitHub Copilot

> Fluxo manual de Spec-Driven Development usando os caminhos que o Copilot já lê automaticamente. Sem instalar ferramenta nova.

---

## Começando (primeira vez)

Não precisa ler o playbook todo antes de começar. Caminho mínimo:

1. Crie `.github/copilot-instructions.md` com o stack e padrões do projeto (ver [Fase 0.1](#01-githubcopilot-instructionsmd) para exemplo).
2. Copie os 4 slash commands do [Anexo A](#anexo-a--slash-commands-prontos) para `.github/prompts/`.
3. Escolha uma feature **pequena** como piloto.
4. Siga a [matriz de calibração](#calibrar-pelo-tamanho-da-feature) — geralmente: spec curta → implementar uma subtask por chat → `mvn verify` → PR.
5. Após a primeira entrega: cada erro que apareceu vira uma linha em `copilot-instructions.md`.

O resto do playbook é consulta sob demanda.

---

## Princípios

1. **Contexto antes de código.** 15 min de contexto economizam horas de retrabalho.
2. **Spec → Plano → Implementação.** Cada fase tem entrega própria.
3. **Mais contexto ≠ melhor.** Janela limitada; ruído derruba precisão (*context rot*).
4. **IA executa, você decide.** Arquitetura, segurança e negócio são suas.
5. **Calibre pelo tamanho.** O fluxo completo é para feature média/grande. Trivial pula tudo.

---

## Calibrar pelo tamanho da feature

| Tamanho | Quando | Spec | Plano | Subtasks por chat |
|---|---|---|---|---|
| **Trivial** (<1h) | Bugfix, copy, config, ajuste pontual | — | — | — |
| **Pequena** (1-4h) | CRUD ou endpoint seguindo padrão existente | Inline no PR | — | Opcional |
| **Média** (1-3 dias) | Lógica nova, integração externa, fluxo novo | `spec.md` curto | `plan.md` curto | Sim |
| **Grande** (>3 dias) | Domínio novo, mudança arquitetural, refactor amplo | Completo | Completo + ADR | Sim |

**Regras práticas:**
- **Em dúvida, suba um nível.** Spec sobrando custa minutos; código sobrando custa dias.
- **Spike/exploração:** sem spec/plano, mas registre aprendizado num ADR. Spec vem depois, baseada no que funcionou.
- **Mudança arquitetural:** ADR primeiro, depois spec/plano.
- **Reaproveite:** feature parecida pronta? Anexe `#file:specs/feature-anterior/` na criação da nova spec.

> **Glossário rápido:**
> - **Subtask** — fatia pequena (1-4h) dentro da implementação de uma feature; cada uma é implementada em um chat próprio.
> - **ADR** (*Architecture Decision Record*) — registro curto de uma decisão arquitetural, salvo em `docs/adr/`. Vira a memória de longo prazo do projeto.

---

## Disciplinas de Contexto

1. **Chat curto, novo a cada fase.** Uma feature por conversa. Spec → novo chat para o plano → novo chat por subtask. Histórico longo polui o contexto.
2. **Contexto cirúrgico.** Via `#file:`, anexe só a spec, o plano e 2-3 referências. Subtask que precisa de >5-6 arquivos é grande demais — quebre.
3. **Memória em arquivos.** Decisões vão para `plan.md` ou ADR (`docs/adr/`). O chat é volátil; não confie no histórico.
4. **Instruções enxutas.** `copilot-instructions.md` curto e com o *porquê* de cada regra. Detalhe por tipo de arquivo em `instructions/*.instructions.md` com `applyTo`.

---

## Como o Copilot lê o contexto

| Tipo | Local | Quando aplica |
|---|---|---|
| **Instruções gerais** | `.github/copilot-instructions.md` | Em **toda** interação |
| **Por arquivo** | `.github/instructions/*.instructions.md` | Quando o `applyTo` casa |
| **Slash commands** | `.github/prompts/*.prompt.md` | Invocação `/nome` |

---

## Fase 0 — Preparar o Repositório (uma vez)

### 0.1. `.github/copilot-instructions.md`

Conciso. Cada linha justificada. Erro recorrente vira regra aqui.

```markdown
# Contexto do Projeto

## Stack
- Java 21, Spring Boot 3.x, Maven
- PostgreSQL + Flyway, Kafka

## Arquitetura
- Camadas: controller → service → repository
- DTOs separados de entidades JPA
- Constructor injection (nunca @Autowired em campo)

## Padrões
- Endpoints REST em `/api/v1/{recurso}`, Bean Validation
- Logs JSON via SLF4J, sem dados sensíveis
- Chamadas externas com timeout e fallback
- Testes: JUnit 5 + Mockito + AssertJ

## NÃO fazer
- Lombok @Data em entidades JPA
- Field injection
- Capturar Exception genérica
- SQL por concatenação
```

> O bloco acima é exemplo. Substitua pelo stack e padrões reais do seu projeto, e ajuste as regras de segurança ao seu domínio (dados pessoais, financeiros, saúde, etc.) e às políticas internas.

### 0.2. Instruções por arquivo (opcional)

Em `.github/instructions/java.instructions.md`:

```markdown
---
applyTo: "**/*.java"
---
- Records para DTOs
- Optional só em retorno, nunca parâmetro
```

### 0.3. Prompts reutilizáveis

Quatro slash commands em `.github/prompts/` (conteúdo no [Anexo A](#anexo-a--slash-commands-prontos)):

- `/revisar-spec` — checa spec antes do plano
- `/gerar-plano` — gera plano técnico
- `/checklist-qualidade` — antes do PR
- `/revisar-pr` — auto-revisão de branch

### 0.4. Build com qualidade

SpotBugs + PMD + Checkstyle, JaCoCo, ArchUnit, OWASP Dependency Check.

### 0.5. Templates de spec/plano

Crie `specs/_TEMPLATE/spec.md` e `specs/_TEMPLATE/plan.md` (templates abaixo).

---

## Fase 1 — Especificar

> Aplica a partir de **feature pequena** (inline no PR) ou **média/grande** (`spec.md`). Trivial pula.

Crie `specs/[feature]/spec.md`:

```markdown
# Spec: [Nome]

## Contexto de Negócio
Por que existe? Que problema resolve?

## Comportamento Esperado
O que o sistema deve fazer.

## Critérios de Aceite
- DADO ... QUANDO ... ENTÃO ...

## Casos de Borda
- Input inválido, serviço externo cair, valores limite.

## Contratos
- Endpoint: método, path, request, responses (200, 400, 422)
- Eventos publicados (se houver)

## Não-Funcionais
- Latência, idempotência, volumetria.

## Fora de Escopo
- O que esta feature NÃO faz.
```

Anexe via `#file:` e rode `/revisar-spec`. Investimento: 20-30 min.

> Disciplina #1: feche o chat ao terminar.

---

## Fase 2 — Planejar

> Aplica a partir de **feature média**. Pequenas geralmente não precisam.

Use `/gerar-plano` a partir da spec, depois revise. Crie `specs/[feature]/plan.md`:

```markdown
# Plano: [Feature]

## Decisões Técnicas
- [Decisão] porque [razão]. Alternativas: X (descartada por Y).

## Mudanças por Camada
- **Domain:** entidade Z, exception XException
- **Persistence:** migration V2_X__add_z.sql, repository W
- **Application:** ZService, casos A e B
- **Interface:** POST /api/v1/...

## Subtasks (1-4h cada, em ordem)
1. [ ] Migration Flyway V2_X__add_z.sql
2. [ ] Entity Z + ZRepository
3. [ ] ZService + testes unitários
4. [ ] DTOs request/response
5. [ ] ZController + tratamento de erros
6. [ ] Testes de integração
7. [ ] OpenAPI

## Pontos de Atenção
- Migration backward-compatible
- Feature flag `feature.z.enabled`

## Riscos
- Performance da query X
- SLA do serviço Y
```

Revisão humana do plano antes do código. Ajustar plano custa minutos; ajustar código custa horas.

> Disciplina #1: plano aprovado → novo chat para implementar.

---

## Fase 3 — Implementar

Uma subtask por vez, contexto mínimo:

1. Novo chat
2. `#file:` spec + plano + 2-3 referências
3. Diga qual subtask
4. Implemente (use **Agent Mode** se tocar múltiplos arquivos; **Ask** para explorar)
5. `mvn verify`
6. Marque `[x]` no `plan.md`
7. Próxima subtask → novo chat

### Como soa um prompt produtivo

> Vou implementar a **subtask 3** do plano (`ZService + testes unitários`).
> Spec, plano e `YService` estão anexados — `YService` usa o mesmo padrão de transação que quero aqui.
> Gere o `ZService` + **5 testes**: 1 caso feliz, 2 borda, 2 falha. Use AssertJ.
> **Não toque** em controller, repository ou DTOs — outras subtasks cuidam disso.

**O que esse prompt acerta:** subtask explícita, referência concreta, escopo de testes, limites claros do que *não* fazer.

**O que evitar:** "Implementa o ZService" — sem referência, sem escopo, sem limites. A IA preenche as lacunas chutando.

### Testes

Todo código tem testes antes do PR. Domínio complexo → escreva testes antes. CRUD simples → depois. Decisão do dev.

### Build a cada subtask

Não acumule erros. Falhou? Cole o erro no chat e peça correção antes de seguir.

---

## Fase 4 — Checklist antes do PR

Com o código aberto, rode `/checklist-qualidade`. Para cada item problemático, peça o teste que captura o bug e adicione ao PR.

---

## Fase 5 — Pull Request

- `/revisar-pr` antes da revisão humana
- Habilite **Copilot Code Review** no GitHub (também lê `copilot-instructions.md`)
- Peça descrição do PR a partir de commits + spec + plano

---

## Anti-Padrões

| Não faça | Faça em vez |
|---|---|
| "Implementa a feature X" direto | Spec → plano → implementação |
| Spec+plano completo para bugfix | Calibre pelo tamanho (matriz acima) |
| Tudo num chat só | Uma subtask por chat |
| Anexar repositório inteiro | `#file:` cirúrgico |
| Confiar no histórico do chat | Decisões em `plan.md` / ADR |
| `copilot-instructions.md` gigante | Conciso + `instructions/` por contexto |
| Aceitar código sem build | `mvn verify` por subtask |
| Pular Fase 4 | É o que mais reduz bug em produção |
| Aceitar testes sem revisar | Revise os asserts manualmente |

---

## Estrutura Final do Repositório

```
projeto/
├── .github/
│   ├── copilot-instructions.md
│   ├── instructions/
│   │   ├── java.instructions.md
│   │   └── tests.instructions.md
│   └── prompts/
│       ├── revisar-spec.prompt.md
│       ├── gerar-plano.prompt.md
│       ├── checklist-qualidade.prompt.md
│       └── revisar-pr.prompt.md
├── specs/
│   ├── _TEMPLATE/
│   └── feature-xyz/{spec.md, plan.md}
├── docs/adr/
├── src/
└── pom.xml
```

---

## Métricas mensais

Bugs em produção por feature ↓ · Cobertura estável · Tempo PR→merge ↓ · Violações SpotBugs/PMD ↓ · % features com plano aprovado antes do código → 100%.

---

## TL;DR

1. **Calibre pelo tamanho.** Trivial pula tudo; média/grande faz spec+plano.
2. `copilot-instructions.md` é a fonte de verdade — auto-lido, conciso
3. **Spec → plano → implementação subtask-por-subtask** para feature média+
4. Cada fase em chat novo
5. Memória em arquivos, não no chat
6. Slash commands: `/revisar-spec`, `/gerar-plano`, `/checklist-qualidade`, `/revisar-pr`
7. `mvn verify` por subtask
8. IA executa, você decide

---

## Próximos Passos

- [ ] Piloto com estrutura completa
- [ ] Aplicar em feature pequena (1-2 dias)
- [ ] Retrospectiva após 2 semanas
- [ ] Replicar para outros projetos novos
- [ ] **(Futuro)** Adaptar para legado

---

## Anexo A — Slash commands prontos

Copie cada bloco para `.github/prompts/<nome>.prompt.md`.

> O campo `agent: 'ask'` no front-matter força o chat em modo **somente-resposta** (sem editar arquivos) — apropriado para revisar e planejar. Prompts sem esse campo permitem ao Copilot editar código (modo Agent).

### `revisar-spec.prompt.md`
```markdown
---
description: 'Revisa uma spec antes do plano'
agent: 'ask'
---
Revise a spec anexada procurando:
1. Casos de borda não cobertos
2. Riscos de segurança/compliance
3. Ambiguidades nos critérios de aceite
4. Inconsistências com os padrões do projeto
5. Não-funcionais ausentes (latência, idempotência, observabilidade)

Liste perguntas que você faria ao PO. NÃO escreva código nem plano.
```

### `gerar-plano.prompt.md`
```markdown
---
description: 'Gera plano técnico a partir de uma spec aprovada'
agent: 'ask'
---
A partir da spec anexada, gere um plano seguindo specs/_TEMPLATE/plan.md:

1. Decisões técnicas com justificativa e alternativas
2. Mudanças por camada (domain, persistence, application, interface)
3. Subtasks em ordem (1-4h cada)
4. Pontos de atenção (migrations, feature flags)
5. Riscos

NÃO implemente. Apenas o plano.
```

### `checklist-qualidade.prompt.md`
```markdown
---
description: 'Checklist de qualidade antes do PR'
---
Revise o código aberto contra este checklist e sugira testes para o que faltar:

**Segurança:** dado sensível em log; entrada validada; stack trace não vaza; secrets fora do código.
**Robustez:** timeouts em chamadas externas; circuit breaker/retry; idempotência; transações.
**Adversarial:** inputs maliciosos; race conditions; falha em cascata; overflow/precisão decimal.
**Observabilidade:** logs estruturados com correlation-id; métricas; erros com contexto.
**Código:** sem TODO/FIXME; cobertura ≥ 80%; build limpo.
```

### `revisar-pr.prompt.md`
```markdown
---
description: 'Auto-revisão da branch antes da revisão humana'
---
Code review desta branch como tech lead. Foque em legibilidade,
manutenibilidade, aderência aos padrões em copilot-instructions.md
e qualidade dos testes. Crítico mas construtivo.
```

---

## Referências

- [GitHub Docs — Custom instructions](https://docs.github.com/en/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot)
- [VS Code — Custom instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
- [VS Code — Prompt files](https://code.visualstudio.com/docs/copilot/customization/prompt-files)
- [GitHub Spec Kit](https://github.com/github/spec-kit)
- [Awesome GitHub Copilot Customizations](https://github.com/github/awesome-copilot)
