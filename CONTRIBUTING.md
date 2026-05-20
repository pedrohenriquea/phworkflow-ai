# Como Contribuir

Este repo evolui com o uso real. Toda contribuição deve vir de aprendizado prático, não teoria.

## Tipos de Contribuição

### 1. Atualizar templates
Descobriu um padrão que a IA continua errando? Adicione regra ao template correspondente.

### 2. Novo prompt
Tem um prompt que usa toda semana? Vira `templates/prompts/*.prompt.md`.

### 3. Novo playbook
Aplicou o fluxo em um contexto diferente (legado, frontend, observability)? Crie playbook próprio em `playbooks/`.

### 4. Documentar caso real
Concluiu um projeto seguindo o playbook? Documente em `examples/` — especialmente o que **não** funcionou.

### 5. Refinar conceitos
Encontrou explicação melhor para um conceito em `docs/`? PR é bem-vindo.

## Processo

1. Branch a partir de `main`
2. Faça a mudança
3. Abra PR com:
   - **O que muda**
   - **Por que** (baseado em qual aprendizado prático)
   - **Como testou** (se for prompt/template, em qual contexto aplicou)
4. Pelo menos 1 revisor antes do merge

## Princípios Editoriais

- **Conciso > completo.** Documentação longa não é lida.
- **Por que > o quê.** Explique o raciocínio, não só a regra.
- **Exemplos > abstrações.** Casos concretos ensinam mais.
- **Versione padrões, não preferências.** Estilo pessoal vai em config local, não no repo.
