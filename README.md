# notify-deployment-composite-action

This GitHub Composite Action helps to notify channels with latest deployed version all together.

### Requirements

Expects that the following secrets are available depending on which channels are enabled:
- GITHUB_TOKEN
- JIRA_CLIENT_ID
- JIRA_SECRET
- JIRA_URL
- SENTRY_AUTH_TOKEN
- SENTRY_ORG

### Options

Check the `action.yaml` for descriptions of the available options.

### Example

Within your GitHub Action workflow add:
```yaml
name: CD

on:
  push:

jobs:
  deploy:
  ...

  notify:
    needs: deploy

    runs-on: ubuntu-latest

    if: always()

    steps:
      - name: Notify Deployment
        uses: satws/notify-deployment-composite-action@main
        with:
          revision: ${{ needs.deploy.outputs.short_sha || '' }}
          environment: ${{ github.event.client_payload.environment || 'sandbox' }}
          deploy-state: ${{ (needs.deploy.outputs.deploy_state == 0 && 'successful') || 'failed' }}
          jira: true
          jira-ticket: ${{ github.event.client_payload.jira_ticket || '' }}
          new-relic: true
          new-relic-application-id: ${{ secrets.APPLICAITON_ID }}
          sentry: true
          sentry-override-environment: 'prod'
```

### Todo

- [ ] Automate the release tagging when merging to master.
- [X] Add conditional statement once GitHub Composite Actions allow it to trigger each channel. 
- [X] Add New Relic channel once we get all products integrated with it.
