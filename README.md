# Purpose

This repository holds only GitHub workflows.

It validates GCP deployments triggered from [gke-sample-app-deployments repository](https://github.com/ksiedlarek/gke-sample-app-deployments).

There are two ways of triggering pipelines:
- automatic - if workflow from gke-sample-app-deployments finishes successfully, at the very last step payload will be send to this repository.
- manual: you can trigger Manually triggered validation workflow from Actions tab. You will have to provide:
    - project name (without spaces)
    - IP/link of your app to be checked by curl
    - RUN id of your deployment pipeline
    - Repository link

Current validation is simple and can be expanded over time.

Checks:
- run curl on provided IP/app link
- get logs from specific run from gke-sample-app-deployments repository
- upload logs to bucket on GCP platform
- check if specific workflow run from gke-sample-app-deployments ended successfully