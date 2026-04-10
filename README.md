# aws-project-onboarding-sample

Sample application repo that exercises the
[`aws-project-onboarding`](https://github.com/subhamay-bhattacharyya-gha/aws-project-onboarding)
reusable workflow end-to-end.

## Layout

```text
.
├── project-config.json                       # project-specific config (edit me)
└── .github/workflows/
    └── onboard-aws-project.yaml              # caller workflow (workflow_dispatch + push on config change)
```

## How it runs

1. `load-config` job reads `project-config.json`, strips `_comment` keys,
   and exposes the compact JSON as a job output.
2. `onboard` job invokes the reusable workflow
   `aws-project-onboarding/.github/workflows/aws-project-onboarding.yaml@v1`
   with that JSON as the `config` input.

## Required org-level secrets

| Secret | Purpose |
| --- | --- |
| `TFE_TOKEN` | HCP Terraform API token |
| `TF_VCS_OAUTH_TOKEN_ID` | VCS OAuth token ID for the HCP workspace |
| `GH_PAT` | Fine-grained PAT (environments: read/write, administration: write) — passed to the reusable workflow as `GITHUB_PAT` |

## Triggering

- **Automatic:** push a change to `project-config.json` on `main`.
- **Manual:** run the `Onboard AWS Project` workflow via `workflow_dispatch`.

## Notes

- Fill in real `aws_account_id` values before pushing.
- Remove the `snowflake_*` fields from each environment if this repo has no
  Snowflake workloads — those env vars are only created when the fields are
  present.
- The reusable workflow is idempotent, so re-running is safe.
