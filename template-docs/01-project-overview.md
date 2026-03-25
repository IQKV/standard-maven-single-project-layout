# Project Overview

## Introduction

`standard-maven-single-project-layout` is a GitHub repository template for bootstrapping Spring Boot microservices under the IQKV organization. It inherits from the `com.iqkv:boot-parent-pom` BOM and enforces a consistent project structure, quality toolchain, and Git workflow out of the box.

## Key Features

- **Custom Parent POM**: Inherits `com.iqkv:boot-parent-pom` — versions for all plugins and third-party libraries are managed centrally
- **Spring Boot 3 + Java 21**: Pre-wired for modern Spring Boot with Modulith support
- **Spring Modulith**: Module boundary enforcement via `spring-modulith-starter-core` and dedicated test profiles
- **Observability**: Micrometer + Prometheus metrics (`micrometer-registry-prometheus`) and structured JSON logging via `logstash-logback-encoder`
- **Git Build Info**: `git-commit-id-maven-plugin` injects build metadata (commit SHA, build time) into the `/info` actuator endpoint automatically
- **Architecture Tests**: ArchUnit (`archunit-junit5-api/engine`) + `TechnicalStructureTest` enforce layered architecture rules at compile-time
- **Code Quality**: Checkstyle (via parent POM), JaCoCo (90% instruction/line/branch coverage gates), optional Qulice via `use-qulice` profile
- **Git Workflow**: Husky + Commitlint + `lint-staged` (oxfmt + Stylelint) + Conventional Commits enforced by CI
- **Release Automation**: `release-it` with `@release-it/conventional-changelog` for automated changelog and version bumps
- **Docker/SonarQube**: `compose.yaml` provides a local SonarQube 25.3 instance for code analysis
- **GitHub Automation**: 6 pre-built workflow files, 7 issue templates, 20 issue labels, community health files

## Project Structure

```
project-root/
├── .github/
│   └── workflows/
│       ├── use-template.yml              # Bootstraps new repos created from template
│       ├── build-nodejs-project.yml      # Lint/format CI for Node.js toolchain
│       ├── check-commit-message.yml      # Commitlint enforcement on push
│       ├── check-pr-title.yml            # Conventional Commits PR title check
│       ├── check-markdown-links.yml      # Markdown link validity check
│       └── auto-approve-dependabot-pr.yml
├── src/
│   ├── main/
│   │   ├── java/com/iqkv/servicename/
│   │   │   ├── shared/                   # Shared kernel (cross-cutting concerns)
│   │   │   │   ├── domain/               # Shared domain primitives
│   │   │   │   ├── exception/            # Common exceptions
│   │   │   │   └── util/                 # Utility classes
│   │   │   ├── infrastructure/           # Infrastructure layer
│   │   │   │   ├── config/               # Spring configuration
│   │   │   │   ├── security/             # Security implementation
│   │   │   │   ├── persistence/          # JPA repositories, adapters
│   │   │   │   └── messaging/            # Event publishers, message brokers
│   │   │   ├── [bounded-context]/        # e.g. user, order, payment
│   │   │   │   ├── domain/               # Domain layer (core business logic)
│   │   │   │   │   ├── model/            # Aggregates, entities, value objects
│   │   │   │   │   ├── service/          # Domain services
│   │   │   │   │   └── event/            # Domain events
│   │   │   │   ├── application/          # Application layer
│   │   │   │   │   ├── service/          # Application services (use cases)
│   │   │   │   │   ├── dto/              # Data transfer objects
│   │   │   │   │   └── port/             # Ports (interfaces for adapters)
│   │   │   │   └── adapter/              # Adapters (interface layer)
│   │   │   │       ├── in/
│   │   │   │       │   └── rest/         # REST controllers
│   │   │   │       └── out/
│   │   │   │           └── persistence/  # Repository implementations
│   │   │   └── ServicenameApplication.java   # Spring Boot entry point (UTC timezone forced)
│   │   └── resources/
│   │       ├── application.yml               # Base config with git build info placeholders
│   │       ├── application-dev.yml           # Dev profile overrides
│   │       ├── banner.txt                    # Custom startup banner
│   │       ├── logback-spring.xml            # Structured JSON logging (logstash encoder)
│   │       └── db/changelog/                 # Database migrations (Liquibase)
│   └── test/
│       └── java/com/iqkv/servicename/
│           ├── [bounded-context]/
│           │   ├── domain/                       # Domain model tests
│           │   ├── application/                  # Application service tests
│           │   └── adapter/                      # Adapter tests
│           ├── architecture/                     # ArchUnit tests
│           │   └── TechnicalStructureTest.java   # ArchUnit layered arch rules
│           ├── ServicenameApplicationTests.java  # Context load smoke test
│           ├── ModulithTest.java                 # Spring Modulith module verification
│           ├── ModulithIntegrationTest.java      # Modulith integration tests
│           ├── ModulithScenarioTest.java         # Modulith scenario tests
│           └── IntegrationTest.java              # Base integration test class
├── .mvn/                                 # Maven wrapper configuration
├── .husky/                               # Git hook scripts
├── compose.yaml                          # Local SonarQube 25.3 dev service
├── package.json                          # Node.js toolchain (husky, commitlint, oxfmt, release-it)
├── pom.xml                               # Maven build — inherits com.iqkv:boot-parent-pom
├── commitlint.config.js                  # Commitlint rules
├── lombok.config                         # Lombok global config
├── AGENTS.md                             # Repository guidelines & agent instructions
└── template-docs/                        # This documentation
```

