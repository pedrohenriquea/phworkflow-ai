# Contexto do Projeto

> **Como usar este template:** copie para `.github/copilot-instructions.md` na raiz do seu repo e substitua os trechos entre `[colchetes]`. Mantenha o arquivo **conciso** — instruções longas perdem efeito.

## Visão Geral
- **Serviço:** [nome do serviço]
- **Domínio:** [ex: processamento de pagamentos PIX]
- **Criticidade:** [ex: missão-crítica, SLA 99.95%]

## Stack
- Java 21, Spring Boot 3.x
- Maven (ou Gradle)
- PostgreSQL + Flyway
- Kafka para eventos
- [outras libs relevantes]

## Arquitetura
- Padrão: [Hexagonal / Clean / MVC]
- Camadas: controller → service → repository
- DTOs sempre separados de entidades JPA
- Mapeamento via MapStruct
- Constructor injection (nunca `@Autowired` em campo)

## Padrões Obrigatórios
- Exceptions customizadas estendem `BusinessException`
- Logs estruturados JSON via SLF4J (nunca `System.out`)
- Endpoints REST seguem `/api/v1/{recurso}`
- Validação via Bean Validation (`@Valid`, `@NotNull`, etc)
- Toda chamada externa tem timeout configurado e fallback
- Testes: JUnit 5 + Mockito + AssertJ
- Cobertura mínima: 80% linhas / 70% branches

## Segurança (CRÍTICO — instituição financeira)
- **Nunca logar dados sensíveis** (CPF, conta, valores) — usar máscara
- Toda entrada externa validada e sanitizada
- SQL sempre via `PreparedStatement` ou JPA (nunca concatenação)
- Secrets via Vault/AWS Secrets Manager, nunca em código ou properties
- Autenticação via [OAuth2 / JWT / mTLS]

## Estrutura de Pastas
```
src/main/java/com/empresa/servico/
├── application/       # casos de uso / services
├── domain/            # entidades de domínio, exceptions
├── infrastructure/    # adapters (DB, Kafka, HTTP)
└── interfaces/        # controllers REST, listeners Kafka
```

## NÃO fazer
- `Lombok @Data` em entidades JPA (causa problemas com `equals`/`hashCode`)
- Field injection (`@Autowired` em campo) — sempre constructor
- Endpoints sem autenticação explícita
- `Optional` como parâmetro de método
- Capturar `Exception` genérica

## Glossário do Domínio
- **[Termo]:** [definição curta]
- **[Termo]:** [definição curta]

## Referências Adicionais
- Padrões detalhados: ver `.github/instructions/` (aplicados automaticamente conforme arquivo)
- Specs de features: ver `specs/`
- Decisões arquiteturais: ver `docs/adr/`
