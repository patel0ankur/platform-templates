# Platform Templates

Backstage Scaffolder Templates for self-service AWS infrastructure provisioning via ACK (AWS Controllers for Kubernetes).

## How It Works

Developers use these templates in the Backstage portal to request AWS resources. Templates generate ACK Custom Resource YAML and open a Pull Request in `platform-configs`. ArgoCD detects the merge and ACK provisions the real AWS resource — no manual infrastructure work needed.

```
Developer fills form in Backstage
    → Template generates ACK CR YAML
    → PR opened in platform-configs repo
    → PR reviewed + merged
    → ArgoCD syncs to cluster
    → ACK creates AWS resource
    → Developer notified
```

## Available Templates

| Template | AWS Resource | Status |
|---|---|---|
| [ack-s3](./templates/ack-s3/) | S3 Bucket | ✅ Available |
| [ack-rds](./templates/ack-rds/) | RDS PostgreSQL Instance | ✅ Available |

## Adding to Backstage

Add the following to `app-config.yaml` in your Backstage deployment:

```yaml
catalog:
  locations:
    - type: url
      target: https://github.com/patel0ankur/platform-templates/blob/main/templates/ack-s3/template.yaml
      rules:
        - allow: [Template]
    - type: url
      target: https://github.com/patel0ankur/platform-templates/blob/main/templates/ack-rds/template.yaml
      rules:
        - allow: [Template]
```

New templates are picked up automatically on the next catalog refresh — no Backstage rebuild needed.

## Prerequisites

- EKS cluster with ACK EKS Capability enabled
- ArgoCD watching `platform-configs` repo (ApplicationSet configured)
- Backstage with GitHub integration (PAT or GitHub App)
