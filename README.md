# Function Template

## Overview

Repository for the template used to create serverless functions which can be deployed to cloud providers and converted to Docker images to be used by the [Apollo orchestration platform](https://github.com/Apollo-Core).

## Get the code

```sh
git clone https://github.com/Apollo-Workflows/Apollo-Template
cd Apollo-Template
```

## Documentation

- [Add your own function](./doc/addFunction.md)
- [Build OpenFaaS Docker images](./doc/openfaas.md)
- Python layers:
  - [Create a custom AWS Python layer](./doc/awsLayer.md)
  - [Create a custom IBM Python layer](./doc/ibmLayer.md)
- Add a function to auto deployment:
  - [For AWS](./doc/awsAddDeployment.md)
  - [For IBM](./doc/ibmAddDeployment.md)

## Autodeploy

1. Save the credentials for your cloud provider in the according subfolder:
   - AWS: Go to the service IAM and create a new user with the access type `Programmatic access` and attach the `AdministratorAccess` policy, which is part of the existing policies. You can copy the `Access key ID` and `Secret access key` and put it into the following format and save it under `aws/credentials`:

     ```config
     [default]
     aws_access_key_id = <Your access key ID>
     aws_secret_access_key = <Your secret access key>
     ```

   - IBM:
   - Create the `ibmcloud_api_key` [here](https://cloud.ibm.com/iam/apikeys) add it to `ibm/terraform.tfvars`
   - Add S3 credentials from AWS to `ibm/s3Credentials.json`
2.
   - A: Deploy to all providers:
    Run from root dir `docker run --rm -it --entrypoint=/app/deployAll.sh -v ${PWD}:/app/ chrisengelhardt/apollo-autodeploy`
   - B: Deploy single provider with custom settings:
    Run `docker run --rm -v ${PWD}:/app/ chrisengelhardt/apollo-autodeploy --help` from within the directory of your chosen cloud provider

Note: For IBM you have to create a namespace first and place it into `ibm.tf` on all lines with `namespace = "YOURNAMESPACE"`.

```text
Usage: /app/deploy.sh [--help] [--region region] [--url] [--mapping]

Commands:
    --help                  Show this help output.
    --region region         Sets a specific region for the deployment. Use a region from:
                https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html
    --url                   Prints out all deployment urls
    --mappings              Creates typeMapping.json with the deployment urls
```
