# Deployment validation workflows

## Purpose

This repository holds only GitHub workflows.

It validates GCP deployments triggered from [gke-sample-app-deployments repository](https://github.com/ksiedlarek/gke-sample-app-deployments).

Current validation is **simple** and can be expanded over time.

## Description
There are two ways of triggering pipelines:
- [automatic](https://github.com/marketplace/actions/repository-dispatch) - if workflow from gke-sample-app-deployments repository finishes successfully, at the very last step payload will be send to this repository.

<img src="/docs/img/flow-a.png" width="auto" margin="auto">

- [manual](https://docs.github.com/en/actions/managing-workflow-runs/manually-running-a-workflow): you can trigger _Manually triggered validation_ workflow from Actions tab. You will have to provide:
    - project name (without spaces)
    - IP/link of your app to be checked by curl
    - RUN id of your deployment pipeline
    - Repository link (defaults to: https://github.com/ksiedlarek/gke-sample-app-deployments)

<img src="/docs/img/run.png" width="auto" margin="auto">

<img src="/docs/img/flow-m.png" width="auto" margin="auto">

Current checks:
- run curl on provided IP/app link
- get logs from specific run from gke-sample-app-deployments repository (using [GitHub CLI](https://github.com/github/hub))
- upload logs to bucket on GCP platform
- check if specific workflow run from gke-sample-app-deployments ended successfully (for manual trigger only, using GitHub CLI)

## Setup

1. Fork this repository (or clone and setup on GitHub)
2. Go to Repository Settings tab -> Secrets, and add:
- GCP_CREDENTIALS -> base64 encoded json file content (your GCP service account credentials)
- GH_TOKEN -> on your GitHub account level/organisation level, create access token that can read repository information and trigger workflows - paste it here. This is needed to query logs from other repositories. (if you don't want to do that, you can skip it - comment out section of code in the workflow responsible for log upload)
- Navigate to .github/workflows, in triggered-workflow.yml and manual-trigger.yml:
change BUCKET_NAME to GCP bucket that exists on your GCP project - here all of the logs will be stored

**This repository is only a demo, it does not contain production-ready environment.**