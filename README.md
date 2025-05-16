# FastAPI CRUD with MySQL Docker Setup

A REST API for managing superheroes with CRUD operations, built with FastAPI, SQLModel, and MySQL in Docker.

## Project Structure
```
├── docker-compose.yml     # Docker configuration for MySQL
├── pyproject.toml        # Poetry dependencies
├── main.py               # FastAPI application and routes
├── README.md             # This documentation
└── .env                  # Environment variables (create this file)
```

## Prerequisites
- [Docker](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Python 3.11+ (3.13 recommended)
- [Poetry](https://python-poetry.org/docs/#installation)

## Setup

### 1. Clone the repository
```bash
git clone https://github.com/your-repo-url.git
cd crud-with-mysql
```

### 2. Configure Environment
Create `.env` file:
```env
USER_DB=<USER_DB>
PASSWORD=<PASSWORD>
HOST_DB=<HOST_DB>
NAME_DB=<NAME_DB>
```

### 3. Start MySQL Container
```bash
docker-compose up -d
```

### 4. Install Python Dependencies
```bash
poetry install
```

### 5. Run FastAPI Application
```bash
poetry run fastapi dev main.py
```

## API Endpoints
| Method | Endpoint         | Description                |
|--------|------------------|----------------------------|
| GET    | /                | Hello World                |
| POST   | /heroes/         | Create new hero            |
| GET    | /heroes/         | List all heroes            |
| GET    | /heroes/{hero_id}| Get hero by ID             |
| PATCH  | /heroes/{hero_id}| Update hero                |
| DELETE | /heroes/{hero_id}| Delete hero                |

## Usage Examples

### Create Hero
```bash
curl -X POST "http://localhost:8000/heroes/" \
-H "Content-Type: application/json" \
-d '{"name": "Ironman", "age": 53, "secret_name": "Tony Stark"}'
```

### List Heroes
```bash
curl "http://localhost:8000/heroes/"
```

## Database Management

### Connect to MySQL CLI
```bash
docker-compose exec mysql-db mysql -u root -p
```

### Verify Database
```sql
CREATE DATABASE test;
USE test;
SHOW TABLES;
SELECT * FROM hero;
```

## Docker Commands

| Command                          | Description                          |
|----------------------------------|--------------------------------------|
| `docker-compose up -d`           | Start MySQL container                |
| `docker-compose down`            | Stop and remove container            |
| `docker-compose logs mysql-db`   | View database logs                   |
| `docker volume inspect mysql_data` | Check persisted data location      |

## Configuration Details

### MySQL Docker Settings (docker-compose.yml)
- **Port**: 3306 (host) → 3306 (container)
- **Root Password**: `your-password`
- **Persistent Volume**: `mysql_data`
- **Auto-created Database**: `test`

### FastAPI Application
- **Host**: `http://localhost:8000`
- **Interactive Docs**: `http://localhost:8000/docs`
- **Auto-reload**: Enabled in development

## Troubleshooting

1. **Port Conflicts**:
   - Ensure no local MySQL instance is running on port 3306
   - Modify `docker-compose.yml` ports to `3306:3306` if needed

2. **Connection Errors**:
   - Verify `.env` matches docker-compose credentials
   - Wait 30-60 seconds after container start for MySQL initialization

3. **Database Persistence**:
   - Data survives container restarts via `mysql_data` volume
   - To reset: `docker-compose down -v && docker-compose up -d`

4. **FastAPI Dependency Issues**:
   ```bash
   poetry lock --no-update
   poetry install
   ```

## Development
- **Code Formatting**: Uses Black (`poetry run black .`)
- **Environment**: Managed with Poetry virtual environment
- **SQL Logging**: Enabled in `main.py` (`echo=True` in engine creation)
