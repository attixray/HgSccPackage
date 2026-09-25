# Description

HgSccPackage is a Mercurial Source Control Package for MS Visual Studio 2017-2026.
It adds an integration and UI to work with mercurial to a MS Visual Studio.

This is archived version of original repository.

The original repository was hosted on BitBucket, but was deleted by Atlassian.

## How to build

Prerequisites:

- The MS Visual Studio 2026
- The MS Visual Studio 2026 SDK

## How to use

1. Install HgSccPackage.vsix
2. Open the MS Visual Studio
3. Select Mercurial Source Control Provider in Tools -> Options -> Source Control

The mercurial command line client (hg.exe) must be installed to use a HgSccPackage.

## Builds

The *Build VSIX* workflow builds two packages from every push:

- `HgSccPackage.vsix`: the Release build.
- `HgSccPackage-logging.vsix`: the Debug build, which logs every step to
  `%LOCALAPPDATA%\HgSccPackage\hgsccpkg.log`. Use it only to diagnose a problem:
  logging slows Visual Studio down.

To publish a release, raise the `Identity` version in
`HgSccPackage/source.extension.vsixmanifest` (and the other version strings),
merge to `main`, then push the tag `v<version>` or run the workflow with
`release-tag` set to `v<version>`. The tag may drop the manifest version's
trailing `.0`: `v1.2.3` for `1.2.3.0`.
