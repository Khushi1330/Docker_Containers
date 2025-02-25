# Debugging Docker Issues

## 1. FastAPI container starts, but `http://localhost:8000` is unreachable

### Dockerfile (Incorrect)
```dockerfile
FROM python:3.9
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Docker Run Command (Incorrect)
```sh
docker build -t fastapi-app .
docker run fastapi-app
```

### Possible Causes
1. The `docker run` command does not expose port 8000.
2. The app is running inside the container but is not mapped to the host machine.

### Solution
Use the `-p` flag to expose the port:
```sh
docker run -p 8000:8000 fastapi-app
```

---

## 2. FastAPI app crashes with `ModuleNotFoundError: No module named 'fastapi'`

### Dockerfile (Incorrect)
```dockerfile
FROM python:3.9
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### requirements.txt (Incorrect)
```txt
uvicorn
```

### Possible Causes
1. `fastapi` is missing from `requirements.txt`.
2. Dependencies were not installed properly.

### Solution
Add `fastapi` to `requirements.txt`:
```txt
fastapi
uvicorn
```
Rebuild the image:
```sh
docker build --no-cache -t fastapi-app .
docker run -p 8000:8000 fastapi-app
```

---

## 3. Flask app inside Docker runs but CSS/JS static files are missing

### Possible Causes
1. Static files are not copied into the container.
2. Flask is not serving static files in production.

### Solution
Modify your **Dockerfile** to collect static files:
```dockerfile
FROM python:3.9
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
RUN flask collect
CMD ["flask", "run", "--host=0.0.0.0", "--port=5000"]
```

---

## 4. Django app throws `OperationalError: could not connect to server` when using PostgreSQL in Docker

### Docker Compose (Incorrect)
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
```

### Possible Causes
1. The database is not ready when Django starts.
2. Django is not waiting for PostgreSQL before running migrations.

### Solution
Use **`wait-for-it.sh`** or `entrypoint.sh` to delay Django startup:

#### entrypoint.sh (Add this file to your project)
```sh
#!/bin/sh
echo "Waiting for PostgreSQL to start..."
while ! nc -z db 5432; do   
  sleep 1
done
echo "PostgreSQL started"
exec "$@"
```
Modify `Dockerfile`:
```dockerfile
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
```
Update `docker-compose.yml`:
```yaml
command: ["python", "manage.py", "migrate"]
```

---

## 5. SQLite database file is missing inside the Flask container after restart

### Possible Causes
1. The SQLite file is stored inside the container and lost on restart.

### Solution
Use **Docker volumes** to persist the SQLite file:
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - ./data:/app/data
```
Modify **Flask settings** to store the database in `/app/data`.

---

## 6. Django migration fails with `django.db.utils.IntegrityError: NOT NULL constraint failed`

### Possible Causes
1. A field was added with `null=False` but without a default value.

### Solution
Modify the migration to set a default value:
```python
class Migration(migrations.Migration):
    operations = [
        migrations.AddField(
            model_name='user',
            name='age',
            field=models.IntegerField(default=18),
        ),
    ]
```
Then run:
```sh
docker-compose run web python manage.py migrate
```

---
## 7. FastAPI app inside Docker cannot access PostgreSQL database

### Docker Compose (Incorrect)
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
```

### Possible Causes
1. The FastAPI app is using `localhost` instead of `db` as the database hostname.
2. The database service may not be ready when FastAPI starts.

### Solution
Modify `database.py` in FastAPI:
```python
SQLALCHEMY_DATABASE_URL = "postgresql://user:password@db/mydb"
engine = create_engine(SQLALCHEMY_DATABASE_URL)
```
Ensure `depends_on` in `docker-compose.yml` waits for the database:
```yaml
depends_on:
  db:
    condition: service_healthy
```

---

## 8. Streamlit app inside Docker does not update when files are modified

### Docker Run Command (Incorrect)
```sh
docker run -p 8501:8501 streamlit-app
```

### Possible Causes
1. The container is running without mounting the local directory.
2. Streamlit does not detect file changes inside the container.

### Solution
Use Docker volumes to enable hot-reloading:
```sh
docker run -p 8501:8501 -v $(pwd):/app streamlit-app
```
Modify `Dockerfile` to install Streamlit properly:
```dockerfile
CMD ["streamlit", "run", "app.py", "--server.port", "8501", "--server.enableCORS", "false"]
```

---

## 9. Flask app running inside Docker loses uploaded files after restart

### Possible Causes
1. Files are stored inside the container and lost when the container stops.

### Solution
Use Docker volumes to persist files:
```yaml
services:
  web:
    build: .
    volumes:
      - uploads:/app/uploads
volumes:
  uploads:
```
Modify Flask app to store files in `/app/uploads`:
```python
UPLOAD_FOLDER = "uploads"
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
```

---

## 10. FastAPI app inside Docker cannot access Redis cache

### Docker Compose (Incorrect)
```yaml
version: '3'
services:
  web:
    build: .
    depends_on:
      - redis
  redis:
    image: redis:latest
