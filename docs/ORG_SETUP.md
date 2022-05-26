## Organization setup

### Secrets

To use the workflows the following secrets need to exist in our Github Org: 

 - **GH_ACTIONS_CI_TOKEN** Github token used to checkout necessary repositories. 

   This secrets is mandatory.

 - **GH_ACTIONS_SLACK_TOKEN** Slack app bot Oauth token for sending notification messages.

   This secret is optional, if not set no notifications will be send via slack. 

 - **ACTIONS_SSH_AGENT_KEY** SSH key used to pull packages from private repositories during build

   This secret is optional, if not set no ssh key will be mounted into the build. 
