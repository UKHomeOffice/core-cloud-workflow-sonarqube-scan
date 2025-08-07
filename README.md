# core-cloud-workflow-sonarqube-scan

## Overview
This repository contains reusable Github Actions workflow files for using Sonarqube scanner. Once the scan is complete, the workflow will push the results to our Sonarqube instance for analysis and, if executed via PR instead of on every commit push, will auto-comment the results on your respective PR.

The reusable workflow file exists here. This is for informational purposes only.
[sonarqube-scan.yaml](https://github.com/UKHomeOffice/core-cloud-workflow-sonarqube-scan/blob/main/.github/workflows/sonarqube-scan.yaml)

## Implementation

Add the SONAR_TOKEN and SONAR_HOST_URL environment secrets to your repo.

#TODO Currently using a Global Analysis Token for the admin account. This is temporary whilst we wait for the SAML accounts to be fixed so we can test the solution of using a global analysis token for a non-admin scoped user account acting as a service account. Docs suggest that global analysis token means just that, but that seems like a security flaw. Will confirm once testing can continue.

Add the following config into the following directory in your repository `.github/workflow/sonarqube-scan.yaml`, or build into your own workflow logic if more complex.

    name: Sonarqube Scanner
    
    on:
      workflow_call:
        secrets:
          sonar_token:
            required: true
          sonar_host_url:
            required: true
      push:
        branches:
          - main
      pull_request:
        branches:
          - main
    
    permissions:
      contents: read
      id-token: write
      actions: read
      security-events: write
    
    jobs:
      sonarqube-scanner:
        uses: UKHomeOffice/core-cloud-workflow-sonarqube-scan/.github/workflows/sonarqube-scan.yaml@1.0.0
        secrets:
          sonar_token: ${{ secrets.sonar_token }}
          sonar_host_url: ${{ secrets.sonar_host_url }}

# Notes
- If you wish to add your own `sonar-project.properties` file for further customisation of your Sonarqube project, this is supported by the workflow. Please add this to your repo's root directory. If you wish to use your own projectKey and name instead of the repo name, you can change this here. An example of this config would be
```
     sonar.projectKey=UKHomeOffice:james-test-sonarqube-name-override
     sonar.projectName=James Test Sonarqube Name Override
     sonar.projectVersion=1.0.0
     sonar.sources=.
     sonar.qualitygate.wait=false
     sonar.issues.fail=false
```
- When adding the Sonar feature to an existing repo, it would be best to push the Sonar feature on its own to your primary branch. This will highlight any existing code quality issues your primary branch currently has.
