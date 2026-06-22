# PipelineIQ Pipeline Runner Service

Independent repository for the PipelineIQ pipeline runner worker.

## Build

```bash
docker build -t <acr-login-server>/final_capstone-pipeline-runner-service:local -f services/pipeline-runner-service/Dockerfile .
```

## Local Run 

This service expects PipelineIQ environment variables from Kubernetes ConfigMap and Key Vault secrets.

```bash
cd services/pipeline-runner-service
npm install
DATABASE_URL=<postgres-url> RABBITMQ_URL=<rabbitmq-url> npm start
```

## CI/CD Pipeline

Pipeline file: `.github/workflows/service-ci.yml`

Run it from GitHub Actions with `Run workflow` on the `dev` branch, or push to `dev`:

```bash
git checkout dev
git add .
git commit -m "change pipeline runner service"
git push origin dev
```

The pipeline calls the reusable workflow in `PipelineIQproject/pipeline_main` and runs SonarQube Cloud, Snyk, Docker build, Trivy, container smoke test, ACR push, Helm dev update, production approval, Helm prod update, and Slack notification.

## Required Secrets

| Secret | Purpose |
| --- | --- |
| `ACR_LOGIN_SERVER` | Azure Container Registry server. |
| `ACR_USERNAME` | Identity allowed to push to ACR. |
| `ACR_PASSWORD` | Password/secret for the ACR identity. |
| `SONAR_TOKEN` | SonarQube Cloud token. |
| `SNYK_TOKEN` | Snyk API token. |
| `MAIN_REPO_PAT` | PAT with write access to `PipelineIQproject/pipeline_main`. |
| `SLACK_WEBHOOK_URL` | Slack incoming webhook for success/failure notifications. |
| `SMOKE_TEST_ENV_FILE` | Optional dotenv content; worker smoke currently verifies the image can run Node. |

Create a protected GitHub Environment named `production` so the prod Helm value update requires approval.
