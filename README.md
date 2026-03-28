[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/MercymeIlya/last-workflow-status/blob/master/LICENSE)

# Last workflow status

Simple GitHub Action to get the previous workflow run's conclusion/status on the current branch.

Inspired by [Travis CI's](https://docs.travis-ci.com/user/notifications/#changing-notification-frequency) `on_success: change` notification feature — useful for sending alerts only when the build status changes.

## Inputs

### `github_token`
* Secret GitHub API token used for API requests.
* Default: `${{ github.token }}` — works automatically, no need to pass it explicitly unless you need a custom token.

## Outputs

### `last_status`
* Conclusion of the last completed workflow run. Possible values:
  * `success`, `failure`, `cancelled`, `skipped`, `timed_out`, `action_required`, `neutral`, `stale`
  * `null` — if there is no previous run

See [GitHub docs](https://docs.github.com/en/rest/checks/runs#create-a-check-run--parameters) for more details on conclusion values.

## Example usage

```yaml
jobs:
  your-job:
    runs-on: ubuntu-latest
    steps:
      - name: Get previous workflow status
        uses: Mercymeilya/last-workflow-status@v0
        id: last_status

      - name: Any action
        run: style-check.sh

      - name: Build fixed slack message
        uses: rtCamp/action-slack-notify@v2.1.3
        if: ${{ success() && steps.last_status.outputs.last_status == 'failure' }}
        env:
          SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
          SLACK_MESSAGE: 'Style check fixed now!'
```