```

### Possible Causes
1. FastAPI is trying to connect to `localhost` instead of `redis`.

### Solution
Modify FastAPI to connect to Redis:
```python
import redis
r = redis.Redis(host='redis', port=6379)
```
Ensure Redis service is defined in `docker-compose.yml`:
```yaml
services:
  redis:
    image: redis:latest
    ports:
      - "6379:6379"
```

---

## 11. Streamlit app inside Docker cannot connect to PostgreSQL database

### Possible Causes
1. The Streamlit app is using `localhost` instead of the correct database hostname.
2. The database container is not accessible from the Streamlit container.

### Solution
Modify `docker-compose.yml`:
```yaml
services:
  web:
    build: .
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
```
Modify the Streamlit app’s connection string:
```python
conn = psycopg2.connect(
    host='db',
    database='mydb',
    user='user',
    password='password'
)
```

---

## 12. Flask app inside Docker cannot access MySQL database

### Possible Causes
1. The Flask app is using `localhost` instead of `db`.
2. The MySQL container is not ready when Flask starts.

### Solution
Modify Flask’s `config.py`:
```python
SQLALCHEMY_DATABASE_URI = "mysql://user:password@db/mydb"
```
Ensure MySQL is defined in `docker-compose.yml`:
```yaml
services:
  db:
    image: mysql:5.7
    environment:
      MYSQL_USER: user
      MYSQL_PASSWORD: password
      MYSQL_DATABASE: mydb
```

---

## 13. FastAPI app inside Docker cannot connect to SQLite database

### Possible Causes
1. SQLite stores its database file inside the container, which is lost on restart.

### Solution
Use Docker volumes to persist the SQLite database:
```yaml
services:
  web:
    build: .
    volumes:
      - sqlite_data:/app/data
volumes:
  sqlite_data:
```
Modify FastAPI database settings:
```python
DATABASE_URL = "sqlite:///data/database.db"
```

---

## 14. Flask app inside Docker does not restart when the database restarts

### Possible Causes
1. Flask does not handle database connection failures gracefully.

### Solution
Modify `app.py` to retry the database connection:
```python
import time
import psycopg2
while True:
    try:
        conn = psycopg2.connect("dbname=mydb user=user host=db password=password")
        break
    except:
        time.sleep(5)
```

Ensure Flask depends on the database in `docker-compose.yml`:
```yaml
web:
  depends_on:
    db:
      condition: service_healthy
```

---

## 15. Streamlit app in Docker fails with `ModuleNotFoundError: No module named 'streamlit'`

### Possible Causes
1. `streamlit` is missing in `requirements.txt`.

### Solution
Add `streamlit` to `requirements.txt`:
```txt
streamlit
```
Rebuild the Docker image:
```sh
docker build --no-cache -t streamlit-app .
```
Run the container:
```sh
docker run -p 8501:8501 streamlit-app
```

---

## 17. Flask application fails to connect to MongoDB running inside Docker

### Possible Causes
1. Flask is using `localhost` instead of the correct service name.
2. The MongoDB container is not exposed properly.

### Solution
Modify Flask’s connection to MongoDB:
```python
from pymongo import MongoClient
client = MongoClient("mongodb://db:27017/")
db = client.mydatabase
```
Ensure MongoDB is correctly configured in `docker-compose.yml`:
```yaml
services:
  db:
    image: mongo:latest
    ports:
      - "27017:27017"
```

---

## 18. FastAPI application fails to access external API inside Docker network

### Possible Causes
1. The container lacks internet access.
2. The `network_mode` is improperly set.

### Solution
Ensure the container uses the default network:
```yaml
services:
  web:
    build: .
    network_mode: "bridge"
```
Check DNS settings inside the container:
```sh
docker exec -it <container_id> cat /etc/resolv.conf
```

---

## 19. Streamlit app inside Docker fails to load environmental variables

### Possible Causes
1. Environment variables are missing in the Docker container.

### Solution
Use `.env` file and load it in `docker-compose.yml`:
```yaml
env_file:
  - .env
```
Ensure `.env` contains the necessary values:
```txt
API_KEY=my_secret_key
```
Load environment variables in Streamlit:
```python
import os
api_key = os.getenv("API_KEY")
```

---

## 20. Flask app inside Docker does not restart after crash

### Possible Causes
1. The container exits and does not restart automatically.

### Solution
Ensure `restart` is set in `docker-compose.yml`:
```yaml
restart: always
```
Monitor logs for errors:
```sh
docker logs -f <container_id>
```

---

## 21. FastAPI app cannot access RabbitMQ queue inside Docker

### Possible Causes
1. The FastAPI app is using the wrong hostname.
2. RabbitMQ is not ready when FastAPI starts.

### Solution
Modify FastAPI connection to RabbitMQ:
```python
import pika
params = pika.ConnectionParameters(host='rabbitmq')
connection = pika.BlockingConnection(params)
```
Ensure RabbitMQ is correctly configured:
```yaml
services:
  rabbitmq:
    image: rabbitmq:latest
    ports:
      - "5672:5672"
