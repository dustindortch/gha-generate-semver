# gha-generate-semver

This action generates a Semantic Versioning compliant version number based on the commit messages since the last tag.  It defaults to v0.0.0 if not tags are found.  It returns the previous (previous-version) and new version (new-version) numbers as output.

The generated version can be used to tag a new release or create a release branch.
