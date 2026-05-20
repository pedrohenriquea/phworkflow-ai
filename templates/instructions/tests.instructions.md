---
applyTo: "**/*Test.java,**/*IT.java,**/*Tests.java"
---

# Padrões de Teste

## Stack
- JUnit 5 (Jupiter)
- Mockito para mocks
- AssertJ para asserções (preferir sobre JUnit assertions)
- TestContainers para testes de integração com banco/Kafka

## Nomenclatura
- Classes: `[ClasseSendoTestada]Test` (unit) ou `[ClasseSendoTestada]IT` (integration)
- Métodos: `deve_[comportamento]_quando_[condição]`
  - Exemplo: `deve_lancar_excecao_quando_saldo_insuficiente`
- Use `@DisplayName` em português para legibilidade nos relatórios

## Estrutura (AAA)
```java
@Test
@DisplayName("Deve calcular juros compostos corretamente")
void deve_calcular_juros_compostos_corretamente() {
    // Arrange (Given)
    var capital = new BigDecimal("1000.00");
    var taxa = new BigDecimal("0.05");
    var periodos = 12;

    // Act (When)
    var resultado = calculadora.calcular(capital, taxa, periodos);

    // Assert (Then)
    assertThat(resultado)
        .isEqualByComparingTo(new BigDecimal("1795.86"));
}
```

## Asserções
- **AssertJ sempre.** `assertThat(x).isEqualTo(y)` em vez de `assertEquals(y, x)`
- Para `BigDecimal`: use `isEqualByComparingTo` (compara valor, não scale)
- Para coleções: `containsExactly`, `containsExactlyInAnyOrder`, `hasSize`
- Para exceções: `assertThatThrownBy(() -> ...).isInstanceOf(...).hasMessage(...)`

## Mocks
- Use `@Mock` + `@InjectMocks` ou builder manual
- **Não mocke o que você não possui** (libs externas) — use wrappers próprios
- Verify só quando o efeito colateral é o ponto do teste, não como redundância

## Cobertura Mínima
- Linhas: ≥ 80%
- Branches: ≥ 70%
- **Mas:** cobertura é necessária, não suficiente. Um teste fraco com 100% de cobertura não vale nada.

## Testes de Integração
- Use `@SpringBootTest` + TestContainers para banco/Kafka reais
- `@AutoConfigureMockMvc` para testar endpoints REST
- Limpe estado entre testes (`@Transactional` + rollback, ou `@DirtiesContext`)
- Não use H2 para testes que precisam validar comportamento PostgreSQL-específico

## Anti-padrões
- Testes que dependem de ordem de execução
- Asserções em campos não relevantes ao comportamento testado
- Mocks que retornam exatamente o que o teste espera (testa nada)
- Testes sem assertions explícitas (só "não lançou exceção")
- Sleep/Thread.sleep para "esperar algo acontecer" — use Awaitility
