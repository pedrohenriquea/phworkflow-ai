# Templates

Arquivos prontos para clonar em novos repositórios.

## Como Usar

1. Identifique quais templates aplicam ao seu projeto
2. Copie para os locais corretos no seu repo (ver mapeamento abaixo)
3. Adapte o conteúdo entre `[colchetes]` ao seu contexto
4. Commite no repo do projeto

## Mapeamento: template → destino no projeto

```
templates/copilot-instructions/java-spring-fintech.md
  → .github/copilot-instructions.md

templates/instructions/java.instructions.md
  → .github/instructions/java.instructions.md

templates/instructions/tests.instructions.md
  → .github/instructions/tests.instructions.md

templates/prompts/*.prompt.md
  → .github/prompts/*.prompt.md (todos os 4 arquivos)

templates/specs/_TEMPLATE/
  → specs/_TEMPLATE/  (mantém o nome como referência)
```

## Por que cada arquivo está aqui

### `copilot-instructions/`
Templates do arquivo principal de contexto. Versão diferente por tipo de stack/domínio. Hoje temos `java-spring-fintech.md`; futuramente outros (Node, Python, frontend).

### `instructions/`
Templates de instruções específicas por tipo de arquivo (`applyTo` no frontmatter). Aplicadas automaticamente pelo Copilot conforme o arquivo aberto.

### `prompts/`
Slash commands reutilizáveis. Os 4 essenciais cobrem o ciclo: revisar spec → gerar plano → checklist de qualidade → revisar PR.

### `specs/_TEMPLATE/`
Templates de `spec.md` e `plan.md` que cada feature copia para sua pasta dentro de `specs/`.

## Evoluindo os Templates

Templates não são imutáveis. Quando descobrir algo útil aplicando-os em projetos reais:

1. Documente o aprendizado em `examples/`
2. Atualize o template aqui
3. Comunique ao time (changelog, retro, etc)

**Princípio:** templates aqui devem refletir o que funciona na prática, não teoria.
