
# 🧾 Error Documentation – Learning Reference  

### 📘 Project: *Multi-container Docker Compose (Flask + Postgres)*  
### 🧑‍💻 Author: Kajal  

---

## 🧩 Error 1: `dependency failed to start: container multi-container-app-docker-compose-db-1 is unhealthy`

### 🧠 Cause:
PostgreSQL container healthcheck failed — database did not become “ready” in time.

### 🔍 Detailed Error:
```
dependency failed to start: container multi-container-app-docker-compose-db-1 is unhealthy
```

### ⚙️ Root Cause:
- Wrong database credentials or environment variables.  
- Healthcheck command `pg_isready` failed.  
- Volume mounted from previous Postgres version.

### 🧰 Fix:
1. Stop all containers:  
   ```bash
   docker-compose down -v
   ```
2. Delete old volume if corrupt:  
   ```bash
   docker volume rm multi-container-app-docker-compose_postgres_data_new
   ```
3. Start fresh:
   ```bash
   docker-compose up --build
   ```

---

## 🧩 Error 2: `error: src refspec main does not match any`

### 🧠 Cause:
You tried to push to a branch (`main`) that doesn’t exist locally.

### ⚙️ Fix:
```bash
git branch -M main
git push -u origin main
```

---

## 🧩 Error 3:  
`fatal: unable to access 'https://github.com:<user-name>/multi-container-docker-compose.git/': URL rejected: Port number was not a decimal number between 0 and 65535`

### 🧠 Cause:
The Git remote URL was incorrect — colon (`:`) used instead of slash (`/`).

### ⚙️ Fix:
```bash
git remote set-url origin https://github.com/<user-name>/multi-container-docker-compose.git
```

---

## 🧩 Error 4:  
`! [rejected] main -> main (fetch first)`

### 🧠 Cause:
Remote repository has commits you don’t have locally.

### ⚙️ Fix:
```bash
git pull origin main --rebase
git push -u origin main
```

*(Use `--force` only if you want to overwrite remote changes)*

---

## 🧩 Error 5:  
`.env` credentials exposed

### 🧠 Cause:
Sensitive credentials (DB username/password) stored directly in `.env` file and committed to Git.

### ⚙️ Fix:
1. Add `.env` to `.gitignore`
2. Use a separate `db-variables.env` locally:
   ```bash
   docker compose --env-file db-variables.env up
   ```
3. Never push `.env` files to GitHub.

---

## 🧩 Error 6:  
`Error: in 18+, these Docker images are configured to store database data in a format which is compatible with pg_ctlcluster`

### 🧠 Cause:
PostgreSQL image changed data directory format after version 18.

### ⚙️ Fix:
- Remove old volume:
  ```bash
  docker volume rm postgres_data_new
  ```
- Then rebuild container.

---

