# Overview

This Docker image (see [Dockerfile](https://github.com/Azure/terraform-test/blob/master/Dockerfile)) is for testing [Azure Terraform modules](https://registry.terraform.io/browse?provider=azurerm).

[![Build Status](https://travis-ci.org/Azure/terraform-test.svg?branch=master)](https://travis-ci.org/Azure/terraform-test)

# Usage 

This image can be used for terraform lint or end to end tests against Azure.

## Lint Tests

These tests ensure consistency in formatting for the terraform module code.

Setup the environment variable which specifies the root path of the module code on the local machine.

```shell
export MODULE_PATH=/user/me/source/Azure/terraform-azurerm-modulename
```

Now run the lint tests:

```shell
docker run -v $MODULE_PATH:/go/src/module --rm microsoft/terraform-test azure-terraform-module-test.sh validate
```

## End to End Tests

These tests will execute a `terraform apply` to deploy the azure module as defined in the `/test/integration` directory and then `terraform destroy` to delete them.

### Pre-requisites

Azure Setup
The container uses Azure SPN Keys to access Azure. The following environment variables need to be set on the local machine running the container image.

```
 ARM_SUBSCRIPTION_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
 ARM_CLIENT_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
 ARM_CLIENT_SECRET=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
 ARM_TENANT_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
 ARM_TEST_LOCATION=westus
 ARM_TEST_LOCATION_ALT=westus
```

Secret Setup
Some test may require SSH keys or other secrets to be passed to the container. For SSH keys, mount the directory where they are located as `/root/.ssh` in the container. If you are reusing your keys, the above command should be:

### Running the Tests

Setup the environment variable which specifies the root path of the module code on the local machine.

```shell
export MODULE_PATH=/user/me/source/Azure/terraform-azurerm-modulename
```

Now run the tests using this docker command:

```shell
docker run -v $MODULE_PATH:/go/src/module -e ARM_CLIENT_ID -e ARM_TENANT_ID -e ARM_SUBSCRIPTION_ID -e ARM_CLIENT_SECRET -e ARM_TEST_LOCATION -e ARM_TEST_LOCATION_ALT --rm microsoft/terraform-test azure-terraform-module-test.sh full
```

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
