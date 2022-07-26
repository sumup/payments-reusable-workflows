## Build and Test Application
- Lints commit mesages(could be skipped)
- Builds image from Dockerfiles located in .deployment/docker sub-directory
- Used in PR or on push to branch ( not main/master )
- In order to receive Slack notifications in your channel:
```
/invite @github_actions_ci 
```

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
    secrets: inherit
```

- Example with commit linting enabled:

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
      enable_commitlint: true
      commitlint_fail_on_warning: false
      commitlint_config_path: .github/.commitlintrc.json
      commitlint_help_url: https://github.com/org/repo#commitlint
      slack_channel: "channel"
      test_commands: |
        go run mage.go -v lint
        go run mage.go -v test
    secrets: inherit
```

#### Inputs

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
  - **project_dir** The application/project folder relative to git root 
      - required No
      - default '.'
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