
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
        command: run .melodium-ci/Compo.toml
        artifact-path: 'logs/'
      secrets:
        token: ${{ secrets.token }}
```

Where `<tag>` is the release tag you want to use (usually Mélodium version itself, e.g. `v0.9.0-pre.2`).  
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
      command: run .melodium-ci/Compo.toml
      artifact-path: 'logs/'
    secrets:
      token: ${{ secrets.token }}
```

Where `<tag>` is the release tag you want to use (usually Mélodium version itself, e.g. `v0.9.0-pre.2`).  
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
      command: run .melodium-ci/Compo.toml
      artifact-path: 'logs/'
    secrets:
      token: ${{ secrets.token }}
```

Where `<tag>` is the release tag you want to use (usually Mélodium version itself, e.g. `v0.9.0-pre.2`).  
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
      command: run .melodium-ci/Compo.toml
      artifact-path: 'logs/'
    secrets:
      token: ${{ secrets.token }}
```

Where `<tag>` is the release tag you want to use (usually Mélodium version itself, e.g. `v0.9.0-pre.2`).  
Refer to the workflow file itself to see all available inputs about checkout and artifacts.

## Inputs, checkout, and artifacts

There are some optional inputs, please refer to the workflow file itself to see all available inputs about checkout and artifacts.

| Input      | Default value    | Description |
|------------|------------------|-------------|
| `version`  | `0.9.0-pre.2`    | The Mélodium version to use (should usually be the same as the component). |
| `edition`  | `alpine`         | The Mélodium edition to use (_not available when local distribution is chosen_). |
| `command`  | `run Compo.toml` | The Mélodium command to run. |
| `repository`  | `${{ github.repository }}` | Repository name with owner. For example, actions/checkout' |
| `ref`  | _default branch_ | The branch, tag or SHA to checkout. |
| `artifact-path`  | `''`| Path to artifact to upload. |


## Documentation

Take look at [Mélodium CI/CD Module Documentation](https://doc.melodium.tech/latest/en/cicd/index.html) and [Cadence.CI Documentation](https://cadence.ci/en/docs/from-gitlab) for more informations.

## Licence

This software is free and open-source, under the EUPL licence.

> Licensed under the EUPL, Version 1.2 or - as soon they will be approved by the European Commission - subsequent versions of the EUPL (the "Licence"); You may not use this work except in compliance with the Licence. You may obtain a copy of the Licence at: https://joinup.ec.europa.eu/software/page/eupl
>
> Unless required by applicable law or agreed to in writing, software distributed under the Licence is distributed on an "AS IS" basis, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.