# API OS - Prevenir

API REST responsável pelo gerenciamento de Ordens de Serviço da **Prevenir**, empresa especializada em instalações elétricas e serviços de combate a incêndio.

Fornece autenticação, cadastro de clientes e técnicos, abertura, atribuição, acompanhamento e finalização de O.S., com registro de data, horário e responsável em cada etapa.

---

## Estrutura do Projeto

```
api-os/
├── .github/workflows/      → Pipeline de CI
├── src/
│   ├── main/
│   │   ├── java/com/prevenir/apios/
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── application-dev.properties
│   │       └── application-prod.properties
│   └── test/
├── .env.example            → Modelo das variáveis de ambiente
├── docker-compose.yml      
├── lombok.config
├── spotbugs-exclude.xml
├── pom.xml
└── README.md
```

---

## Stack

| Camada | Tecnologia |
|---|---|
| Linguagem | Java 25 |
| Framework | Spring Boot 4 |
| Segurança | Spring Security + JWT |
| Banco de dados | PostgreSQL 16 |
| Persistência | Spring Data JPA (Hibernate) |
| Infra | Docker |
| Análise estática | Checkstyle, PMD, SpotBugs |
| Cobertura de testes | JaCoCo |
| CI | GitHub Actions |

---

## Pré-requisitos

- Java 25
- Docker e Docker Compose

> Não é necessário instalar o Maven, o projeto utiliza o Maven Wrapper (`mvnw`).

---

### 1. Configurar as variáveis de ambiente

Crie o arquivo `.env` na raiz do projeto com base no `.env.example`:

```bash
cp .env.example .env
```

Preencha os valores:

```env
# POSTGRES
DB_HOST=localhost
DB_PORT=5433
DB_NAME=os_db
DB_USER=
DB_PASSWORD=

# BACKEND
SPRING_PROFILE=dev

# SEGURANÇA
JWT_SECRET=
JWT_EXPIRATION=28800000
```

> O `.env` **nunca** deve ser commitado. Ele já está no `.gitignore`.

#### Gerando o `JWT_SECRET`

**PowerShell (Windows):**

```powershell
$bytes = New-Object byte[] 64
[Security.Cryptography.RandomNumberGenerator]::Create().GetBytes($bytes)
[Convert]::ToBase64String($bytes)
```

**Bash (Linux / macOS / Git Bash):**

```bash
openssl rand -base64 64
```

---

### 2. Subir o banco de dados

```bash
docker compose up -d
```

O PostgreSQL estará disponível em: `localhost:5433`

> As credenciais do banco são lidas automaticamente do `.env`.

Para conferir se está rodando:

```bash
docker ps
```

---

### 3. Rodar a API

```bash
./mvnw spring-boot:run
```

No Windows:

```powershell
.\mvnw spring-boot:run
```

A API estará disponível em: `http://localhost:8081`

---

## Perfis

| Perfil | Uso | `ddl-auto` | SQL no console |
|---|---|---|---|
| `dev` | Desenvolvimento local | `update` | Sim |
| `prod` | Servidor de produção | `validate` | Não |

O perfil é definido pela variável `SPRING_PROFILE` no `.env`.

---

## Análise estática e testes

Para compilar, rodar os testes e executar todas as análises:

```bash
./mvnw verify
```

| Ferramenta | Função | Relatório |
|---|---|---|
| Checkstyle | Padrão de código | `target/checkstyle-result.xml` |
| PMD | Más práticas | `target/pmd.xml` |
| SpotBugs | Bugs prováveis | `target/spotbugsXml.xml` |
| JaCoCo | Cobertura de testes | `target/site/jacoco/index.html` |

> A propriedade `analise.falhar` no `pom.xml` define se violações quebram o build (`true`) ou apenas geram avisos (`false`).

---

## Integração Contínua

A cada **push** ou **pull request** nas branches `develop` e `main`, o GitHub Actions:

1. Sobe um PostgreSQL temporário
2. Compila o projeto
3. Executa os testes
4. Roda Checkstyle, PMD, SpotBugs e JaCoCo
5. Disponibiliza os relatórios para download

O secret `JWT_SECRET_TEST` precisa estar configurado em **Settings → Secrets and variables → Actions**.

---

## Branches

| Branch | Uso |
|---|---|
| `main` | Versão estável |
| `develop` | Desenvolvimento |

---

## Equipe

| Nome | Papel                                                           |
|---|-----------------------------------------------------------------|
| Alice Ferreira Correia | Analista de Sistema Júnior / Desenvolvedora mobile e Full-Stack |

---

## Tecnologias

- Java 25
- Spring Boot 4
- Spring Security + JWT
- Spring Data JPA
- PostgreSQL
- Docker
- Lombok
- Checkstyle, PMD, SpotBugs, JaCoCo
- GitHub Actions

---

## Licença

© 2026 lliliss. Todos os direitos reservados.