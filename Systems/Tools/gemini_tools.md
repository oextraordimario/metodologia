# Gemini Tools

This file describes the tools available for interacting with Gemini and other services.

## CloudFormation Tools

- **get_resource_schema_information**: Retrieves the resource schema for a given CloudFormation resource type.
- **list_resources**: Lists resources of a specific CloudFormation resource type.
- **get_resource**: Retrieves the properties of a specific CloudFormation resource.
- **update_resource**: Updates a CloudFormation resource with a given patch document.
- **create_resource**: Creates a new CloudFormation resource.
- **delete_resource**: Deletes a CloudFormation resource.
- **get_resource_request_status**: Retrieves the status of a CloudFormation resource request.
- **create_template**: Initiates the generation of a CloudFormation template from existing resources.

## Terraform Tools

- **ExecuteTerraformCommand**: Executes Terraform workflow commands like `init`, `plan`, `apply`.
- **SearchAwsProviderDocs**: Searches the official Terraform AWS provider documentation.
- **SearchAwsccProviderDocs**: Searches the Terraform AWS Cloud Control (AWSCC) provider documentation.
- **SearchSpecificAwsIaModules**: Searches for specific AWS-IA (Infrastructure and Automation) Terraform modules.
- **RunCheckovScan**: Runs a Checkov security scan on Terraform files to find misconfigurations.
- **SearchUserProvidedModule**: Searches for a user-provided Terraform module in the registry.
- **ExecuteTerragruntCommand**: Executes Terragrunt workflow commands.

## SQS Tools

- **create_queue**: Creates a new SQS queue, automatically adding required tags and handling FIFO queue attributes.
- **send_message**: Sends a message to an SQS queue.
- **receive_message**: Receives messages from an SQS queue.
- **send_message_batch**: Sends a batch of messages to an SQS queue.
- **delete_message**: Deletes a message from an SQS queue.

## Forecasting Tools

- **forecastIowaLiquorSalesTool**: A tool for time-series forecasting, specifically for Iowa liquor sales.

## Gemini Model Interaction

- **call**: Calls the Gemini model with a given prompt.
- **call_parallel**: Calls the Gemini model for multiple prompts in parallel.
- **_to_gemini_schema**: Converts an OpenAPI schema to a Gemini-compatible schema.
- **setup_gemini_config**: Creates a custom evaluation configuration for Gemini models.
