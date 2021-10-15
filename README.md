# notify-deployment-composite-action

This GitHub Composite Action helps to notify channels with latest deployed version all together.

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
    runs-on: ubuntu-latest

    if: always()

    steps:
      - name: Notify Deployment
        uses: satws/notify-deployment-composite-action@latest
        env:
          GITHUB_TOKEN: ${{ secrets.JIRA_GITHUB_TOKEN }}
          SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
          SENTRY_ORG: ${{ secrets.SENTRY_ORG }}
          SENTRY_PROJECT: ${{ github.event.repository.name }}
        with:
          jira_client_id: ${{ secrets.JIRA_CLIENT_ID }}
          jira_secret: ${{ secrets.JIRA_SECRET }}
          jira_url: ${{ secrets.JIRA_URL }}
          jira_ticket: ${{ github.event.client_payload.jira_ticket || '' }}
          environment: ${{ github.event.client_payload.environment || 'sandbox' }}
          deploy_state: ${{ (needs.deploy.outputs.deploy_state == 0 && 'successful') || 'failed' }}
```

### Todo

- [ ] Automate the release tagging when merging to master.
- [ ] Add conditional statement once GitHub Composite Actions allow it to trigger each channel. 
- [ ] Add New Relic channel once we get all products integrated with it.
