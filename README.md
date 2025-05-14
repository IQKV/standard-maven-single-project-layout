# 🚀 Servicename Backend

<a name="description"></a>
## 📜 Description

A service created for implementing the servicename API...

> ##### 🛠️ Technology stack
>
> Java 21, Maven, Spring Boot

<a name="instructions"></a>
## 📒 Instructions

> [!TIP]
> #### Install Prerequisites:
> - [Node LTS version](https://nodejs.org/en/blog/release/v22.15.0/), [Git](https://git-scm.com/), [Docker](https://www.docker.com/get-started/), [Docker Compose](https://docs.docker.com/compose/)
> - [OpenJDK LTS](https://aws.amazon.com/corretto/), [Maven](https://maven.apache.org/install.html)

### 🔺 Local development

```bash
# Clone the repository
git clone https://github.com/expertness/standard-maven-project-layoutt.git my-service

# Navigate to project directory
cd my-service

# Install git hooks
npm install

# Start local dev services dependencies
docker compose -f compose.yaml up -d

#  Run the application in dev mode
mvn spring-boot:run -Dspring-boot.run.profiles=local -P dev

```

### 🔺 Tests

The application contains different test layers according to the [Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html).

The base of the pyramid is made up of unit tests. They should make up the biggest part of an automated test suite.

To run JUnit tests use:

```bash
mvn clean test
```

The next layer, integration tests, test all places where your application serializes or deserializes data.
Application Service's REST API, Repositories, or calling third-party services are good examples.

Run this to enable integration tests, powered by testcontainers:

```bash
mvn clean verify -P use-testcontainers
```
The minimum percentage of code coverage required for the workflow to pass is **80**.

<a name="license"></a>
## 📑 License

This project is licensed under the Apache License. See the [LICENSE](LICENSE) file for more details.
