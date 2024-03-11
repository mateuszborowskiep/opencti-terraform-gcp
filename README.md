# opencti-terraform
This repository is here to provide you with a quick and easy way to deploy an OpenCTI instance in the cloud (AWS, Azure, or GCP).

If you run into any issues, please open an issue.

## Before you deploy
You will need to first change into the `aws/` or `azure/` or `gcp/` directory _before_ you run `terraform init`. The following sections will bring you through the entire process and outline the various settings you will need to set before you can deploy.


### GCP
Change into the `gcp/` directory:
```
cd gcp/
```

You will need to create a new project in GCP and set up billing. Note the project ID because you will need it in a minute. Then, set up a service account with the following roles and download the service account key:
- Security Admin
- Owner
- Storage Admin

The following items can be set in `terraform.tfvars`:
- `credentials`: The path to your service account key file. Please make sure it has the permissions listed above. No default.
- `disk_size`: The disk size (in GB) for the instance. Default `32`. [OpenCTI minimum specs](https://github.com/OpenCTI-Platform/opencti/blob/5ede2579ee3c09c248d2111b483560f07d2f2c18/opencti-documentation/docs/getting-started/requirements.md) is 32GB drive.
- `machine_type`: The GCE machine type to use. Default `e2-standard-8`. [OpenCTI minimum specs](https://github.com/OpenCTI-Platform/opencti/blob/5ede2579ee3c09c248d2111b483560f07d2f2c18/opencti-documentation/docs/getting-started/requirements.md) is 8x16. The default size is 8x32.
- `project_id`: The Google Cloud project ID. No default.
- `region`: The Google Cloud region to run the instance in. Default `us-east1`.
- `zone`: The Google Cloud zone to run the instance in. Default `us-east1-b`.

## Deployment
To see what Terraform is going to do and make sure you're cool with it, create a plan (`terraform plan`) and check it over. Once you're good to go, apply it (`terraform apply`).

### GCP
The apply will probably fail because the APIs (Compute Engine, IAM, etc.) are being activated. If it errors out because of the APIs, wait a few minutes and re-run `terraform apply`.

## Post-deployment
Once the installation is complete, you'll want to grab the admin password that was generated. The username is the e-mail you provided in `terraform.tfvars`. Get the password by running the following on the VM:
```
cat /opt/opencti/config/production.json | jq '.app.admin.password'
```

Next, go to port 4000 of the public IP of the machine and login with the credentials you just grabbed.
