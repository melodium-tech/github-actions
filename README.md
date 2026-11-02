
# Mélodium on Github Actions

This project contains reusable workflows to use Mélodium in Github Actions.

## Reusable Workflows

### Mélodium in Github Actions

Use this workflow to run Mélodium in Github Action.

To add this workflow to your CI/CD pipeline, add the following include entry to your
project CI/CD configuration:

```yaml
jobs:
  melodium:
    uses: melodium-tech/github-actions/.github/workflows/melodium.yml@<tag>
    with:
      command: run --api-report .melodium-ci/Compo.toml
      artifact-path: 'logs/'
    secrets:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      melodium-api-token: ${{ secrets.MELODIUM_API_TOKEN }}
```

Where `<tag>` is the release tag you want to use (usually Mélodium version itself, e.g. `v0.11.0`).  
Refer to the workflow file itself to see all available inputs about checkout and artifacts.

### Mélodium in Github with local distribution

Use this workflow to run Mélodium in Github Actions with local distribution available.

To add this workflow to your CI/CD pipeline, add the following include entry to your
project CI/CD configuration:

```yaml
jobs:
  melodium:
    uses: melodium-tech/github-actions/.github/workflows/melodium-local-distrib.yml@<tag>
    with:
      command: run --api-report .melodium-ci/Compo.toml
      artifact-path: 'logs/'
    secrets:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      melodium-api-token: ${{ secrets.MELODIUM_API_TOKEN }}
```

Where `<tag>` is the release tag you want to use (usually Mélodium version itself, e.g. `v0.11.0`).  
Refer to the workflow file itself to see all available inputs about checkout and artifacts.

### Mélodium in Github on Ubuntu

Use this workflow to run Mélodium in Github Actions on Ubuntu.

To add this workflow to your CI/CD pipeline, add the following include entry to your
project CI/CD configuration:

```yaml
jobs:
  melodium:
    uses: melodium-tech/github-actions/.github/workflows/melodium-ubuntu.yml@<tag>
    with:
      command: run --api-report .melodium-ci/Compo.toml
      artifact-path: 'logs/'
    secrets:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      melodium-api-token: ${{ secrets.MELODIUM_API_TOKEN }}
```

Where `<tag>` is the release tag you want to use (usually Mélodium version itself, e.g. `v0.11.0`).  
Refer to the workflow file itself to see all available inputs about checkout and artifacts.


### Mélodium in Github on Windows

Use this workflow to run Mélodium in Github Actions on Windows.

To add this workflow to your CI/CD pipeline, add the following include entry to your
project CI/CD configuration:

```yaml
jobs:
  melodium:
    uses: melodium-tech/github-actions/.github/workflows/melodium-windows.yml@<tag>
    with:
      command: run --api-report .melodium-ci/Compo.toml
      artifact-path: 'logs/'
    secrets:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      melodium-api-token: ${{ secrets.MELODIUM_API_TOKEN }}
```

Where `<tag>` is the release tag you want to use (usually Mélodium version itself, e.g. `v0.11.0`).  
Refer to the workflow file itself to see all available inputs about checkout and artifacts.

### Mélodium in Github on Mac OS

Use this workflow to run Mélodium in Github Actions on Mac OS.

To add this workflow to your CI/CD pipeline, add the following include entry to your
project CI/CD configuration:

```yaml
jobs:
  melodium:
    uses: melodium-tech/github-actions/.github/workflows/melodium-macos.yml@<tag>
    with:
      command: run --api-report .melodium-ci/Compo.toml
      artifact-path: 'logs/'
    secrets:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      melodium-api-token: ${{ secrets.MELODIUM_API_TOKEN }}
```

Where `<tag>` is the release tag you want to use (usually Mélodium version itself, e.g. `v0.11.0`).  
Refer to the workflow file itself to see all available inputs about checkout and artifacts.

## Inputs, checkout, and artifacts

