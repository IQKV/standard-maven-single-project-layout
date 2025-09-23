# Spring Boot Application Template

A GitHub template for quickly bootstrapping Spring Boot applications with best practices.

## Overview

This template provides a standardized foundation for developing Java applications using Spring Boot with Maven's single-module layout. For detailed documentation, please refer to the [docs](./docs) directory.

## Quick Links

- [Project Overview](./docs/01-project-overview.md)
- [Getting Started Guide](./docs/02-getting-started.md)
- [Architecture Overview](./docs/03-architecture-overview.md)
- [Development Workflow](./docs/04-development-workflow.md)
- [Testing Guide](./docs/05-testing-guide.md)

## Key Features

- **Project Structure**: Standard Maven single-module layout
- **GitHub Integration**: Issue templates, labels, and workflows
- **Quality Tools**: Code formatting, linting, and testing setup
- **Documentation**: Community guidelines and contribution process

## Prerequisites

- Java 21 (OpenJDK)
- Maven
- Node.js & pnpm
- Docker & Docker Compose

## Quick Start

```bash
# Clone the repository
git clone https://github.com/dimdnk/standard-maven-single-project-layout.git my-service

# Navigate to project directory
cd my-service

# Install git hooks
pnpm install

# Start local dev services
docker compose up -d

# Run the application
mvn spring-boot:run -Dspring-boot.run.profiles=local -P dev
```

For more detailed instructions, configuration options, and development guidelines, please refer to the [documentation](./docs).

## License

This project is licensed under the Apache License. See the [LICENSE](LICENSE) file for details.

## Contributing

Please read our [Contributing Guidelines](.github/CONTRIBUTING.md) and [Code of Conduct](.github/CODE_OF_CONDUCT.md).
