# gha-generate-semver

This action generates a [Semantic Versioning](https://semver.org) compliant version number based on the commit messages since the last tag.  It defaults to v0.0.0 if not tags are found as the previous version.  It returns the previous (previous-version) and new version (new-version) numbers as output.

It revises versions with the following contexts (commit messages containing, case-insensitive):

* major - [breaking, major]
* minor - [enhancement, feature, minor]
* patch - [bugfix, fix, hotfix, patch]

This is designed to work with [dustindortch/gha-cherry-pick-release-branch@v1](/dustindortch/gha-cherry-pick-release-branch)

## Inputs

* prefix
  * description: 'Version tag prefix (default: v)'
  * required: false

## Outputs

* new-version
    description: Semantic Version number based on commit history.
* previous-version
  * description: Previous Semantic Version number based on commit history.

## Usage

```yaml
    steps:
      - uses: actions/checkout@v4

      - name: Generate next version
        id: next-version
        uses: dustindortch/gha-generate-semver@v1

      - name: Tag release
        id: tag-release
        run: |
          release=${{ steps.next-version.outputs.new-version }}
          username="$(git --no-pager log --format=format:'%an' -n 1)"
          email=$(git --no-pager log --format=format:'%ae' -n 1)
          git config --global user.name "${username}"
          git config --global user.email "${email}"
          git tag ${release} -m "Release ${release}"
          git push --tags

      - name: Cherry-pick release
        id: cherry-pick-release
        uses: dustindortch/gha-cherry-pick-release-branch@v1
        with:
          tag: ${{ steps.next-version.outputs.new-version }}
```
