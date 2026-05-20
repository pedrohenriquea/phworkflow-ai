# ph-ai-playbook

> Playbooks, templates e convenções para desenvolvimento assistido por IA.
> Foco em qualidade, produtividade e independência de provedor.

## O que é este repo

Conjunto de **práticas operacionais** para desenvolver com IA de forma consistente, reduzindo bugs em produção e mantendo aderência aos padrões do time.

**Hoje focado em GitHub Copilot.** Os princípios (controle de contexto, Spec-Driven Development, slash commands) se aplicam a Claude Code, Cursor e similares — playbooks específicos para esses provedores estão no roadmap.

Não é uma ferramenta. É um conjunto de:
- **Playbooks** — fluxos de trabalho passo a passo por tipo de projeto
- **Templates** — arquivos prontos para clonar em novos repos
- **Conceitos** — base teórica curta sobre como IA funciona (contexto, agentes, SDD)

## Como usar

**Para começar um novo projeto:**

1. Leia [`playbooks/novos-projetos-java-spring.md`](playbooks/novos-projetos-java-spring.md)
2. Copie os templates relevantes de `templates/` para o seu repo
3. Adapte `.github/copilot-instructions.md` ao stack e padrões do seu projeto

**Para entender os fundamentos:**

- [Disciplinas de Contexto](docs/disciplinas-de-contexto.md) — por que controlar contexto importa
- [Glossário](docs/glossario.md) — SDD, agentes, context rot e outros termos

## Estrutura do Repositório

```
ph-ai-playbook/
├── README.md                          # este arquivo
├── playbooks/                         # fluxos operacionais
│   └── novos-projetos-java-spring.md
├── templates/                         # arquivos prontos para clonar
│   ├── copilot-instructions/
│   ├── instructions/
│   ├── prompts/
│   └── specs/_TEMPLATE/
├── examples/                          # casos reais aplicados
└── docs/                              # conceitos fundamentais
    ├── disciplinas-de-contexto.md
    └── glossario.md
```

## Roadmap

- [x] Playbook: novos projetos Java + Spring Boot (GitHub Copilot)
- [ ] Playbook: novos projetos com Claude Code
- [ ] Playbook: novos projetos com Cursor
- [ ] Playbook: projetos legado (engenharia reversa de contexto)
- [ ] Playbook: code review com IA
- [ ] Playbook: observability e debugging com IA
- [ ] Templates para outros stacks (Node, Python)

## Como contribuir

Este repo evolui com o uso. Sempre que descobrir um padrão novo, prompt útil ou erro recorrente da IA:

1. Abra PR com a mudança
2. Inclua o contexto do aprendizado em `examples/`
3. Atualize templates se aplicável

**Princípio:** cada lição aprendida no campo vira uma linha de instrução ou um prompt aqui.
