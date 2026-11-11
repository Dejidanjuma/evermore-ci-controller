# Evermore private-source CI controller

This public repository contains only a manually triggered validation workflow. The private Evermore application source is checked out into an ephemeral GitHub-hosted runner; the controller does not deploy the application or mutate production resources.

## Required credential

Create a fine-grained personal access token with:
- Repository access limited to `Dejidanjuma/evermore` only.
- **Contents: Read-only** permission only.
- No Administration, Actions, Deployments, Environments, Secrets, Variables, or workflow-write permissions.

Store it as the repository Actions secret `EVERMORE_READ_TOKEN`. Never put the token in source, workflow inputs, artifacts, or logs. The workflow uses `persist-credentials: false` for both checkouts.

## Validation boundary

- The workflow currently checks out `feat/customer-resident-portal`; it does not validate `main` unless the workflow is deliberately updated and reviewed.
- CI database and Supabase variables are local placeholders, not production credentials.
- The workflow regenerates a dependency lockfile in the ephemeral checkout, installs dependencies, runs dependency audit/tests/typecheck/build, and uploads only package manifests for review.
- It does not commit changes to the private repository, apply production migrations, publish artifacts to production, or deploy.

## Operations

Run only through Actions → Validate private Evermore → Run workflow. Review the exact checked-out commit and all stage results. A successful build is not deployment approval.
