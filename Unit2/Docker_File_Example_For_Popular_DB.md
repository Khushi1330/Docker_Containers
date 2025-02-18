# Dockerfile example for popular DB

### 1. **MySQL**

#### Docker Image Pull:
```bash
docker pull mysql:latest
```

#### Running MySQL Container:
```bash
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql:latest
```

#### Accessing MySQL via Bash:
```bash
docker exec -it mysql-container bash
```

#### Access MySQL Shell:
```bash
mysql -u root -p
```
**Password**: `root`

#### Create Database and Table, Insert Data, and Query:
```sql
CREATE DATABASE testdb;
USE testdb;

CREATE TABLE users (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(100), age INT);
INSERT INTO users (name, age) VALUES ('Alice', 30), ('Bob', 25);

SELECT * FROM users;
```

---

### 2. **PostgreSQL**

#### Docker Image Pull:
```bash
docker pull postgres:latest
```

#### Running PostgreSQL Container:
```bash
docker run --name postgres-container -e POSTGRES_PASSWORD=password -p 5432:5432 -d postgres:latest
```

#### Accessing PostgreSQL via Bash:
```bash
docker exec -it postgres-container bash
```

#### Access PostgreSQL Shell:
```bash
psql -U postgres
```
**Password**: `password`

#### Create Database and Table, Insert Data, and Query:
```sql
CREATE DATABASE testdb;
\c testdb;

CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR(100), age INT);
INSERT INTO users (name, age) VALUES ('Alice', 30), ('Bob', 25);

SELECT * FROM users;
```

---

### 3. **MongoDB**

#### Docker Image Pull:
```bash
docker pull mongo:latest
```

#### Running MongoDB Container:
```bash
docker run --name mongo-container -p 27017:27017 -d mongo:latest
```

#### Accessing MongoDB via Bash:
```bash
docker exec -it mongo-container bash
```

#### Access MongoDB Shell:
```bash
mongo
```

#### Create Database and Collection, Insert Data, and Query:
```js
use testdb;

db.users.insertMany([
  { name: 'Alice', age: 30 },
  { name: 'Bob', age: 25 }
]);

db.users.find();
```

---

### 4. **Redis**

#### Docker Image Pull:
```bash
docker pull redis:latest
```

#### Running Redis Container:
```bash
docker run --name redis-container -p 6379:6379 -d redis:latest
```

#### Accessing Redis via Bash:
```bash
docker exec -it redis-container bash
```

#### Access Redis CLI:
```bash
redis-cli
```

#### Set and Get Data:
```bash
SET user:1 "Alice"
GET user:1
```

---

### 5. **MariaDB**

#### Docker Image Pull:
```bash
docker pull mariadb:latest
```

#### Running MariaDB Container:
```bash
docker run --name mariadb-container -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mariadb:latest
```

#### Accessing MariaDB via Bash:
```bash
docker exec -it mariadb-container bash
```

#### Access MariaDB Shell:
```bash
mysql -u root -p
```
**Password**: `root`

#### Create Database and Table, Insert Data, and Query:
```sql
CREATE DATABASE testdb;
USE testdb;

CREATE TABLE users (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(100), age INT);
INSERT INTO users (name, age) VALUES ('Alice', 30), ('Bob', 25);

SELECT * FROM users;
```

---

### 6. **SQLite**

#### Docker Image Pull:
```bash
docker pull nouchka/sqlite3:latest
```

#### Running SQLite Container:
```bash
docker run --name sqlite-container -v /path/to/data:/data -p 8080:8080 -d nouchka/sqlite3:latest
```

#### Accessing SQLite via Bash:
```bash
docker exec -it sqlite-container bash
```

#### Create Database and Table, Insert Data, and Query:
```bash
sqlite3 /data/test.db

CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, age INTEGER);
INSERT INTO users (name, age) VALUES ('Alice', 30), ('Bob', 25);

SELECT * FROM users;
```

---

### 7. **Cassandra**

#### Docker Image Pull:
```bash
docker pull cassandra:latest
```

#### Running Cassandra Container:
```bash
docker run --name cassandra-container -p 9042:9042 -d cassandra:latest
```

#### Accessing Cassandra via Bash:
```bash
docker exec -it cassandra-container bash
```

#### Access Cassandra CQL Shell:
```bash
cqlsh
```

#### Create Keyspace, Table, Insert Data, and Query:
```sql
CREATE KEYSPACE testdb WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
USE testdb;

CREATE TABLE users (id UUID PRIMARY KEY, name TEXT, age INT);
INSERT INTO users (id, name, age) VALUES (uuid(), 'Alice', 30);
INSERT INTO users (id, name, age) VALUES (uuid(), 'Bob', 25);

SELECT * FROM users;
```

---

### 8. **Elasticsearch**

#### Docker Image Pull:
```bash
docker pull docker.elastic.co/elasticsearch/elasticsearch:latest
```

#### Running Elasticsearch Container:
```bash
docker run --name es-container -p 9200:9200 -p 9300:9300 -d docker.elastic.co/elasticsearch/elasticsearch:latest
```

#### Accessing Elasticsearch via Bash:
```bash
docker exec -it es-container bash
```

#### Perform Elasticsearch Query:
```bash
curl -X GET "localhost:9200/_cat/indices?v"
```

#### Insert and Query Data (Using curl):
```bash
curl -X POST "localhost:9200/testdb/_doc/1" -H 'Content-Type: application/json' -d '{"name": "Alice", "age": 30}'
curl -X POST "localhost:9200/testdb/_doc/2" -H 'Content-Type: application/json' -d '{"name": "Bob", "age": 25}'
curl -X GET "localhost:9200/testdb/_search?q=age:30"
```

---

### 9. **Neo4j**

#### Docker Image Pull:
```bash
docker pull neo4j:latest
```

#### Running Neo4j Container:
```bash
docker run --name neo4j-container -e NEO4J_AUTH=neo4j/password -p 7474:7474 -p 7687:7687 -d neo4j:latest
```

#### Accessing Neo4j via Bash:
```bash
docker exec -it neo4j-container bash
```

#### Access Neo4j Shell:
```bash
cypher-shell -u neo4j -p password
```

#### Create Nodes, Relationships, and Query:
```cypher
CREATE (a:Person {name: 'Alice', age: 30});
CREATE (b:Person {name: 'Bob', age: 25});
CREATE (a)-[:FRIEND]->(b);

MATCH (n:Person) RETURN n;
```

---

### 10. **InfluxDB**

#### Docker Image Pull:
```bash
docker pull influxdb:latest
```

#### Running InfluxDB Container:
```bash
docker run --name influxdb-container -p 8086:8086 -d influxdb:latest
```

#### Accessing InfluxDB via Bash:
```bash
docker exec -it influxdb-container bash
```

#### Access InfluxDB CLI:
```bash
influx
```

#### Create Database, Write Data, and Query:
```bash
CREATE DATABASE testdb;
USE testdb;

INSERT INTO cpu_usage (value) VALUES (0.64);
SELECT * FROM cpu_usage;
```

---

### Summary

These are Docker commands and examples for interacting with **10 popular databases**. You can pull their Docker images, run containers, and access their respective shells via bash or CLI to perform basic operations like creating databases, adding data, and querying data.

- **MySQL, PostgreSQL, MariaDB** (SQL-based relational databases)
- **MongoDB, Cassandra, Elasticsearch, Neo4j** (NoSQL databases)
- **Redis, SQLite, InfluxDB** (Key-value, embedded, and time-series databases)

You can customize these commands depending on your project's needs, such as different user credentials or more complex queries.