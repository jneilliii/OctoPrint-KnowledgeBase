If you want that each repository should use the same labels (name, color, description) for issue-tracking, you can install an app in you profile. 

https://github.com/settings/installations/2162505

After installation and enabling which repository should be used (single or all), you can create a ```.github/settings.yml```

This file includes the following informations:

```yaml
repository:
  # See https://developer.github.com/v3/repos/#edit for all available settings.

  # The name of the repository. Changing this will rename the repository
#  name: repo-name

  # A short description of the repository that will show up on GitHub
#  description: description of repo

  # A URL with more information about the repository
#  homepage: https://example.github.io/

  # Either `true` to make the repository private, or `false` to make it public.
#  private: false

  # Either `true` to enable issues for this repository, `false` to disable them.
#  has_issues: true

  # Either `true` to enable the wiki for this repository, `false` to disable it.
  has_wiki: false

  # Either `true` to enable downloads for this repository, `false` to disable them.
#  has_downloads: true

  # Updates the default branch for this repository.
#  default_branch: master

  # Either `true` to allow squash-merging pull requests, or `false` to prevent
  # squash-merging.
#  allow_squash_merge: true

  # Either `true` to allow merging pull requests with a merge commit, or `false`
  # to prevent merging pull requests with merge commits.
#  allow_merge_commit: true

  # Either `true` to allow rebase-merging pull requests, or `false` to prevent
  # rebase-merging.
#  allow_rebase_merge: true

# Labels: define labels for Issues and Pull Requests
labels:
  - name: "type: bug"
    description: "Something isn't working"
    color: d73a4a
  - name: "type: enhancement"
    description: "Something isn't working"
    color: a2eeef
  - name: "type: question"
    description: "Further information is requested"
    color: d876e3
  - name: "status: inProgress"
    description: "I am working on it"
    color: 5319e7
  - name: "status: waitingForFeedback"
    description: "Wating for Customers feedback"
    color: bfdadc
  - name: "status: wontfix"
    description: "I don't wont to fix this"
    color: fbca04


# Collaborators: give specific users access to this repository.
#collaborators:
#  - username: bkeepers
    # Note: Only valid on organization-owned repositories.
    # The permission to grant the collaborator. Can be one of:
    # * `pull` - can pull, but not push to or administer this repository.
    # * `push` - can pull and push, but not administer this repository.
    # * `admin` - can pull, push and administer this repository.
#    permission: push

#  - username: hubot
#    permission: pull

#  - username:
#    permission: pull
```
