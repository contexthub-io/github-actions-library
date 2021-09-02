# github-actions-library

Several github actions we use at contexthub.io.

**Please note that this repository is public so it can be accessed by github actions**

## terraform-plan-pr-comment

Run terraform plan in a pull request and add the plan output as a comment to the PR.

```
jobs:
  plan:
    steps:
      - name: Terraform plan
        id: plan
        run: terraform plan -no-color -detailed-exitcode
        continue-on-error: true

      - name: Add PR comment if plan has changes
        if: steps.plan.outputs.exitcode == 2
        uses: contexthub-io/github-actions-library/plan-pr-comment@main
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          plan-stdout: ${{ steps.plan.outputs.stdout }}

      - name: Exit if terraform plan failed
        if: steps.plan.outputs.exitcode == 1
        run: exit 1
```
