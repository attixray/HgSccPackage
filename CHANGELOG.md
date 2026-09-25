# Version history

## 2.1.0

The package loads in the background. It was a synchronous package with a
synchronous auto-load, which Visual Studio no longer runs, and it started
`hg version` and `hg showconfig` on the UI thread while it loaded.

- The package is an `AsyncPackage` that allows background loading. Its
  auto-load on the provider's UI context runs in the background.
- It finds the mercurial version and extensions on a background thread. It
  still offers its service, adds its menu commands and registers with the
  source control manager on the main thread.
- Becoming the active provider reuses that version, instead of starting
  `hg version` again on the UI thread.

## 2.0.9

Opening a solution with many projects and solution folders took about a minute.
Every project and solution folder started `hg root`, and a failed repository
open was retried for each of them. Visual Studio reported HgSccPackage as
slowing down solution load.

- The repository root is found in-process, by looking for the nearest `.hg`
  directory, and remembered per directory until the solution closes. No `hg`
  process runs for it.
- Solution folders have no file on disk. They now use the solution's
  repository, instead of opening one of their own from Visual Studio's current
  directory.
- A repository that fails to open is not tried again until the solution closes.
- Checking whether a solution is inside a repository no longer starts a
  command server that was never closed.
- When Visual Studio runs in a job that does not allow break-away, `hg` is
  started inside that job instead of failing.
- A command server that exits early no longer makes the reader loop forever,
  or ask for a 4 GB buffer.

## 2.0.8

Built outside this repository; its source is not available here.

## 2.0.7

- Visual Studio 2022 support.
- Visual Studio 2026 support.

## Earlier versions

See `HgSccPackage/ReleaseNotes.rtf`, which goes up to 2.0.6.
