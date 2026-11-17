# Storage

## Azure Storage Account

Terrakube requires an Azure Storage account to save the state/output for the jobs and to save the terraform modules when using terraform CLI and it require the following containers:
- registry (blob)
- tfstate (private)
- tfoutput (private)

To create the Azure storage account you can use the following [terraform module](https://github.com/terrakube-io/terraform-azurerm-terrakube-cloud-storage).

## AWS S3

Terrakube requires an AWS S3 to save the state/output for the jobs and to save the terraform modules when using terraform CLI, it could be a private bucket.

Creating the bucket with a Terraform module is a work in progress.

To connect with an IAM role (EKS Pod Identity or IRSA) instead of access keys, leave `accessKey` and `secretKey` empty and set `enableRoleAuth`. The api, registry and executor then use the AWS SDK default credentials, so give their service accounts (`api.serviceAccountName`, `registry.serviceAccountName`, `executor.serviceAccountName`) access to the bucket.

```yaml
storage:
  defaultStorage: false
  aws:
    bucketName: "my-bucket"
    region: "us-east-1"
    enableRoleAuth: true
```

## GCP Storage

Terrakube requires a storage bucket to save the state/output for the jobs and to save the terraform modules when using terraform CLI.

Creating the bucket with a Terraform module is a work in progress.

## MinIO

Terrakube requires a storage bucket to save the state/output for the jobs and to save the terraform modules when using terraform CLI.

Creating the bucket with a Terraform module is a work in progress.
