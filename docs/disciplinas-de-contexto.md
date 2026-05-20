# Disciplinas de Contexto

> Por que controlar o contexto importa para qualidade — e como fazer isso na prática.

## O Problema

Modelos de IA têm uma **janela de contexto** limitada. Tudo que entra nessa janela (seu prompt, arquivos abertos, instruções, histórico do chat, resultados de busca) influencia a resposta.

Dois problemas existem em direções opostas:

**Contexto demais (context rot):**
- Modelos perdem precisão quando o contexto fica grande, mesmo dentro do limite suportado
- Informação relevante "se dilui" em meio a informação irrelevante
- Sintomas: a IA ignora instruções do `copilot-instructions.md`, esquece decisões anteriores, inventa padrões fora do projeto

**Contexto de menos:**
- A IA chuta padrões genéricos em vez de seguir os do projeto
- Gera código fora da arquitetura
- Sintomas: PRs que precisam ser refeitos para se adequar ao projeto

**Controle de contexto é o equilíbrio entre os dois.**

## Como Ferramentas SDD Tratam Isso

Ferramentas maduras (Spec Kit, Kiro, Aider, Cline) gerenciam contexto explicitamente:

1. Carregam apenas arquivos relevantes para a tarefa atual
2. Resetam contexto entre fases (a fase de implementação não carrega histórico da fase de spec, só o resultado final)
3. Mantêm "memória externa" em arquivos versionados (specs, plans, decisions)
4. Usam RAG (busca semântica) para puxar só o trecho relevante de arquivos grandes

Como estamos construindo nosso padrão sem ferramenta dedicada, precisamos das mesmas disciplinas — manualmente.

## As 7 Disciplinas

### 1. Uma feature, uma conversa
Não misture features no mesmo chat. Ao terminar uma, abra um chat novo.

**Por quê:** evita que contexto antigo polua o novo. Histórico de chat é uma fonte de poluição comum.

### 2. `copilot-instructions.md` enxuto
Mantenha conciso. Detalhes por contexto vão para `instructions/*.instructions.md` com `applyTo` específico.

**Por quê:** o `copilot-instructions.md` é carregado em **toda** interação. Cada linha adicional reduz o espaço para o conteúdo real da tarefa. As instructions específicas só carregam quando o tipo de arquivo bate, economizando contexto.

### 3. `#file:` cirúrgico
Anexe só o necessário: a spec, o plano, 2-3 arquivos de referência. Evite "olha o projeto inteiro" ou anexar pastas inteiras.

**Por quê:** mais arquivos = mais ruído. A IA presta mais atenção a contextos enxutos e relevantes.

### 4. Memória em arquivos, não no chat
Decisões importantes vão para `plan.md`, `spec.md` ou ADR (`docs/adr/`). Nunca confie no histórico do chat para lembrar decisões.

**Por quê:** chats são efêmeros. Em chats longos, a IA "esquece" o que foi dito 20 mensagens atrás. Arquivos versionados são memória de longo prazo, recarregáveis sob demanda.

### 5. Resete entre fases
Spec terminada → fecha o chat → novo chat para plano. Plano terminado → fecha o chat → novo chat para implementar subtask 1.

**Por quê:** cada fase tem foco diferente. Misturar discussão de spec com discussão de implementação polui o contexto. Cada novo chat começa "limpo" carregando só o que importa para aquela fase.

### 6. Subtasks pequenas o suficiente
Se uma subtask precisa de mais de 5-6 arquivos como contexto, é grande demais. Quebre em duas ou três.

**Por quê:** subtasks grandes forçam contexto grande, que ativa o context rot. Subtasks pequenas com contexto enxuto produzem código melhor.

### 7. Limpe contexto explicitamente
Em chats longos, prefira abrir um chat novo a tentar "resetar via prompt" ("ignore tudo antes desta mensagem...").

**Por quê:** instruções de reset em texto não removem o contexto da janela — apenas pedem para a IA ignorá-lo, o que ela faz com baixa confiabilidade. Chat novo é garantido.

## Sintomas de Má Gestão de Contexto

Reconheça os sinais:

- IA gera código que ignora `copilot-instructions.md` em chats longos → contexto poluído
- Bugs porque a IA "esqueceu" decisão tomada 20 mensagens atrás → memória só no chat
- Implementação diverge da spec porque ela não foi recarregada → falta de #file:
- Refactor sugerido quebra invariantes porque a IA não viu o teste correspondente → contexto incompleto
- A IA "perde a linha" em conversas longas, dá respostas mais genéricas → context rot

Quando isso acontece, **abra chat novo**.

## Em Resumo

| Princípio | Prática |
|---|---|
| Janela de contexto é finita | Use-a com economia |
| Mais não é melhor | Cirurgia, não acúmulo |
| Chat é efêmero | Memória vai para arquivos |
| Cada fase, contexto novo | Reseta entre fases |
| Subtask grande = contexto grande | Quebre |

Controle de contexto é uma habilidade. Como qualquer habilidade, melhora com prática consciente.
