# 📁 E-Commerce Configuration Repository

Centralized configuration files for all microservices.

## Structure

```
ecom-config-repo/
├── gateway/
│   └── application.yml          # Gateway-specific config
├── identity-service/
│   └── application.yml          # Identity service config
├── shared/
│   └── application.yml          # Common config for all services
└── README.md
```

## How It Works

Spring Cloud Config Server reads from this repository and serves configurations based on:
- **Service name** (matches folder name)
- **Profile** (default, dev, prod, etc.)

## Configuration Naming

- `{service-name}/application.yml` - Default profile
- `{service-name}/application-dev.yml` - Development profile
- `{service-name}/application-prod.yml` - Production profile
- `shared/application.yml` - Merged with all services

## Adding a New Service

1. Create a folder named after your service (e.g., `catalog-service/`)
2. Add `application.yml` with service-specific configuration
3. Config Server will automatically serve it at: `/catalog-service/default`

## Environment Variables

Use `${VARIABLE_NAME:default-value}` syntax:
```yaml
spring:
  datasource:
    url: jdbc:postgresql://${DB_HOST:localhost}:${DB_PORT:5432}/ecom_iam
```

## Secrets

⚠️ **Do NOT commit sensitive data (passwords, API keys) directly in this repo.**

Options:
1. Use environment variables (recommended)
2. Use Spring Cloud Config Server encryption
3. Use external secret management (Vault, AWS Secrets Manager)

## Examples

### Service-Specific Config
```yaml
# gateway/application.yml
server:
  port: 8080
```

### Shared Config (applied to all)
```yaml
# shared/application.yml
spring:
  kafka:
    bootstrap-servers: localhost:9092
```

## Testing Config Access

Once Config Server is running:
```bash
# Get gateway config
curl http://localhost:8888/gateway/default

# Get identity-service config
curl http://localhost:8888/identity-service/default
```

