# Glossário

Termos usados nos playbooks deste repo.

## A

### ADR (Architecture Decision Record)
Documento curto que registra uma decisão arquitetural, seu contexto, alternativas consideradas e consequências. Vive no repo (`docs/adr/`) e é versionado junto com o código. Memória de longo prazo do projeto.

### Agent Mode (Copilot)
Modo do GitHub Copilot que opera como um agente real: edita múltiplos arquivos, executa testes, observa resultados e ajusta em loop. Diferente do modo "Ask" (perguntas e respostas) e "Edit" (edição pontual).

### Agente
Sistema onde a IA opera em loop autônomo: recebe um objetivo, decide quais ferramentas usar, executa, observa o resultado e ajusta o plano. Tem capacidade de **agir**, não só responder. Exemplos: Claude Code, Copilot Agent Mode, Cursor Agent, Cline.

**Não confundir com:** "agente persona" (`dev-backend`, `architect`), que é apenas um prompt com papel atribuído — não tem nada de autônomo.

## C

### Context Rot
Degradação da qualidade das respostas da IA quando o contexto fica muito grande ou poluído. Estudos mostram queda significativa de precisão quando o contexto passa de ~50% da janela suportada, mesmo dentro do limite técnico.

### Context Window (Janela de Contexto)
Quantidade máxima de tokens (≈ palavras) que um modelo pode processar em uma única requisição. Modelos modernos têm janelas grandes (128k-1M tokens), mas qualidade degrada antes de atingir o limite.

### Copilot Instructions
Arquivo `.github/copilot-instructions.md` lido automaticamente pelo Copilot em todas as interações. Padrão oficial do GitHub para configurar comportamento por repositório.

## I

### Instructions (path-specific)
Arquivos `.github/instructions/*.instructions.md` aplicados apenas quando o arquivo aberto bate com o padrão `applyTo` definido no frontmatter. Permite regras específicas por linguagem ou tipo de arquivo sem inflar o `copilot-instructions.md`.

## P

### Plan (no contexto SDD)
Documento que descreve **como** implementar uma feature, com decisões técnicas, mudanças por camada, subtasks ordenadas e riscos. Vive entre a Spec (o quê) e a Implementação (execução).

### Prompt Files
Arquivos `.github/prompts/*.prompt.md` que viram slash commands invocáveis no chat (`/nome-do-prompt`). Permitem padronizar prompts recorrentes do time.

## R

### RAG (Retrieval-Augmented Generation)
Técnica onde, antes de chamar o modelo, um sistema busca semanticamente os trechos mais relevantes de uma base de conhecimento e os anexa ao contexto. Ferramentas como Copilot e Cursor usam isso internamente para puxar trechos do codebase.

## S

### SDD (Spec-Driven Development)
Metodologia de desenvolvimento onde toda feature começa com uma especificação formal antes do código. Frameworks: GitHub Spec Kit, Kiro (AWS), Cline Memory Bank. Princípio central: a spec é a fonte da verdade, código é derivação.

### Spec
Documento que descreve **o quê** uma feature faz e **por quê**, sem entrar em como implementar. Contém critérios de aceite, casos de borda, contratos e requisitos não-funcionais.

### Subtask
Unidade pequena de trabalho dentro de um plano. Granularidade recomendada: 1-4h. Cada subtask deve ser implementável com contexto mínimo (≤ 5-6 arquivos).

## T

### TDD (Test-Driven Development)
Prática de escrever testes antes do código de produção. Combina bem com IA porque testes servem como "contrato executável" que reduz espaço para a IA inventar comportamento. **Não é obrigatório** neste playbook — é uma técnica entre outras.

## V

### Vertical Slice
Implementação de uma feature ponta a ponta em fatias finas (controller → service → repo → DB funcionando junto) em vez de "horizontal" (toda a camada de dados, depois toda a camada de serviço). Reduz risco e dá feedback cedo.
