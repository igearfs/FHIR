# HAPI FHIR + PostgreSQL Setup (Docker)

## 📌 What This Does
This setup runs **HAPI FHIR** with a **PostgreSQL database**, ensuring that all data is **saved across restarts**. 🚀

## 🔹 Step 1: Clone This Repo (or Copy Files)
```bash
git clone https://your-repo-url.git
cd your-repo
```

## 🔹 Step 2: Start HAPI FHIR + PostgreSQL
```bash
docker-compose up -d
```
👉 Starts **HAPI FHIR** on → `http://localhost:8080/fhir`  
👉 Starts **PostgreSQL** with persistent data storage.

## 🔹 Step 3: Stop and Restart Everything
```bash
docker-compose down
docker-compose up -d
```
This **keeps your data** safe even after restarts! 🛠️

## 🔹 Step 4: Backup Your Data
To back up the FHIR database:
```bash
docker exec fhir-db pg_dump -U admin -d hapi > backup_fhir.sql
```

To restore from backup:
```bash
cat backup_fhir.sql | docker exec -i fhir-db psql -U admin -d hapi
```

---

## 📌 What's Inside?
### **`docker-compose.yml`**
```yaml
version: '3.7'

services:
  fhir:
    container_name: fhir
    image: "hapiproject/hapi:v7.6.0"
    ports:
      - "8080:8080"
    configs:
      - source: hapi
        target: /app/config/application.yaml
    depends_on:
      - db

  db:
    image: postgres:17.2
    restart: always
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    volumes:
      - ./hapi.postgress.data:/var/lib/postgresql/data

configs:
  hapi:
    file: ./hapi.application.yaml
```

---

## 📌 Where Is My Data Stored?
All PostgreSQL data is stored in **`./data/postgres_fhir/`**, ensuring it persists even when you restart or shut down Docker.

---

## 📌 Need Help?
- **FHIR API Docs**: [https://hapifhir.io/](https://hapifhir.io/)
- **PostgreSQL Docs**: [https://www.postgresql.org/](https://www.postgresql.org/)
- **Troubleshooting?** → Restart your containers:
  ```bash
  docker-compose down && docker-compose up -d
  ```

---

🚀 **Now your HAPI FHIR server is up and running with a persistent PostgreSQL database!** 🎉

