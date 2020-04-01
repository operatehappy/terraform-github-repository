# Terraform Module: GitHub Repository

> This repository is a [Terraform](https://terraform.io/) Module for managing GitHub Repository and Branch Protection resources.

## Table of Contents

- [Requirements](#requirements)
- [Dependencies](#dependencies)
- [Usage](#usage)
  - [Module Variables](#module-variables)
- [Author Information](#author-information)
- [License](#license)

## Requirements

This module requires Terraform version `0.10.x` or newer.

## Dependencies

This module depends on a correctly configured [GitHub Provider](https://www.terraform.io/docs/providers/github/index.html) in your Terraform codebase.

## Usage

Add the module to your Terraform resources like so:

```hcl
module "foo-cli" {
  source                                    = "github.com/withmethod/terraform-module-github-repository?ref=0.3.1"
  name                                      = "foo-cli"
  description                               = "foo CLI"
  homepage_url                              = "https://withmethod.com/open-source/"
  private                                   = true
  has_issues                                = true
  has_wiki                                  = false
  allow_merge_commit                        = false
  allow_squash_merge                        = true
  allow_rebase_merge                        = false
  has_downloads                             = false
  auto_init                                 = true
  gitignore_template                        = "Terraform"
  license_template                          = "apache-2.0"
  enable_team_repository                    = 1
  team_repository_team                      = "${github_team.internal.id}"
  team_repository_permission                = "pull"
  branch                                    = "master"
  enable_branch_protection                  = 1 // only works after initial creation of repository
  enforce_admins                            = true
  req_status_checks_strict                  = false
  req_status_checks_context                 = ["continuous-integration/travis-ci"]
  req_pr_reviews_dismiss_stale_reviews      = true
  req_pr_reviews_require_code_owner_reviews = false
  req_pr_reviews_dismissal_users            = []
  req_pr_reviews_dismissal_teams            = []
  restrictions_users                        = []
  restrictions_teams                        = []
}
```

Then, load the module using `terraform get`.

### Module Variables

Available variables are listed below, along with their default values:

| Name                                 | Description                                                                               |  Type  |    Default     | Required |
|:-------------------------------------|:------------------------------------------------------------------------------------------|:------:|:--------------:|:--------:|
| name                                 | The name of the repository                                                                | string |      n/a       |   yes    |
| allow_merge_commit                   | Set to `false` to disable merge commits on the repository                                 | string |    `"true"`    |    no    |
| allow_rebase_merge                   | Set to `false` to disable rebase merges on the repository                                 | string |    `"true"`    |    no    |
| allow_squash_merge                   | Set to `false` to disable squash merges on the repository                                 | string |    `"true"`    |    no    |
| archived                             | Specifies if the repository should be archived                                            | string |   `"false"`    |    no    |
| auto_init                            | Set to `true` to produce an initial commit in the repository                              | string |    `"true"`    |    no    |
| branch                               | The name of the default branch of the repository                                          | string |   `"master"`   |    no    |
| description                          | A description of the repository                                                           | string |      `""`      |    no    |
| enable_branch_protection             | Boolean to toggle branch protection settings. Only works when repository has been created | string |     `"0"`      |    no    |
| enable_team_repository               | Boolean to toggle team repository settings                                                | string |     `"0"`      |    no    |
| enforce_admins                       | Boolean to toggle enforcement of status checks for administrators                         | string |    `"true"`    |    no    |
| gitignore_template                   | Set to a template to use for the `.gitignore` file                                        | string |      `""`      |    no    |
| has_downloads                        | Set to `true` to enable the (deprecated) downloads features on the repository             | string |   `"false"`    |    no    |
| has_issues                           | Set to `true` to enable the Github Issues features on the repository                      | string |    `"true"`    |    no    |
| has_projects                         | Set to true to enable the GitHub Projects features on the repository                      | string |   `"false"`    |    no    |
| has_wiki                             | Set to `true` to enable the Github Wiki features on the repository                        | string |    `"true"`    |    no    |
| homepage_url                         | URL of a page describing the project                                                      | string |      `""`      |    no    |
| license_template                     | Set to a template to use for the license                                                  | string | `"apache-2.0"` |    no    |
| private                              | Set to `true` to create a private repository                                              | string |    `"true"`    |    no    |
| req_pr_reviews_dismiss_stale_reviews | Boolean to toggle dismissal of reviews when a new commit is pushed                        | string |    `"true"`    |    no    |
| req_pr_reviews_dismissal_teams       | The list of team slugs with dismissal access                                              |  list  |    `<list>`    |    no    |
| req_pr_reviews_dismissal_users       | The list of user logins with dismissal access                                             |  list  |    `<list>`    |    no    |
| req_status_checks_context            | List of status checks to require in order to merge into this branch                       |  list  |    `<list>`    |    no    |
| req_status_checks_strict             | Boolean to toggle strictness of status checks                                             | string |   `"false"`    |    no    |
| restrictions_teams                   | The list of team slugs with push access                                                   |  list  |    `<list>`    |    no    |
| restrictions_users                   | The list of user logins with push access                                                  |  list  |    `<list>`    |    no    |
| team_repository_permission           | The permissions of team members regarding the repository                                  | string |    `"pull"`    |    no    |
| team_repository_team                 | The GitHub team ID                                                                        | string |      `""`      |    no    |
| topics                               | The list of topics of the repository                                                      |  list  |    `<list>`    |    no    |

- `private` defaults to `true`
- `has_downloads` defaults to `false`
- `license_template` defaults to `apache-2.0`
- `branch` defaults to `master`
- `enforce_admins` defaults to `true`
- `req_pr_reviews_dismiss_stale_reviews` defaults to `true`

Please note: `auto_init`, `gitignore_template` as well as `license_template` are actions that will result in commits being made to the GitHub Repository. These commits will be attributed to the user that is linked to the token that is used for the GitHub provider.

### Module outputs

Available outputs are listed below, along with their description

| Name           | Description                                                                                                        |
|:---------------|:-------------------------------------------------------------------------------------------------------------------|
| full_name      | A string of the form `orgname/reponame`.                                                                           |
| git_clone_url  | URL that can be provided to git clone to clone the repository anonymously via the git protocol.                    |
| http_clone_url | URL that can be provided to `git clone` to clone the repository via HTTPS.                                         |
| ssh_clone_url  | URL that can be provided to `git clone` to clone the repository via SSH.                                           |
| svn_url        | URL that can be provided to `svn checkout` to check out the repository via GitHub's Subversion protocol emulation. |

## Author Information

This module is currently maintained by the individuals listed below.

- [Kerim Satirli](https://github.com/ksatirli)

Development of this module was sponsored by [Method](https://github.com/withmethod).

## License

Copyright 2017 [Kerim Satirli](https://github.com/ksatirli)

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License.

You may obtain a copy of the License at [apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an _"AS IS"_ basis, without WARRANTIES or conditions of any kind, either express or implied.

See the License for the specific language governing permissions and limitations under the License.
