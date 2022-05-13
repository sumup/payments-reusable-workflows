## Build,Test and Deploy Application to development env

Builds image from Dockerfiles located in .deployment/docker sub-directory
Tests and Packages helm chart
Creates PR in deploy repo for development env

Used on push to main/master branch

In order to use these workflows you need to add secret for slack webhook named `SLACK_WEBHOOK_URL` in your repo

#### Example usage

 - Simplest possible example:

```yaml
name: Build,Test,Deploy to Theta
on:  
  push:
    branches:
      - main

jobs:
  deploy-dev:
    uses: sumup/payments-reusable-workflows/.github/workflows/build-deploy-dev.yaml
    with:
      image_repository: nexus.sam-app.ro:5001
      slack_channel: "channel"
      deployment_config_path: projects/my-team/my-app/values-theta.yaml
      chart_repository: s3://helm-charts/my-team/
      test_commands: |
        go run mage.go -v lint
        go run mage.go -v test
    secrets: inherit

```

#### Inputs
  - **image_repository**  The image repository for the built images, defaults to value of `Image.repository` from `values.yaml`
      - required No
      - default nexus.sam-app.ro:5000
  - **image_name**  Built image name, defaults to value of Image.name from values.yaml
      - required No
      - default ''
  - **image_tag** Built image tag, defaults to value of `Image.tag` from `values.yaml`
      - required No
      - default ''
  - **target** The build target if image needs to be build to sertain layer ex.builder
      - required No
      - default 'builder'
  - **test_commands** Test commands to run in development container for executing tests, one per line
      - required No
      - default ''
  - **results_path** The directory ( in the container ) where results will be stored
      - required No
      - default /tmp
  - **chart_repository** Chart repository 
      - required Yes
  - **deployment_config_path**  Path to configuration file that needs to be updated
      - required Yes
  - **deployment_environment** deployment env name
      - default 'Theta'
      - required No
  - **slack_channel** The slack channel where you want your messages being sent
      - required No
      - default ''
