# AWS Control Tower

- Quick and easy setup of multi-account AWS environment
- Orchestrates other AWS services to provide this functionality
  - AWS Organizations, IAM Identity Center, CloudFormation, Config...
- Landing zone - multi-account AWS environment
  - Pre-configured with best-practices blueprints
  - Can be customized
- Guardrails: rules that control what can be done in the landing zone and rules/standards across all accounts
- Account factory: automates and standardizes creation of new accounts
- Dashboard: single page oversight of the entire environment

## Architecture

- Managmenet account: where Control Tower is deployed with AWS orgs, foundational OU and custom OU. As well as an Audit account and log archive account
- Account factory: both control tower console or via service catalog. and automates the creation of new accounts
  - Account baseline template and network baseline template
- Control Tower uses CloudFormation under the hood to automate this creation. 
- Control Tower uses both AWS config and SCPs to enforce guardrails

### Control Tower - Landing Zone

- Well-architected multi-account AWS environment - home region
  - built with AWS organizations, Config and CloudFormation
- Security OU: Log archive and Audit accounts (cloudtrail and config logs)
- Sandbox OU: for experimentation and testing. Less security
- You can create other OUs and accounts.
- IAM identity center: SSO for all accounts, ID federation
- Landing zone provides monitoring and notifcations: CloudWatch and SNS
- End users can provision new accounts via service catalog

### Guardrails

- Guardrails are rules: multi-account governance
  - Mandatory: strongly recommeneded or elec
  - Preventive: Stop you doing things (AWS Org SCPs)
    - Enforced or not enabled
    - i.e. allow or deny regions or disallow bucket policy changes
  - Detective: compliance checks (AWS Config)
    - clear, in violation or not enabled
    - detect CloudTrail disabled, S3 bucket public access or EC2 public IPV4 
- Preventive guardrails stops thing happening but Detective guardrails will only identify them

### Account Factory

- Automated account provisioning
  - cloud admins or end users with appropriate permissions can do this
- Guardrails - automatically added
- Account admin given to any named user (IAM Identity Center)
- Account and network standard configuration
- Accounts can be closed or repurposed
- Can be fully integrated with a business SDLC (like APIs or Terraform)
- 
