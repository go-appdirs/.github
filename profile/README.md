<p align="center"><img src="https://raw.githubusercontent.com/go-appdirs/brand/main/social/go-appdirs.png" alt="go-appdirs" width="640"></p>

<h1 align="center">go-appdirs</h1>
<p align="center">Where a file is allowed to go. Durable per-application directories in pure Go, with the refusals that matter — <strong>no cgo</strong>, no dependencies.</p>
<p align="center">
  <img src="https://img.shields.io/badge/Go-1.26-00ADD8?style=flat-square&logo=go&logoColor=white">
  <img src="https://img.shields.io/badge/license-BSD--3--Clause-0A6E96?style=flat-square">
</p>

## Repositories

| Repo | What it is |
| --- | --- |
| [`outdir`](https://github.com/go-appdirs/outdir) | Choose a **durable** directory for a file that must never be committed — a screen capture, a camera frame, a log of somebody's data — and **refuse** any path inside a git work tree |

## Why this exists

A program that writes an artefact has to answer two questions, and both
are easy to get wrong in the same direction.

**Where does it survive?** `t.TempDir()` is removed when the test ends,
so the picture is gone before anybody can open it. The artefact exists
*so that a person can look at it*.

**Where can it not be published?** A capture of a real display is a
picture of somebody at work. A `.gitignore` entry is a safety net, not a
barrier: `git add -f`, a fresh clone, or any tool that does not consult
it publishes the file anyway. The barrier is to write somewhere that is
not in a work tree at all, and to **refuse** when the chosen place is.

That check is subtler than it looks. It has to walk to the filesystem
root, because `testdata/` is three levels below the `.git` that would
publish it. A `.git` **file** counts as well as a directory — that is
what a worktree and a submodule leave behind. And it has to resolve
symbolic links: a directory reached through one is still in the tree it
points into, and four separate implementations of this check got that
last part wrong.

Which is why it is one package now, and not four.