```

---

## 22. Streamlit app inside Docker fails with `OSError: [Errno 98] Address already in use`

### Possible Causes
1. The port `8501` is already in use on the host machine.

### Solution
Run the container on a different port:
```sh
docker run -p 8502:8501 streamlit-app
```
Check running containers:
```sh
docker ps
```

---

## 23. Flask app inside Docker fails with `sqlite3.OperationalError: unable to open database file`

### Possible Causes
1. SQLite database file is missing or improperly mounted.

### Solution
Ensure the database file is in a persistent volume:
```yaml
volumes:
  - ./db:/app/db
```
Modify Flask app’s database path:
```python
SQLALCHEMY_DATABASE_URI = "sqlite:////app/db/database.db"
```

---

## 24. FastAPI application fails to detect changes in code inside Docker

### Possible Causes
1. The FastAPI app does not reload inside the container.

### Solution
Modify `Dockerfile` to enable auto-reloading:
```dockerfile
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
```
Ensure live reload works by mounting the project directory:
```sh
docker run -v $(pwd):/app fastapi-app
```

---

## 25. Flask app inside Docker cannot send emails using SMTP server

### Possible Causes
1. SMTP settings are not correctly configured in the Flask app.

### Solution
Ensure Flask has correct SMTP settings:
```python
MAIL_SERVER = "smtp.mailtrap.io"
MAIL_PORT = 587
MAIL_USERNAME = os.getenv("MAIL_USERNAME")
MAIL_PASSWORD = os.getenv("MAIL_PASSWORD")
```
Add these values in `.env` and use them in `docker-compose.yml`:
```yaml
env_file:
  - .env
```

---

**# Debugging Questions with Answers**

## **Q26: FastAPI or Streamlit Service Unreachable in Docker**

### **Scenario:**
You have deployed a FastAPI or Streamlit-based web service using Docker, but when you try to access it via `http://localhost:5000`, the service is unreachable. Upon inspecting the Dockerfile, you notice the following configuration:

```dockerfile
FROM python:3.9  
WORKDIR /app  
COPY . .  
RUN pip install -r requirements.txt  
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5000"]
```

### **Possible Causes:**
1. The container starts successfully, but no response is received.  
2. `docker ps` shows the container running.  

### **Tasks & Answers:**
1. **Identify the missing configuration:** The issue is likely that the port is not properly mapped between the container and the host.
2. **Explain why the issue occurs:** The service is listening inside the container on port `5000`, but it has not been exposed to the host machine.
3. **Provide a corrected Dockerfile or `docker run` command to fix the issue:**
   - Use the following `docker run` command to map the port correctly:
     ```sh
     docker run -d -p 5000:5000 myimage
     ```
   - Alternatively, modify the Dockerfile to include:
     ```dockerfile
     EXPOSE 5000
     ```

---

## **Q27: FastAPI Service Not Accessible on Exposed Port**

### **Scenario:**
You have created and started a container using the following command:

```sh
docker run -d --name myfastapi -p 8000:5000 myfastapiimage
```

However, when you try to access `http://localhost:8000`, it is unreachable.

### **Possible Causes:**
- The container is running, but the service is not accessible.  
- No errors appear in the logs.

### **Tasks & Answers:**
1. **Identify the mistake in the `docker run` command:** The port mapping is incorrect. The container's internal port `5000` should be mapped to `5000` on the host, not `8000`.
2. **Explain why this issue occurs:** The FastAPI application inside the container is running on port `5000`, but the host is trying to access it on `8000`, leading to a connection failure.
3. **Provide a corrected command to properly expose the FastAPI service:**
   ```sh
   docker run -d --name myfastapi -p 5000:5000 myfastapiimage
   ```

---

## **Q28: Machine Learning-Based Web Service Crashes Due to Python Version Issues**

### **Scenario:**
You are deploying a machine learning-based web service using Docker. Your Dockerfile uses Python 3.8, but the service crashes due to missing dependencies. The error logs indicate an issue with package compatibility. You suspect that the pre-installed Python version inside the container is incompatible with the model’s dependencies.

### **Tasks & Answers:**
1. **Identify the root cause of the issue:** The Python version inside the container does not match the version required by the model's dependencies.
2. **Suggest a method to check and match the correct Python version:**
   - Run the following command inside the container to verify the installed Python version:
     ```sh
     docker run --rm mymlimage python --version
     ```
   - Ensure that the correct Python version is installed and used in the Dockerfile.
3. **Provide a corrected Dockerfile that ensures compatibility with the required Python version:**
   ```dockerfile
   FROM python:3.9  # Ensure compatibility with the required version
   WORKDIR /app
   COPY . .
   RUN pip install --upgrade pip && pip install -r requirements.txt
   CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5000"]
   ```

---