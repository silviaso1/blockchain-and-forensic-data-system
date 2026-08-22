# SICCAD - Sistema de Cadeia de Custódia Digital com Blockchain e Auditoria Inteligente

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-0db7ed?style=for-the-badge&logo=docker&logoColor=white)

Uma plataforma integrada para gestão, rastreabilidade e garantia de integridade de evidências digitais em investigações forenses. O sistema atende aos requisitos legais de cadeia de custódia (alinhado ao Pacote Anticrime, Lei nº 13.964/2019) utilizando uma arquitetura em microsserviços containerizados que combina controle de acesso baseado em papéis (RBAC), imutabilidade criptográfica via Blockchain e um motor autônomo de auditoria e detecção de anomalias.

---

## Arquitetura do Sistema

O ecossistema é orquestrado via Docker Compose e dividido em três containers independentes, garantindo isolamento de processos e o princípio do privilégio mínimo:

*   **forense-db**: Banco de dados relacional PostgreSQL 15 para armazenar metadados de casos, usuários, histórico de transições de custódia e logs de acesso.
*   **forense-api**: Backend desenvolvido em Java Spring Boot responsável pelo controle de regras de negócio, autenticação/autorização RBAC, manipulação de arquivos e registro na Blockchain.
*   **forense-monitor**: Engine assíncrona em Python (FastAPI/httpx) responsável por realizar varreduras contínuas no banco de dados e nos artefatos físicos para detectar violações de integridade, acessos suspeitos e ataques de força bruta.

---

## Configuração do Ambiente

### 1. Clonar o Repositório

No terminal, execute o comando abaixo para clonar o repositório e acessar a pasta do projeto:

```bash
git clone https://github.com/seu-usuario/siccad.git
cd siccad
```

### 2. Configurar as Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto manualmente e adicione as variáveis de ambiente necessárias.

**No Linux / macOS:**
```bash
touch .env
```

**No Windows (Command Prompt):**
```cmd
type nul > .env
```

Abra o arquivo `.env` e insira as seguintes configurações:

```env
# Configurações do Banco de Dados PostgreSQL
POSTGRES_USER=forense_user
POSTGRES_PASSWORD=forense_pass
POSTGRES_DB=forense_db

# Integração da API Java
JAVA_API_URL=http://forense-api:8080/api/notificacoes
```

### 3. Execução dos Serviços Backend (Docker)

Para construir as imagens e iniciar os serviços do Backend (API Java, Motor de Auditoria Python e PostgreSQL) em segundo plano (*detached mode*), execute na raiz do repositório:

```bash
docker-compose up -d --build
```

### 4. Configuração e Execução do Frontend

O frontend foi desenvolvido utilizando **React** com **Vite** e deve ser executado **localmente**.

1. Navegue até a pasta do frontend:
   ```bash
   cd frontend
   ```

2. Instale as dependências do projeto:
   ```bash
   npm install
   # ou se utilizar yarn / pnpm:
   # yarn install
   # pnpm install
   ```

3. Inicie o servidor de desenvolvimento do Vite:
   ```bash
   npm run dev
   ```

4. O projeto estará disponível no endereço `http://localhost:5173`.


### 5. Verificação de Status dos Containers

Para confirmar se todos os serviços do container estão em execução corretamente:

```bash
docker ps
```

### Portas Expostas no Ambiente Local (`localhost`)

| Serviço | URL / Endereço Local | Descrição / Observação |
| :--- | :--- | :--- |
| **Frontend (React + Vite)** | `http://localhost:5173` | Executado localmente via Node (`npm run dev`) |
| **API Java** | `http://localhost:8080` | Executado via Docker |
| **Monitor Python** | `http://localhost:8000` | Executado via Docker |
| **PostgreSQL** | `localhost:5433` | Mapeado internamente para a porta `5432` |



### 6. Visualização de Logs em Tempo Real

Acompanhe a saída e os logs dos containers Docker para monitoramento ou depuração:

### Logs de todos os containers
```bash
docker-compose logs -f
```

### Logs apenas do motor de auditoria Python
```bash
docker logs -f forense-monitor
```

---

## Tecnologias Utilizadas

| Camada | Tecnologia | Descrição |
| :--- | :--- | :--- |
| **API Backend** | Java 17 / Spring Boot | Regras de negócio, RBAC, transações ACID e gestão da cadeia de custódia. |
| **Motor de Monitoramento** | Python 3.10 / FastAPI | Motor de auditoria contínua, análise heurística e integração HTTP. |
| **Banco de Dados** | PostgreSQL 15 | Persistência relacional de dados transacionais e registros de auditoria. |
| **Frontend** | React com Tailwindcss | Estrutura e estilização das telas doo sistema. |
| **Imutabilidade** | Blockchain / Ledger JSON PoC | Encadeamento criptográfico (SHA-256) com suporte a nós da RBB (Hyperledger Besu). |
| **Infraestrutura** | Docker / Docker Compose | Isolamento em containers, gerenciamento de volumes e rede virtualizada. |

---

## Estrutura do Repositório

```text
.
├── api/                    # Código-fonte da aplicação Java Spring Boot
├── frontend/               # Telas da aplicação
├── monitor/                # Código-fonte do motor de auditoria em Python
│   ├── main.py             # Script principal de varredura e alertas
│   └── logger.py           # Módulo de persistência de logs em JSON
├── shared_data/            # Volume compartilhado no Docker (Blockchain e Logs)
├── docker-compose.yml      # Configuração de orquestração dos containers
├── .env.example            # Modelo de variáveis de ambiente
└── README.md               # Documentação do projeto
```

## Licença
[MIT](https://choosealicense.com/licenses/mit/)