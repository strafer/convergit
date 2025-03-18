🌍 Available languages: [🇺🇸 English](README.md) | [🇷🇺 Русский](README.ru.md)

# convergit

`convergit` is a lightweight, POSIX‑compliant shell utility that automatically generates semantic version tags — and optionally creates branches — based on Conventional Commits in your Git repository. Designed to work flawlessly even in busybox sh containers (e.g. Alpine Linux), `convergit` helps you maintain a clear and consistent version history.

## Features

- Semantic Versioning: Automatically generate version tags following the X.Y.Z format.
- Automatic Bump: Increment patch, minor, or major versions based on commit message keywords.
- Branch Creation: Optionally create new branches when a major or minor version change is detected.
- Customizable Tags: Use tag prefixes (e.g. v), custom version delimiters, and annotated tags with custom author/email details.
- Hooks Support: Execute custom commands before and after pushing tags.
- POSIX Compliant: Fully compatible with minimal shell environments, including busybox sh.

# Installation

Clone the repository and ensure the script is executable:

```shell
git clone https://github.com/strafer/convergit.git
cd convergit
chmod +x convergit/convergit
```

For easier use, you can add it to your PATH:

```shell
sudo cp convergit/convergit /usr/local/bin/convergit
```

# Usage

`convergit` scans your Git commit history for messages following the conventional commit format and calculates the next semantic version accordingly. Run it from the root of your Git repository.

## Basic Usage

Preview the tags and branches that would be generated (without modifying the repository):

```shell
convergit
```

Create the calculated tags and branches locally:

```shell
convergit --create
```

Create and push tags to your remote repository (default remote is origin):

```shell
convergit --create --push
```

# Options

Some key options include:

- Tag and Branch Generation
  - `--recent-tag` – Generate only the most recent tag, skipping intermediate ones.
  - `-T`, `--tag-prefix PREFIX` – Prepend a prefix (e.g. v) to the version (default: no prefix).
  - `--minor-tag` – Generate a minor version tag (X.Y) alongside the full tag.
  - `--major-tag` – Generate a major version tag (X) alongside the full tag.
  - `--respect-pre-release` – Consider pre-release tags (e.g., X.Y.Z-alpha.1) when calculating the next version.

- Annotated Tags
  - `-A`, `--annotated-tags` – Create annotated tags instead of lightweight ones.
  - `--annotated-author NAME` – Specify the tag author name (requires --annotated-email).
  - `--annotated-email EMAIL` – Specify the tag author email (requires --annotated-author).
  - `--annotated-message MSG` – Customize the message for annotated tags.

- Version Bump Controls
  - `--deny-inc-major` – Exit with an error when encountering breaking changes.
  - `--inc-major-head-branch` – Allow major version increases only in the HEAD branch.
  - `--inc-major-branch BRANCH` – Allow major version increases only in the specified branch.
  - `--current-branch BRANCH` – Override the detected current branch.

- Repository and Push Options
  - `-C`, `--cwd DIR` – Specify the repository root directory (default: current directory).
  - `-c`, `--create` – Create the calculated tags and branches.
  - `-p`, `--push` – Create and push the tags/branches (implies --create).
  - `--git-remote-name NAME` – Specify the remote name (default: origin).
  - `--git-remote-url URL` – Set the remote URL if not already defined.
  - `--push-pre-hook "command"` – Run a command before pushing each tag (substitute %t with tag name).
  - `--push-post-hook "command"` – Run a command after pushing each tag.

# Examples

## Create Annotated Tags with Custom Author Details

Generate annotated tags by specifying a custom author and message:

```shell
convergit --create --annotated-tags \
  --annotated-author "John Doe" \
  --annotated-email "john@example.com" \
  --annotated-message "Auto-generated tag based on conventional commits"
```

## Push Tags with Pre and Post Push Hooks

To automatically push tags and run custom commands before and after pushing:

```shell
convergit --create --push \
  --push-pre-hook "echo 'About to push tag %t...'" \
  --push-post-hook "echo 'Tag %t pushed successfully!'"
```

# Checking GNU getopt Version

For maximum compatibility, `convergit` requires GNU getopt. A version check is performed automatically when the script starts. If GNU getopt is not found, the script exits with an error message. You can also run the following manually:

```shell
getopt --version
```

A typical GNU getopt output starts with something like:

```shell
GNU getopt 2.33.1
```

# Contributing

Contributions, suggestions, and bug reports are highly welcome. Please fork the repository, make your changes, and submit a pull request. For any issues or feature requests, please open an issue on GitHub.

# License

This project is licensed under the GPL-3.0 License.
