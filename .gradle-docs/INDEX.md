# Bearsampp Module Git - Documentation Index

## Overview

This directory contains complete documentation for the Gradle-based build system for the Bearsampp Git module. The project has been fully converted from Ant to Gradle, providing a modern, maintainable build system.

## Documentation Structure

### Quick Start
- **[QUICKSTART.md](QUICKSTART.md)** - Get started in 5 minutes
  - Prerequisites check
  - First build walkthrough
  - Common commands
  - Troubleshooting basics

### Complete Guides
- **[README.md](README.md)** - Complete build documentation
  - Project structure
  - Configuration files
  - Build process details
  - Version management
  - Advanced usage

- **[TASKS.md](TASKS.md)** - Task reference card
  - All available Gradle tasks
  - Task options and combinations
  - Common workflows
  - Quick reference table

- **[API.md](API.md)** - API reference
  - Build script functions
  - Project properties
  - Custom task examples
  - Error handling

### Migration & Comparison
- **[MIGRATION.md](MIGRATION.md)** - Ant to Gradle migration guide
  - Migration steps
  - Key differences
  - Task mapping

- **[COMPARISON.md](COMPARISON.md)** - Ant vs Gradle comparison
  - Feature comparison
  - Performance benefits
  - Advantages of Gradle

## Available Tasks

| Task           | Purpose                    | Example                                |
|----------------|----------------------------|----------------------------------------|
| `release`      | Build a release bundle     | `gradle release -PbundleVersion=X.X.X` |
| `listVersions` | List available versions    | `gradle listVersions`                  |
| `info`         | Show build configuration   | `gradle info`                          |
| `clean`        | Clean build artifacts      | `gradle clean`                         |
| `tasks`        | Show all available tasks   | `gradle tasks`                         |

## Quick Reference

### Build Commands

```bash
# Show build information
gradle info

# List available versions
gradle listVersions

# Build a specific version (interactive)
gradle release

# Build a specific version (non-interactive)
gradle release -PbundleVersion=2.51.2

# Clean build artifacts
gradle clean
```

### Configuration Files

| File                  | Purpose                           |
|-----------------------|-----------------------------------|
| `build.gradle`        | Main build script                 |
| `build.properties`    | Bundle configuration              |
| `releases.properties` | Version download URLs             |
| `gradle.properties`   | Gradle configuration (optional)   |
| `settings.gradle.kts` | Gradle settings                   |

### Output Structure

```
bearsampp-build/
├── tmp/
│   ├── downloads/git/    # Downloaded archives
│   ├── extract/git/      # Extracted sources
│   └── bundles_prep/     # Prepared bundles
└── tools/git/2025.11.1/  # Final releases
    ├── bearsampp-git-2.51.2-2025.11.1.7z
    ├── bearsampp-git-2.51.2-2025.11.1.7z.md5
    ├── bearsampp-git-2.51.2-2025.11.1.7z.sha1
    ├── bearsampp-git-2.51.2-2025.11.1.7z.sha256
    └── bearsampp-git-2.51.2-2025.11.1.7z.sha512
```

## Prerequisites

- **Java**: 8 or higher
- **Gradle**: 6.0 or higher (wrapper included)
- **7-Zip**: For .7z compression (optional for .zip)

## Getting Help

### Documentation
- Start with [QUICKSTART.md](QUICKSTART.md) for immediate setup
- Read [README.md](README.md) for comprehensive documentation
- Check [TASKS.md](TASKS.md) for task reference
- Explore [API.md](API.md) for advanced customization

### Command Line Help
```bash
# Show all available tasks
gradle tasks

# Show build information
gradle info

# Show task details
gradle help --task release
```

### Common Issues

| Issue                  | Solution                                    |
|------------------------|---------------------------------------------|
| 7-Zip not found        | Install 7-Zip or set `7Z_HOME` env variable |
| Version not found      | Add to `releases.properties`                |
| Java not found         | Install Java and set `JAVA_HOME`            |
| Build fails            | Run with `--stacktrace` for details         |

## Project Status

✅ **Conversion Complete**: Ant build system fully replaced with Gradle
✅ **Documentation Updated**: All docs aligned with Gradle tasks
✅ **Ant Files Removed**: build.xml and related files removed
✅ **Pure Gradle Build**: No Ant dependencies

## Key Features

### Build System
- ✅ Pure Gradle build (no Ant dependencies)
- ✅ Interactive version selection
- ✅ Automatic download and extraction
- ✅ Hash file generation (MD5, SHA1, SHA256, SHA512)
- ✅ Shared build directory structure
- ✅ Remote version fallback

### Documentation
- ✅ Comprehensive guides
- ✅ Quick start guide
- ✅ Task reference card
- ✅ API documentation
- ✅ Migration guide
- ✅ Aligned tables and formatting

### Developer Experience
- ✅ Clear error messages
- ✅ Progress indicators
- ✅ Verbose logging options
- ✅ IDE integration support
- ✅ CI/CD ready

## Version History

| Version | Date       | Changes                              |
|---------|------------|--------------------------------------|
| 1.0.0   | 2025-01-XX | Initial Gradle conversion complete   |
|         |            | - Removed Ant build system           |
|         |            | - Updated all documentation          |
|         |            | - Aligned tables and task names      |

## Contributing

When contributing to the build system:

1. **Test Changes**: Always test build changes locally
2. **Update Docs**: Keep documentation in sync with code
3. **Follow Conventions**: Use existing patterns and naming
4. **Add Examples**: Include usage examples for new features

## Support

- **Issues**: https://github.com/Bearsampp/module-git/issues
- **Documentation**: https://github.com/Bearsampp/module-git/tree/main/.gradle-docs
- **Gradle Docs**: https://docs.gradle.org

## License

This project is part of the Bearsampp project. See LICENSE file for details.

---

**Last Updated**: 2025-01-XX  
**Version**: 1.0.0  
**Status**: ✅ Production Ready
