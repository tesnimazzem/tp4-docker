# TP4 : Docker et Docker Compose

## Objectif

Conteneuriser une application React (client) + Express (serveur) avec MongoDB,
puis l'orchestrer avec Docker Compose.

## Structure du projet

- `Client/` : application React (Vite) + Dockerfile
- `Server/` : API Express + Dockerfile
- `docker-compose.yml` : orchestration des 3 services

## Étapes réalisées

### 1. Dockerfiles

- `Server/Dockerfile` : image `node:lts-alpine`, installation des dépendances,
  port 9000, démarrage avec `npm start`.
- `Client/Dockerfile` : image `node:lts-alpine`, build Vite (`npm run build`),
  serveur statique `serve` sur le port 3000.

### 2. Construction des images

```bash
docker build -t mern-server ./Server
docker build -t mern-client ./Client
```

### 3. Réseau et conteneurs

```bash
docker network create mern-network
docker run -d --name mongodb --network mern-network mongo
docker run -d --name server --network mern-network -p 9000:9000 -e MONGO_URI=mongodb://mongodb:27017/mydb mern-server
docker run -d --name client --network mern-network -p 3000:3000 mern-client
```

### 4. Docker Compose

```bash
docker compose up --build
```

Services : `mongodb`, `server` (dépend de mongodb, variable `MONGO_URI`),
`client` (dépend de server), réseau `mern-network` (driver bridge).

## Tests

- Client : http://localhost:3000
- API : http://localhost:9000/api/health

## Captures d'écran

(à ajouter : `docker ps`, `docker compose ps`, page client, page API)
