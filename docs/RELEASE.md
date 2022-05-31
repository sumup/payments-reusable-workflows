## Deploy to Environment

- Creates PR in deploy repo for Environment

Used on release

In order to receive Slack notifications in your channel:
```
/invite @github_actions_ci 
```

#### Example usage

 - Simplest possible example:

```yaml
name: Deploy to Environment
on:  
  push:
    branches:
      - main

jobs:
  release:
    uses: sumup/payments-reusable-workflows/.github/workflows/release.yaml
    with:
      environment_config_path: projects/my-team/my-app/values-prod.yaml
    secrets: inherit
  
```

#### Inputs
  - ** environment_name** Name of the environment 
      - required No
      - default Prod
  - ** environment_config_path**  Path to configuration file for environment
      - required Yes
  - **slack_channel** The slack channel where you want your messages being sent
      - required No
      - default ''
  - **project_dir** The application/project folder relative to git root 
      - required No
      - default '.'

