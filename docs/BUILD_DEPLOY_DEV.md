## Build,Test and Deploy Application to development env
- Lints commit mesages(could be skipped)
- Builds image from Dockerfiles located in .deployment/docker sub-directory
- Tests and Packages helm chart
- Creates PR in deploy repo for development env 
- Option to merge PR in deploy repo for development env 

Used on push to main/master branch

In order to receive Slack notifications in your channel:
```
/invite @github_actions_ci 
```

#### Example usage

 - Simplest possible example:

```yaml
name: Build, Test and Deploy to Theta
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
 - With deployment repo PR auto-merge example:

```yaml
name: Build, Test and Deploy to Theta
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
      auto_merge: 'true'
      test_commands: |
        go run mage.go -v lint
        go run mage.go -v test
    secrets: inherit

``` 

- Example with commit linting enabled:

```yaml
name: Build, Test and Deploy to Theta
on:  
  push:
    branches:
      - main

jobs:
  deploy-dev:
    uses: sumup/payments-reusable-workflows/.github/workflows/build-deploy-dev.yaml
    with:
      enable_commitlint: true
      commitlint_fail_on_warning: false
      commitlint_config_path: .github/.commitlintrc.json
      commitlint_help_url: https://github.com/org/repo#commitlint
      image_repository: nexus.sam-app.ro:5001
      slack_channel: "channel"
      deployment_config_path: projects/my-team/my-app/values-theta.yaml
      chart_repository: s3://helm-charts/my-team/
      auto_merge: 'true'
      test_commands: |
        go run mage.go -v lint
        go run mage.go -v test
    secrets: inherit
```
    

#### Inputs
  - **image_repository**  The image repository for the built images, defaults to value of `Image.repository` from `values.yaml`
      - required No
      - default nexus.sam-app.ro:5000
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
  - **project_dir** The application/project folder relative to git root 
      - required No
      - default '.'
  - **auto_merge** Auto merge the created PR. Will not be merged by default
      - required No
      - default ''
- **enable_commitlint** Enables commit linting
    - default ''
    - required No
- **commitlint_help_url** A help URL to a page explaining your commit message conventions
    - required No
    - default "https://github.com/conventional-changelog/commitlint/#what-is-commitlint"
- **commitlint_config_path** Configuration file for the commit lint action. Default: .commitlint.config.js
    - required No
    - default ".commitlint.config.js"
- **commitlint_fail_on_warning** Fails commitlint on warning
    - required No
    - default true