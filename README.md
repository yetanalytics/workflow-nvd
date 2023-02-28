# workflow-nvd
Workflows to use for NVD scanning. Includes two workflows: `nvd-scan` and `notify-slack`.

## `nvd-scan`

The following are the inputs to the `nvd-scan` workflow, which is used to perform scans for CVEs against the National Vulnerability Database by calling out to the [nvd-clojure](https://github.com/rm-hull/nvd-clojure) app.

| Input                 | Description                    | Default
| ---                   | ---                            | ---
| `classpath-command`   | nvd-clojure classpath command  | `clojure -Spath`
| `nvd-clojure-version` | nvd-clojure version            | `2.9.0`
| `nvd-config-filename` | nvd-clojure configuration file | None

## `notify-slack`

The following are the inputs to the `notify-slack` workflow, which uses the [slack-github-action](https://github.com/slackapi/slack-github-action) to send a notification to Slack with a link to the current active GitHub Actions run.

| Input                | Description                                      | Default
| ---                  | ---                                              | ---
| `repo-name`          | GitHub repo name (e.g. `yetanalytics/lrsql`)     | None (required input)
| `link-variable-name` | Slack workflow var name for the GitHub repo link | `run_link`

In addition, the workflow requires a `SLACK_WEBHOOK_URL` secret to be stored in the repository secrets.

To use, first [create a Slack workflow](https://slack.com/help/articles/360053571454-Set-up-a-workflow-in-Slack) (not to be confused with a GitHub workflow). The workflow should include a variable named `run_link` (or whatever name `link-variable-name` is set to). Then run it whenever NVD scanning fails by adding the following job to the repo workflow:

```yaml
  notify_slack:
    uses: yetanalytics/workflow-nvd/.github/workflows/notify-slack.yml@current-version
    with:
      repo-name: 'yetanalytics/my-repo'
    env:
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

