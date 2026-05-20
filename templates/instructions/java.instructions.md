---
applyTo: "**/*.java"
---

# Padrões Java

## Preferências de Estilo
- Use `records` para DTOs sempre que possível
- Prefira `var` apenas quando o tipo for óbvio pelo lado direito
- Use streams em vez de loops imperativos quando aumentar legibilidade
- Métodos curtos (< 30 linhas idealmente)
- Nomes em inglês para código, português permitido em DTOs de domínio brasileiro

## Optional
- Use `Optional` apenas como **retorno** de método
- **Nunca** como parâmetro
- **Nunca** como campo de classe
- Evite `.get()` sem `.isPresent()` ou `.orElse()`

## Imutabilidade
- Prefira objetos imutáveis sempre que possível
- Use `List.of()`, `Map.of()` para coleções imutáveis
- Records são imutáveis por padrão — prefira a classes com getters

## Exceções
- Customizadas estendem `BusinessException` (ou outra base do projeto)
- Nunca capture `Exception` genérica
- Nunca capture e ignore silenciosamente — sempre logar com contexto
- Re-throw com contexto adicionado, não engula a stack trace

## Injeção de Dependência
- **Constructor injection sempre.** Nunca `@Autowired` em campo.
- Use `final` em todos os campos injetados
- Lombok `@RequiredArgsConstructor` é aceito para reduzir boilerplate

## Datas e Horários
- Use `java.time` (`LocalDate`, `LocalDateTime`, `Instant`)
- Nunca use `Date`, `Calendar`, `SimpleDateFormat`
- Persista timestamps em UTC, converta para timezone do usuário só na apresentação

## Valores Monetários
- **Sempre `BigDecimal`** para valores monetários
- Nunca `double` ou `float`
- Sempre defina `scale` e `RoundingMode` explicitamente em operações
