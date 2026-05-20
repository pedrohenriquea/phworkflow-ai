---
description: 'Roda checklist de qualidade antes de abrir PR'
---

Revise o código da feature aberta nesta conversa contra o checklist abaixo. Para cada item problemático, aponte o problema e **sugira um teste que capturaria o bug**.

## Segurança & Compliance (instituição financeira)
- [ ] Nenhum dado sensível em log (CPF, conta, valores, tokens) — sempre mascarado
- [ ] Toda entrada externa validada e sanitizada
- [ ] Stack trace não vaza para o cliente nas respostas de erro
- [ ] Sem secrets hardcoded (verificar properties, código, comentários)
- [ ] Autorização verificada explicitamente em cada endpoint
- [ ] Auditoria/rastreabilidade de operações sensíveis

## Robustez
- [ ] Timeouts configurados em **todas** as chamadas externas
- [ ] Circuit breaker ou retry com backoff onde apropriado
- [ ] Idempotência tratada (chave de idempotência, deduplicação)
- [ ] Transações com escopo correto (sem dirty read, lost update)
- [ ] Recursos fechados em try-with-resources

## Adversarial — tente quebrar
- [ ] Que inputs maliciosos podem causar comportamento inesperado? (injection, XSS via API, etc)
- [ ] Há race condition possível em ambiente concorrente?
- [ ] O que acontece se serviço externo demorar muito ou cair?
- [ ] Edge cases numéricos (overflow, divisão por zero, precisão decimal em BigDecimal)?
- [ ] Edge cases de coleções (vazia, null, com 1 elemento, com muitos)?
- [ ] Comportamento sob carga (timeouts em cascata, exaustão de pool)?

## Observabilidade
- [ ] Logs estruturados com correlation-id propagado
- [ ] Métricas expostas (contadores de sucesso/erro, latência)
- [ ] Erros logados com contexto suficiente para diagnóstico
- [ ] Sem log excessivo em hot path (performance)

## Código
- [ ] Sem `TODO`/`FIXME` esquecidos
- [ ] Cobertura ≥ 80% linhas / 70% branches
- [ ] Build limpo (SpotBugs, PMD, Checkstyle sem warnings novos)
- [ ] Sem dependências novas sem aprovação
- [ ] Sem código comentado deixado no PR

## Aderência aos Padrões
- [ ] Segue `.github/copilot-instructions.md`
- [ ] Constructor injection (sem field injection)
- [ ] Exceptions estendem hierarquia do projeto
- [ ] Endpoints em `/api/v1/` com versionamento correto

## Formato da Saída

Para cada item com problema:
1. **O problema** (qual item, em qual arquivo/linha)
2. **Por que é problema** (impacto potencial)
3. **Teste sugerido** (código ou descrição clara do teste que capturaria)
4. **Sugestão de correção** (curta)

Itens conformes: liste rapidamente como "OK" sem detalhar.
