# workflow-nvd
Reusable workflow to use for NVD scanning.

The following are the inputs to the `nvd-scan` workflow, which is used to perform scans for CVEs against the National Vulnerability Database by calling out to the [nvd-clojure](https://github.com/rm-hull/nvd-clojure) app.

| Input                  | Description                                      | Default
| ---                    | ---                                              | ---
| `classpath-command`    | nvd-clojure classpath command                    | `clojure -Spath`
| `nvd-clojure-version`  | nvd-clojure version                              | `3.2.0`
| `nvd-config-filename`  | nvd-clojure configuration file                   | None
| `notify-slack`         | Whether or not to report scan failures to Slack  | `false`
| `notify-link-var-name` | Slack workflow variable name for the CI run link | `run_link`

If `notify-slack` is true, then an NVD scan failure will result in a notification being posted to Slack, with the link to the failed CI run. To use:
1. [Create a Slack workflow](https://slack.com/help/articles/360053571454-Set-up-a-workflow-in-Slack) (not to be confused with a GitHub workflow). The workflow should include a variable named `run_link` (or whatever name `notify-link-var-name` is set to) in the JSON payload, whose value will be set to the CI run link.
2. Create a GitHub repository secret `SLACK_WEBHOOK_URL` using the generated webhook URL.
3. Activate Slack notifications and pass the secret as follows:

```yaml
  notify_slack:
    uses: yetanalytics/workflow-nvd/.github/workflows/nvd-scan.yml@[current-version]
    with:
      notify-slack: true
    secrets:
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

Alternatively you can pass the secret as `secrets: inherit`.
