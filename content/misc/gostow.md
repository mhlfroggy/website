---
date: '2026-07-14T00:00:00-05:00'
draft: false
title: 'Gostow'
---
GNU Stow is a great utility, but it's biggest issue is that it was designed to
be general-purpose, meaning there's some finicky areas where it can mess with
your head. One great example is how folder-management is handled. I propose a
utility which applies a Suckless philosophy of "do one thing and do it well" for
this problem, written in Golang so it compiles as a single binary and not as a
Perl module like GNU Stow is.

The following commands are supported in Gostow:
- `dry-run` performs a dry run of the file stowing, showing what would occur
- `dir` changes the directory to pull from to VALUE, default is current directory
- `target` changes the target configuration directory to VALUE, default is .config
- `verbose` shows verbose output of files
- `remove` unlinks target files
- `replace` unlinks and relinks target files
- `ignore` ignores files which match a Perl regex
- `defer` doesn't link files which match a Perl Regex
- `override` force linking of files matching Perl regex even if they exist
- `migrate` generates the direcotry and migrates automatically

Currently the program must be built from source, but I am planning on release
package binaries for Nix, Homebrew, and Arch.

You can find the repository [here](https://github.com/mhlfroggy/gostow).
