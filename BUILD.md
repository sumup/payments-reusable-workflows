## Build and Test Application

Builds image from Dockerfiles located in .deployment/docker sub-directory

Used in PR or on push to branch ( not main/master )

#### Inputs

  - **ssh_agent_key** Used if during build dependencies/libraries require to be fetched from a  private repo
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
  - **slack_channel** The slack channel where you want your messages being sent
      - required No
      - default ''
  - **trivy_version** Version of trivy scanner
      - required No
      - default 0.27.1

#### Secrets
  - **gh_token**
      - required Yes
  - **slack_webhook_url**
      - required No

#### Example usage

 - Simplest possible example:

```yaml
name: Build,Test and Scan
on:
  push:
    branches-ignore:
      - main

jobs:
  build:
    uses: sumup/payments-reusable-workflows/.github/workflows/build.yaml
    with:
      slack_channel: "channel"
      test_commands: |
        go run mage.go -v lint
        go run mage.go -v test
    secrets:
      gh_token: ${{ secrets.GH_TOKEN }}
      slack_webhook_url: ${{ secrets.SLACK_WEBHOOK_URL }}

```
