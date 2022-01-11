# workflows

Reusable GitHub workflows that can be called from other workflows across our organization.
See the [GitHub documentation on reusing workflows](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows) for more information.

## Catalog

### `ecr-build`

A workflow to build and push Docker images to an ECR repository in our central AWS account.
The workflow takes care of provisioning the ECR repository.
The resulting image is tagged and labeled using the [docker/metadata-action](https://github.com/docker/metadata-action) action.
This tags the image with the branch name and generates a set of standard labels including the build time, commit SHA and source repository.

This workflow has optional inputs, see the [ecr-build](.github/workflows/ecr-build.yaml) workflow for details.

#### Examples

Calling the `ecr-build` reusable workflow from the `ci` workflow in another repository:

```yaml
name: ci

on: push

jobs:
  build:
    uses: 3DHubs/workflows/.github/workflows/ecr-build.yaml@main
```

Specifying a build target using the `target` workflow input:

```yaml
name: ci

on: push

jobs:
  build:
    uses: 3DHubs/workflows/.github/workflows/ecr-build.yaml@main
    with:
      target: test
```

### `tf-lint`

Runs `terraform fmt` and `terraform validate` against all Terraform files in a repository.
By default, the workflow will commit the results of `terraform fmt`.

This workflow has optional inputs, see the [tf-lint](.github/workflows/tf-lint.yaml) workflow for details.

#### Examples

Specify the Terraform version and only run `terraform fmt` without committing the result.

```yaml
name: ci

on: push

jobs:
  tf-lint:
    uses: 3DHubs/workflows/.github/workflows/tf-lint.yaml@main
    with:
      terraform_version: "1.0"
      commit_fmt: false
```
