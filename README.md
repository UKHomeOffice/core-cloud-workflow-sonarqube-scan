# core-cloud-workflow-sonarqube-scan

## Overview

This repository contains reusable Github Actions workflow files for SAST and code quality scanning using Sonarqube Scanner. Unlike the Checkov SAST scan, Sonarqube scanner is not mandatory but is an excellent tool for code quality checks.

Add the following workflow config in your repo:

     name: Sonarqube Scanner
     
     on:
       workflow_call:
         secrets:
           sonar_token:
             required: true
           sonar_host_url:
             required: true
     
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

TODO: Change logic for Sonar token

If you wish to use a sonar-project.properties file for more granualar configuration, place it in your root directory.

     # Example Properties file
     # If no projectKey and/or projectName is provided, it will automatically use the repo name
     sonar.projectKey=UKHomeOffice:james-test-sonarqube-name-override
     sonar.projectName=James Test Sonarqube Name Override
     sonar.projectVersion=1.0.0
     sonar.sources=.
     # if you wish
     sonar.qualitygate.wait=false

Please consult the Sonarqube documentation [here](https://docs.sonarsource.com/sonarqube-cloud/advanced-setup/analysis-parameters/) for more extensive configuration.


