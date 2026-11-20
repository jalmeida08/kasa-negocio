# Cabeçalho
- name: Stack do projeto
- status: **PROPOSTA — aguardando confirmação**
- description: Tecnologias, versões e comandos padrão. Fonte única para todos os agentes.

> Enquanto o status for **PROPOSTA**, os agentes planejadores podem planejar com base nesta stack, mas os agentes executores **não iniciam código** até o status mudar para **CONFIRMADA**.
> Qualquer troca de tecnologia exige um ADR e a atualização deste arquivo.

# Backend
| Área | Tecnologia |
|---|---|
| Linguagem | Java 21 |
| Framework | Spring Boot 3.x, Spring Data JPA, Spring Security |
| Arquitetura | Monólito modular (ver `backend/backend.md` seção 1), preparado para extração em serviços |
| Build | Gradle (Groovy DSL) |
| Banco relacional | MySQL 8.4 |
| Banco de documentos | MongoDB 8 — **opcional**, somente com ADR aprovado |
| Migrações | Flyway (MySQL), Mongock (MongoDB, se usado) |
| Mensageria | RabbitMQ (Kafka somente com ADR aprovado) |
| Identidade | Keycloak (OAuth2/OIDC) atrás de um **BFF**, que entrega a sessão ao frontend em cookie `HttpOnly` |
| Arquivos | Object storage compatível com S3 (MinIO em dev) |
| Documentação de API | springdoc-openapi (Swagger UI) |
| Testes | JUnit 5, Mockito, Testcontainers, Cucumber, JaCoCo |
| Observabilidade | Actuator, Micrometer, Prometheus, Grafana, logs em JSON |

# Frontend
| Área | Tecnologia |
|---|---|
| Framework | Angular 22 (standalone components, signals, control flow `@if`/`@for`) |
| Runtime | Node 26.13.0 |
| Linguagem | TypeScript 6.x (compatível com o Angular 22) |
| Micro-frontends | Native Federation (`@angular-architects/native-federation`): 1 shell + remotes |
| Estado | Signals; NgRx Signal Store para estado compartilhado ou complexo |
| UI | Bootstrap 5 + ng-bootstrap (sem jQuery) |
| Cliente da API | Gerado de `docs/backend/api/openapi.yaml` com `openapi-generator` (typescript-angular) |
| Testes | Vitest (unitários), Playwright + playwright-bdd (E2E e Gherkin), `@axe-core/playwright` |
| Idioma | i18n em pt-BR |

# Infraestrutura e qualidade
| Área | Tecnologia |
|---|---|
| Containers | Docker, Docker Compose |
| Orquestração | Kubernetes (local: kind ou minikube) — somente quando o Compose não bastar |
| IaC | Terraform — somente quando houver nuvem definida |
| CI/CD | Jenkins |
| Proxy | Nginx |
| Monitoramento | Prometheus, Grafana, `mysqld_exporter` |
| Qualidade e segurança | ESLint, Prettier, SonarQube, Snyk |
| Mobile | A definir (fora do escopo inicial) |

# Comandos padrão
| Ação | Backend | Frontend |
|---|---|---|
| Build | `./gradlew build` | `npm run build` |
| Testes | `./gradlew test` | `npm run test` |
| Cobertura | `./gradlew jacocoTestReport jacocoTestCoverageVerification` | `npm run test -- --coverage` |
| Lint | (incluído no build) | `npm run lint` |
| E2E | — | `npm run e2e` |

# Padrões de nomenclatura
- Pacote base do backend: `{grupo}.{projeto}` (ex.: `br.com.kasa`), definido pelo nome em `negocio/negocio.md`.
- Migrações Flyway: `V{AAAAMMDDHHmm}__{descricao}.sql`.
- Realm e clients do Keycloak: `{projeto}` e `{projeto}-bff`.
