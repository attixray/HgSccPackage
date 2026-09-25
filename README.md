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

## Changes in 2.0.9

Opening a solution with many projects and solution folders took about a minute,
because every project and solution folder started `hg root`, and a failed
repository open was retried for each of them. Visual Studio reported
HgSccPackage as slowing down solution load.

- The repository root is found in-process, by looking for the nearest `.hg`
  directory, and remembered per directory until the solution closes. No `hg`
  process runs for it.
- Solution folders, which have no file on disk, use the solution's repository
  instead of opening one of their own from Visual Studio's current directory.
- A repository that fails to open is not tried again until the solution closes.
- Checking whether a solution is inside a repository no longer starts a
  command server that was never closed.
- When Visual Studio runs in a job that does not allow break-away, `hg` is
  started inside that job instead of failing.
- A command server that exits early no longer makes the reader loop forever
  or ask for a 4 GB buffer.

## Builds

The *Build VSIX* workflow builds two packages from every push:

- `HgSccPackage.vsix`: the Release build.
- `HgSccPackage-logging.vsix`: the Debug build, which logs every step to
  `%LOCALAPPDATA%\HgSccPackage\hgsccpkg.log`. Use it only to diagnose a problem:
  logging slows Visual Studio down.

To publish a release, raise the `Identity` version in
`HgSccPackage/source.extension.vsixmanifest` (and the other version strings),
merge to `main`, then push the tag `v<version>` or run the workflow with
`release-tag` set to `v<version>`.
