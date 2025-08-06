# TaskManager DevOps

This repository contains the full infrastructure and deployment logic for the [TaskManager API](https://github.com/tahaelidrissi/taskmanager-api).  
It centralizes all manifests and automation for deploying the application using Docker Swarm, Serverless Framework, and optionally Vercel.

---

## 📁 Repository Structure

```
.github/workflows/         → GitHub Actions CI/CD workflows (e.g. docker-publish.yml)
docker/
  ├── docker-compose.yml   → Local development/testing stack
  ├── stack.yml            → Docker Swarm stack for production deployment
  └── traefik/
      ├── traefik.yml         → Traefik static configuration (entrypoints, log levels, etc.)
      └── traefik-stack.yml   → Dynamic configuration for Traefik (routers/services with TLS)
serverless/
  ├── serverless.yml       → AWS Lambda deployment definition (via Serverless Framework)
  ├── package.json         → Node dependencies for serverless plugins
  └── package-lock.json
vercel/
  └── vercel.json          → Vercel build and route config for deploying as Python Serverless function
```

---

## 🚀 Deployment Options

### 🐳 Docker Swarm

Deploy the backend behind Traefik with HTTPS using Docker Swarm:
```bash
docker stack deploy -c docker/stack.yml taskmanager
```

Traefik will route incoming traffic to the API containers using labels defined in the stack file.

---


## Deployment on Vercel

1. Push your code to a public GitHub repository.  
2. Create a new project on Vercel and link it to your GitHub repo.  
3. Set the environment variable `MONGO_URI` in Vercel dashboard (Project Settings > Environment Variables).  
4. Set the build command to:

   ```
   uvicorn api.main:app --host 0.0.0.0 --port $PORT
   ```

5. Deploy and get your live API URL from Vercel dashboard.


---

## 🔁 GitHub Actions CI/CD

GitHub workflows under `.github/workflows/` automate:
- Building Docker images and pushing to registry (e.g. docker-publish.yml)
- Triggering deployments (can be extended to include SSH, AWS, etc.)

---

## 📦 Dependencies

**Node.js & NPM**: Required for `serverless/` deployment  
**Serverless Plugins Used**:
- `serverless-python-requirements`
- `serverless-dotenv-plugin`
- `serverless-offline`

Install them via:
```bash
npm install
```

---

## 🔗 Related Repository

- [📦 taskmanager-api](https://github.com/tahaelidrissi/taskmanager-api) – Contains the FastAPI app source code and Dockerfile.

---