There are some optional inputs, please refer to the workflow file itself to see all available inputs about checkout and artifacts.

| Input      | Default value    | Description |
|------------|------------------|-------------|
| `version`  | `0.11.0`    | The Mélodium version to use (should usually be the same as the component). |
| `edition`  | `alpine`         | The Mélodium edition to use (_available only for pure Mélodium `melodium.yml`_). |
| `command`  | `run Compo.toml` | The Mélodium command to run. |
| `repository`  | `${{ github.repository }}` | Repository name with owner. For example, actions/checkout' |
| `ref`  | _default branch_ | The branch, tag or SHA to checkout. |
| `artifact-path`  | `''`| Path to artifact to upload. |
| `environment`  | _none_ | GitHub environment of the job, such as the one holding the [cache scope](#cache-scopes) token. |
| `melodium-api-token`  | _none_ | _Secret_ The Mélodium API token for reporting and work distribution, see service like [Cadence.CI](https://cadence.ci). |
| `github-token`  | _required_ | _Secret_ The token used to checkout the repository, also given to the program as `MELODIUM_SECRET_GITHUB_TOKEN`. |
| `secret-1` to `secret-10`  | _none_ | _Secret_ Given to the program as `MELODIUM_SECRET_1` to `MELODIUM_SECRET_10`. |

## Secrets

Since Mélodium 0.11.0, credentials are `Secret<string>` parameters. A secret parameter takes a locator telling where its value comes from, never the value itself:

```shell
melodium run .melodium-ci/Compo.toml --ci_service_token env:MELODIUM_SECRET_1
```

A plain value given to a secret parameter is refused. Locators can also be `file:<path>`, or `vault:<path>#<field>` with the `vault` package.

Programs only read the environment variables whose name starts with `MELODIUM_SECRET_`, so the workflows give their secrets under that prefix:

| Workflow secret | Environment variable | Locator |
|-----------------|----------------------|---------|
| `github-token` | `MELODIUM_SECRET_GITHUB_TOKEN` | `env:MELODIUM_SECRET_GITHUB_TOKEN`, default status token of `cicd` steps |
| `secret-1` to `secret-10` | `MELODIUM_SECRET_1` to `MELODIUM_SECRET_10` | `env:MELODIUM_SECRET_1` to `env:MELODIUM_SECRET_10` |
| `melodium-api-token` | `MELODIUM_API_TOKEN` | _none_, read by Mélodium itself |
| `MELODIUM_CACHE_SCOPE` | `MELODIUM_CACHE_SCOPE` | _none_, read by Mélodium itself (see [Cache scopes](#cache-scopes)) |

For example, to give a token stored as `NPM_TOKEN` in the repository secrets to a `npm_token: Secret<string>` parameter:

```yaml
jobs:
  melodium:
    uses: melodium-tech/github-actions/.github/workflows/melodium.yml@<tag>
    with:
      command: run --check-secrets --api-report .melodium-ci/Compo.toml --npm_token env:MELODIUM_SECRET_1
      artifact-path: 'logs/'
    secrets:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      melodium-api-token: ${{ secrets.MELODIUM_API_TOKEN }}
      secret-1: ${{ secrets.NPM_TOKEN }}
```

`--check-secrets` resolves the secrets given to the entrypoint before running, so a missing one fails the job at startup.

The `api_token` parameter of `CicdDispatchEngine` is optional, Mélodium uses `MELODIUM_API_TOKEN` when it is not given: use the `melodium-api-token` secret rather than a command argument.

### Vault with GitHub OIDC

The `vault` package can log in to HashiCorp Vault or OpenBao with the GitHub OIDC token of the job. Give the calling job the `id-token: write` permission; the workflows then give the runner's `ACTIONS_ID_TOKEN_REQUEST_TOKEN` to the program as `MELODIUM_SECRET_ACTIONS_ID_TOKEN_REQUEST_TOKEN`, the default of the `Vault` model.

```yaml
jobs:
  melodium:
    permissions:
      contents: read
      id-token: write
    uses: melodium-tech/github-actions/.github/workflows/melodium.yml@<tag>
    ...
```

## Cache scopes

Caches saved by runs on Cadence.CI are shared by every run of the organization, unless a run is requested with a cache scope token (`cad_cs_...`, created on the _Storage_ page of Cadence.CI): it then reads and writes the caches of that scope only. Give the token to protected branches only, so that a pull request never writes a cache the default branch then builds with.

Repository secrets are not enough for this: a branch pushed by any collaborator can change its workflow and read them. Environment secrets are:

1. In _Settings > Environments_, create an environment, such as `protected`, whose _Deployment branches and tags_ are your protected branches only.
2. Add to it a secret named `MELODIUM_CACHE_SCOPE` holding the token. Environment secret names cannot hold `-`, hence a name unlike the other secrets of the workflows.
3. Give the environment with the `environment` input, on those branches only. Other branches run without environment, and use the default caches of the organization:
   ```yaml
   jobs:
     melodium:
       uses: melodium-tech/github-actions/.github/workflows/melodium.yml@<tag>
       with:
         environment: ${{ github.ref == 'refs/heads/main' && 'protected' || '' }}
       secrets:
         github-token: ${{ secrets.GITHUB_TOKEN }}
         melodium-api-token: ${{ secrets.MELODIUM_API_TOKEN }}
   ```

GitHub gives the job the environment's `MELODIUM_CACHE_SCOPE`, and Mélodium sends it with the run it requests. Programs never see the token, they only name caches: the same program runs on every branch. It needs a Mélodium version sending the scope in run requests (melodium/melodium#169); older versions ignore it and use the default caches. See [Caches](https://cadence.ci/en/docs/how-it-works/caches#cache-scopes) in the Cadence.CI documentation.

## Migrating from 0.10

- `GITHUB_TOKEN` and `SECRET_1` to `SECRET_10` are now `MELODIUM_SECRET_GITHUB_TOKEN` and `MELODIUM_SECRET_1` to `MELODIUM_SECRET_10`, and programs read them through `env:` locators.
- Credentials are no longer given as command arguments: `--api_token` is replaced by the `melodium-api-token` secret, and other tokens are given as locators (`--ci_service_token env:MELODIUM_SECRET_1`).
- The `log` package and `cicd/logging` are removed: logs are written with `melodium run --logs <file>`.
- String arguments are given raw unless `--parse-arguments` is used: write `--repository_clone_ref main`, not `--repository_clone_ref '"main"'`.
- A program that fails to launch (missing parameter, unresolvable secret with `--check-secrets`) now exits with code 1, failing the job. A run cancelled with SIGTERM ends gracefully within 10 seconds and exits with code 143 (128 plus the signal number).
- Mélodium 0.11.0 trusts the renewed distribution certificates. Earlier versions trust a root expiring on 2026-12-23, after which they can no longer reach distribution nodes: upgrade any pinned `version` before then.

See also the [Github Actions migration guide](https://gitlab.com/melodium/melodium/-/blob/master/skills/melodium/references/github-migration.md).


## Documentation

Take look at [Mélodium CI/CD Module Documentation](https://doc.melodium.tech/latest/en/cicd/index.html) and [Cadence.CI Documentation](https://cadence.ci/en/docs/from-gitlab) for more informations.

## Licence

This software is free and open-source, under the EUPL licence.

> Licensed under the EUPL, Version 1.2 or - as soon they will be approved by the European Commission - subsequent versions of the EUPL (the "Licence"); You may not use this work except in compliance with the Licence. You may obtain a copy of the Licence at: https://joinup.ec.europa.eu/software/page/eupl
>
> Unless required by applicable law or agreed to in writing, software distributed under the Licence is distributed on an "AS IS" basis, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.