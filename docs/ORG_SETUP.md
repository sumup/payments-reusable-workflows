## Organization setup

### Secrets

To use the workflows you will need the following github secrets

 - **GH_ACTIONS_CI_PAT** Github token used to checkout repositories. 

   This secrets is mandatory.

 - **SLACK_WEBHOOK_URL** Slack web hook for sending messages.

   This secret is optional, if not set no notifications will be send via slack

 - **ACTIONS_SSH_AGENT_KEY** SSH key used to pull packages from private repositories during build

   This secret is optional, if not set no ssh key will be mounted into the build. 
