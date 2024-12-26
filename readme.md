# Gitlab CI Examples

Gitlab CI Examples

## Collection of some of my examples

- example-gitlab-ci-openID.yml -> Regular spec input with OpenID
- example-gitlab-ci-change-dir -> Change Directory with OpenID
- example-gitlab-ci-branch-trigger -> Also trigger from branch
- example-gitlab-ci-dir-structure -> A main CI file that can swap into seperate deploy folders


## Example Gitlab CI with TFLint

Overview of the GitLab CI/CD Configuration File

This GitLab CI/CD file is tailored for Terraform workflows in a multi-environment setup, integrating various tools like Checkov, TFLint, and AWS CLI. Here’s a breakdown of its key components:

1. Default Settings
	•	Image: A preconfigured Docker image with Terraform 1.5.7.
	•	Cache: Caches Terraform files per environment (TF_STACK and TF_INSTANCE) to improve initialization time.
	•	Tags: Indicates the type of GitLab-hosted runners required.
	•	OIDC for AWS: Configures GitLab OpenID Connect tokens to authenticate with AWS.

2. Includes
	•	Integrates a shared template (assume-role.yml) for AWS Assume Role with Web Identity.

3. Command Templates

Templates define reusable commands for common tasks:
	•	.aws-cli: Installs AWS CLI.
	•	.checkov-cli: Installs the Checkov CLI.

4. Job Definitions

Terraform Initialization (.tf_init)
	•	Initializes Terraform with the specified backend state.
	•	Always runs using gitlab-terraform init.

Terraform Validation (.tf_validate)
	•	Formats and validates Terraform configurations.
	•	Ensures code style and correctness.

TFLint for Instances (.tf_lint_instances)
	•	Lints Terraform configurations specific to instances.
	•	Generates JUnit reports for CI/CD insights.
	•	Rules allow failures without blocking the pipeline.

TFLint for Modules (.tf_lint_modules)
	•	Similar to instances, but checks reusable Terraform modules.

Checkov Security Checks (.tf_checkov)
	•	Runs Checkov to scan Terraform for security and compliance issues.
	•	Outputs JUnit reports and sets hard failure criteria for high-severity findings.

Terraform Plan (.tf_plan)
	•	Generates a Terraform execution plan.
	•	Artifacts include:
	•	JSON representation of the plan.
	•	Plan cache for subsequent jobs.
	•	The plan is reported on merge requests.

Terraform Apply (.tf_apply)
	•	Applies the planned Terraform changes.
	•	Limited to specific branches/environments (dev and default branch).
	•	Requires manual approval to prevent accidental changes.

Terraform Destroy (.tf_destroy)
	•	Destroys Terraform-managed resources.
	•	Restricted to default branch or manual trigger.
	•	Option to disable this job via the DISABLE_TERRAFORM_DESTROY variable.

Key Variables
	•	TF_ROOT: Path to the Terraform root directory (stacks).
	•	TF_ADDRESS: Backend address for Terraform state.
	•	TF_PLAN_JSON: File path for the Terraform plan JSON artifact.
	•	TF_INSTANCE and TF_STACK: Represent environment and configuration context.

Artifacts and Rules
	•	Artifacts:
	•	Store generated files like Terraform plans, JUnit reports, and cached resources.
	•	Rules:
	•	Control when jobs run (e.g., always, on success, manual triggers).

Key Features
	•	Reusable Templates: Modularized commands for DRY (Don’t Repeat Yourself) principles.
	•	AWS Integration: Built-in support for AWS CLI and OIDC.
	•	Security Scans: Checkov and TFLint integration ensure secure, high-quality Terraform code.
	•	Environment-Specific Logic: Jobs are configured for specific environments (dev, prod).

This CI/CD configuration is robust and well-suited for managing Terraform workflows in a structured, automated, and secure manner.


## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)