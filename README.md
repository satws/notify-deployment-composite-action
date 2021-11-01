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
    needs: deploy

    runs-on: ubuntu-latest

    if: always()

    steps:
      - name: Notify Deployment
        uses: satws/notify-deployment-composite-action@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
          SENTRY_ORG: ${{ secrets.SENTRY_ORG }}
          SENTRY_PROJECT: ${{ github.event.repository.name }}
        with:
          jira_client_id: ${{ secrets.JIRA_CLIENT_ID }}
          jira_secret: ${{ secrets.JIRA_SECRET }}
          jira_url: ${{ secrets.JIRA_URL }}
          jira_ticket: ${{ github.event.client_payload.jira_ticket || '' }}
          short_sha: ${{ needs.deploy.outputs.short_sha || '' }}
          environment: ${{ github.event.client_payload.environment || 'sandbox' }}
          deploy_state: ${{ (needs.deploy.outputs.deploy_state == 0 && 'successful') || 'failed' }}
```

### Todo

- [ ] Automate the release tagging when merging to master.
- [ ] Add conditional statement once GitHub Composite Actions allow it to trigger each channel. 
- [ ] Add New Relic channel once we get all products integrated with it.
