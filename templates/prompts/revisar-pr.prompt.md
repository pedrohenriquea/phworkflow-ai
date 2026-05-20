---
description: 'Auto-revisão da branch antes de pedir revisão humana'
---

Faça code review desta branch como se fosse o tech lead do time. Seja crítico mas construtivo.

## Foque em

### 1. Legibilidade
- Nomes de variáveis, métodos e classes claros?
- Métodos curtos e com responsabilidade única?
- Comentários explicam o "porquê", não o "o quê"?
- Código autodocumentado onde possível?

### 2. Manutenibilidade
- Acoplamento desnecessário entre classes?
- Duplicação de código (DRY)?
- Abstrações apropriadas (nem demais, nem de menos)?
- Magic numbers ou strings sem constantes?

### 3. Aderência aos Padrões
- Segue `.github/copilot-instructions.md`?
- Segue as instructions específicas do tipo de arquivo?
- Arquitetura respeitada (camadas, dependências corretas)?

### 4. Qualidade dos Testes
- Testes cobrem critérios de aceite da spec?
- Testes cobrem casos de borda?
- Asserções verificam comportamento, não implementação?
- Mocks usados com critério (não exagero)?
- Nomes de testes descritivos (`deve_X_quando_Y`)?

### 5. Coisas que Tech Lead Repara
- Decisões de design que vão envelhecer mal
- Lugares onde o "código funciona, mas..."
- Trade-offs implícitos que mereciam ADR
- Oportunidades de simplificação

## Formato da Saída

Estruture em três níveis:

**🔴 Bloqueadores** (devem ser corrigidos antes do merge)
- Problemas de segurança, bugs claros, violações arquiteturais graves

**🟡 Importantes** (forte recomendação corrigir)
- Más práticas, falta de tratamento de erros, testes fracos

**🟢 Sugestões** (melhoria, mas não obrigatório)
- Refinamentos de estilo, refactor opcional, ideias para o futuro

Para cada apontamento, inclua: arquivo, linha (se aplicável), problema, sugestão.

**Não escreva código corrigido inline.** Aponte o problema e sugira a direção da correção.
