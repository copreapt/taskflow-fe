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
