# Taskflow

Nest.js front end for the Taskflow task management app.

## Tech Stack

- Next.js 16
- Typescript
- Tailwind CSS

## Prerequisites

- Node 20
- npm

## Local Development

Start the dev server:
npm run dev

App runs on http://localhost:3000

## Environment Variables

Copy `.env.example` to `.env.local` and fill in the values.

| Variable            | Description               |
| ------------------- | ------------------------- |
| NEXT_PUBLIC_API_URL | Backend API URL           |
| NODE_ENV            | Environment (development) |

## CI/CD

This project uses Github Actions for continuous integration.

The pipeline runs automatically on every push and pull request to `main` and `staging` branches.

### Pipeline steps

1. Install dependencies
2. Run linter
3. Run type check
4. Build

## Docker

### Local Development

Run directly without Docker:
npm run dev

### Building Production Image

docker build -t taskflow-frontend .

### Docker Compose Files

| File                            | Purpose                        |
| ------------------------------- | ------------------------------ |
| `docker-compose.yml`            | Available but not used locally |
| `docker-compose.staging.yml`    | Staging deployment on EC2      |
| `docker-compose.production.yml` | Production deployment on EC2   |

### Image Sizes

| Image               | Content Size |
| ------------------- | ------------ |
| `taskflow-frontend` | 91.2MB       |

## Deployment

### Staging

- **Server**: AWS EC2 t3.micro (eu-north-1)
- **Docker image**: copreapt/taskflow-frontend:staging

#### Manual deployment steps

1. Build and push image to Docker Hub:
   docker build -t copreapt/taskflow-frontend:staging .
   docker push copreapt/taskflow-frontend:staging

2. SSH into EC2:
   ssh -i <staging-key>.pem ubuntu@<staging-ip>

3. Pull and run:
   docker pull copreapt/taskflow-frontend:staging
   docker-compose -f docker-compose.frontend.yml up -d

## CI/CD Pipeline

This project uses GitHub Actions for continuous integration and deployment.

### How it works

| Branch       | CI                 | Deployment                                |
| ------------ | ------------------ | ----------------------------------------- |
| `main`       | Runs on every push | Deploys to production (requires approval) |
| `staging`    | Runs on every push | Deploys to staging (automatic)            |
| Pull Request | Runs on every PR   | No deployment                             |

### Pipeline steps

1. Lint
2. Type check
3. Build
4. Deploy (if branch is `staging` or `main`)

### Environments

| Environment | Frontend                    | Backend                     |
| ----------- | --------------------------- | --------------------------- |
| Staging     | http://<staging-ip>:3000    | http://<staging-ip>:3001    |
| Production  | http://<production-ip>:3000 | http://<production-ip>:3001 |

### Required GitHub Secrets

| Secret                     | Description                    |
| -------------------------- | ------------------------------ |
| `DOCKER_USERNAME`          | Docker Hub username            |
| `DOCKER_PASSWORD`          | Docker Hub access token        |
| `EC2_HOST`                 | Staging EC2 IP                 |
| `EC2_USER`                 | Staging EC2 username           |
| `EC2_SSH_KEY`              | Staging EC2 SSH private key    |
| `EC2_PROD_HOST`            | Production EC2 IP              |
| `EC2_PROD_USER`            | Production EC2 username        |
| `EC2_PROD_SSH_KEY`         | Production EC2 SSH private key |
| `NEXT_PUBLIC_API_URL`      | Staging backend API URL        |
| `NEXT_PUBLIC_API_URL_PROD` | Production backend API URL     |
