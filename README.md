# PipelineIQ Pipeline Runner Service

Independent repository staging folder for the PipelineIQ pipeline runner service.

## Build

```bash
docker build -t nimeshsv814/pipelineiq-pipeline-runner-service:v1.0.0 -f services/pipeline-runner-service/Dockerfile .
```

## Run

This service expects PipelineIQ environment variables from Kubernetes ConfigMap and Key Vault secrets.