**Key DDD Concepts:**

- **Bounded Context**: Logical boundary for a specific domain model (e.g., user, order, payment)
- **Domain Layer**: Core business logic — entities, value objects, aggregates, domain services
- **Application Layer**: Use cases, orchestration, DTOs, ports (interfaces)
- **Adapter Layer**: Implementation of ports (REST controllers, repository implementations)
- **Infrastructure**: Technical concerns (config, security, persistence framework)
- **Shared Kernel**: Common code shared across bounded contexts

## Technology Stack

| Layer                | Technology                                          |
| -------------------- | --------------------------------------------------- |
| Language             | Java 21 (OpenJDK)                                   |
| Framework            | Spring Boot 3.x (via `boot-parent-pom`)             |
| Modularity           | Spring Modulith                                     |
| Build                | Maven 3.9+, `boot-parent-pom` BOM                   |
| Logging              | Logback + `logstash-logback-encoder` (JSON)         |
| Metrics              | Micrometer + Prometheus                             |
| Containerization     | Docker Compose (SonarQube)                          |
| Node.js toolchain    | pnpm ≥ 10 / Node ≥ 22.15                            |
| Formatter            | oxfmt (Java + general), Prettier plugin             |
| CSS Linter           | Stylelint 17 + `stylelint-config-standard-scss`     |
| Commit validation    | Commitlint + Husky + `lint-staged`                  |
| Architecture testing | ArchUnit 0.x (JUnit 5)                              |
| Coverage             | JaCoCo — 90% instruction / line / branch minimum    |
| Code style           | Checkstyle (configured via parent POM)              |
| Release              | `release-it` + `@release-it/conventional-changelog` |

## Maven Profiles

| Profile         | Purpose                                                                        |
| --------------- | ------------------------------------------------------------------------------ |
| `default`       | Active by default — standard build                                             |
| `dev`           | Adds `spring-boot-devtools` for local development                              |
| `production`    | Placeholder for production-specific configuration                              |
| `modulith-test` | Runs only `*ModulithTest`, `*ModulithIntegrationTest`, `*ModulithScenarioTest` |
| `use-qulice`    | Enables Qulice static analysis (license header checks etc.)                    |

## Actuator / Info Endpoint

`application.yml` is pre-wired to expose Git build metadata:

```yaml
info:
    app:
        name: "@project.name@"
        description: "@project.description@"
        version: "@project.version@"
        git_commit: "${git.commit}"
        build_time: "${git.build_time}"
```

These placeholders are resolved at build time by `maven-resources-plugin` (`@` delimiters) and `git-commit-id-maven-plugin`.

## Next Steps

- See the [Getting Started Guide](02-getting-started.md) for setup instructions
- Review the [Architecture Overview](03-architecture-overview.md) for design details
- Check the [Development Workflow](04-development-workflow.md) for day-to-day practices
- Check the [Testing Guide](05-testing-guide.md) for testing strategy
