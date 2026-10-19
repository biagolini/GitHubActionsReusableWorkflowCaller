# GitHub Actions Reusable Workflow Caller (secret leak demo)

**Author:** Carlos Biagolini-Jr.

**LinkedIn:** [https://www.linkedin.com/in/biagolini/](https://www.linkedin.com/in/biagolini/)

**Medium:** [https://medium.com/@biagolini](https://medium.com/@biagolini)

## Overview

This repository is the "caller" side of a two-repository demo. Its workflows contain no pipeline logic: each one calls a [reusable workflow](https://docs.github.com/en/actions/sharing-automations/reusing-workflows) from the central catalog [GitHubActionsReusableWorkflowCatalog](https://github.com/biagolini/GitHubActionsReusableWorkflowCatalog) and passes two repository secrets packed in a JSON secret.

| Workflow | Calls | Result in the run log |
|---|---|---|
| `.github/workflows/pipeline-vulnerable.yml` | `terraform-vulnerable.yml@main` | `TF_VAR_db_password` and `TF_VAR_api_private_key` in plain text |
| `.github/workflows/pipeline-masked.yml` | `terraform-masked.yml@main` | the same variables shown as `***` |

The secrets `DB_PASSWORD` and `API_PRIVATE_KEY` hold fake values created only for this demo.

## Run it

```bash
gh workflow run pipeline-vulnerable.yml --repo biagolini/GitHubActionsReusableWorkflowCaller
gh workflow run pipeline-masked.yml --repo biagolini/GitHubActionsReusableWorkflowCaller
```

Open each run, expand the step `Terraform plan (simulated)` and look at its `env:` group.
