# plasmactl-package

A [Launchr](https://github.com/launchrctl/launchr) plugin for [Plasmactl](https://github.com/plasmash/plasmactl) that creates distributable tar.gz archives of platform code.

## Overview

`plasmactl-package` packages your Plasma platform or package code into compressed tar.gz archives for distribution and deployment. It automatically extracts repository information from git and creates properly structured artifacts.

## Features

- **Automatic Archiving**: Creates tar.gz archives with git metadata
- **Smart Filtering**: Excludes build artifacts and temporary directories
- **Repository Metadata**: Includes commit SHA and repository name in filenames
- **Symlink Preservation**: Maintains symbolic links in archives

## Usage

```bash
plasmactl package
```

This creates a tar.gz archive in `.compose/artifacts/` with the naming pattern:
```
{repo-name}-{commit-sha}-plasma-src.tar.gz
```

Example output:
```
pla-plasma-abc123d-plasma-src.tar.gz
```

## Workflow

Typically used as part of the build and publish pipeline:

```bash
# 1. Make changes and commit
git add -A
git commit -m "feat: add new component"

# 2. Bump component versions
plasmactl bump

# 3. Package the code
plasmactl package

# 4. Publish to artifact repository
plasmactl publish
```

## Archive Contents

The package includes:
- All source code
- Platform components
- Configuration files
- Ansible playbooks and roles
- Documentation

### Excluded Directories

The following directories are automatically excluded:
- `.git/` - Git repository data
- `.compose/` - Temporary composition files
- `.plasmactl/` - CLI working directory

## Archive Structure

```
archive.tar.gz
├── platform/
│   ├── applications/
│   ├── services/
│   └── ...
├── library/
├── group_vars/
├── plasma-compose.yaml
└── ...
```

## Artifact Location

Archives are created in:
```
.compose/artifacts/{repo-name}-{commit-sha}-plasma-src.tar.gz
```

## Repository Information

The command extracts the following from git:
- **Repository Name**: From the git remote origin URL
- **Commit SHA**: Short hash of the current HEAD (7 characters)
- **Commit Message**: Full message from the latest commit

## Integration

### With plasmactl-publish

```bash
plasmactl package  # Create archive
plasmactl publish  # Upload to artifact repository
```

### With plasmactl-ship

```bash
plasmactl package  # Create archive
plasmactl publish  # Upload archive
plasmactl ship dev platform.interaction.observability  # Deploy
```

## Best Practices

1. **Commit First**: Always commit changes before packaging
2. **Clean State**: Ensure no uncommitted changes that should be included
3. **Version Bump**: Run `plasmactl bump` before packaging to update versions
4. **Verify Archive**: Check `.compose/artifacts/` for the created file

## Documentation

- [Plasmactl](https://github.com/plasmash/plasmactl) - Main CLI tool
- [Plasma Platform](https://plasma.sh) - Platform documentation

## License

Apache License 2.0
