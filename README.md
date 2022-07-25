[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/MercymeIlya/last-workflow-status/blob/master/LICENSE)

# Last workflow status

Simple GitHub action to get previous workflow conclusion/status. Was inspired by sending notification after build status changing in 
[Travis CI](https://docs.travis-ci.com/user/notifications/#changing-notification-frequency).
```yaml
notifications:
  slack:
    rooms: slack_room
    on_success: change
```

## Inputs:
### `github_token`
* Secret GitHub API token to use for making API requests.  
`default: ${{ github.token }}`

## Outputs:
### `last_status`
* Conclusion value of last workflow.

See https://docs.github.com/en/rest/checks/runs#create-a-check-run--parameters conclusion parameter.

## Example usage

```yaml
jobs:
  yor-job:
    runs-on: ubuntu-latest
    steps:
      - name: Get previous workflow status
        uses: Mercymeilya/last-workflow-status@v0.3
        id: last_status
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}

      - name: Any action
        run: slyle-check.sh

      - name: Build fixed slack message
        uses: rtCamp/action-slack-notify@v2.1.3
        if: ${{ success() && steps.last_status.outputs.last_status == 'failure' }}
        env: 
          SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
          SLACK_MESSAGE: 'Style check fixed now!'
```         
