# Bearsampp Module Git - Gradle Build Documentation

## Table of Contents

- [Overview](#overview)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Build Tasks](#build-tasks)
- [Configuration](#configuration)
- [Architecture](#architecture)
- [Troubleshooting](#troubleshooting)
- [Migration Guide](#migration-guide)

---

## Overview

The Bearsampp Module Git project has been converted to a **pure Gradle build system**, replacing the legacy Ant build configuration. This provides:

- **Modern Build System**     - Native Gradle tasks and conventions
- **Better Performance**       - Incremental builds and caching
- **Simplified Maintenance**   - Pure Groovy/Gradle DSL
- **Enhanced Tooling**         - IDE integration and dependency management
- **Cross-Platform Support**   - Works on Windows, Linux, and macOS

### Project Information

| Property          | Value                                    |
|-------------------|------------------------------------------|
| **Project Name**  | module-git                               |
| **Group**         | com.bearsampp.modules                    |
| **Type**          | Git Module Builder                       |
| **Build Tool**    | Gradle 8.x+                              |
| **Language**      | Groovy (Gradle DSL)                      |

---

## Quick Start

### Prerequisites

| Requirement       | Version       | Purpose                                  |
|-------------------|---------------|------------------------------------------|
| **Java**          | 8+            | Required for Gradle execution            |
| **Gradle**        | 8.0+          | Build automation tool                    |
| **7-Zip**         | Latest        | Archive extraction and compression       |

### Basic Commands

```bash
# Display build information
gradle info

# List all available tasks
gradle tasks

# List available versions
gradle listVersions

# Build a release (interactive)
gradle release

# Build a specific version (non-interactive)
gradle release -PbundleVersion=2.51.2

# Clean build artifacts
gradle clean
```

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/bearsampp/module-git.git
cd module-git
```

### 2. Verify Environment

Check that you have the required tools:

```bash
# Check Java version
java -version

# Check Gradle version
gradle --version

# Check 7-Zip
7z
```

### 3. List Available Versions

```bash
gradle listVersions
```

### 4. Build Your First Release

```bash
# Interactive mode (prompts for version)
gradle release

# Or specify version directly
gradle release -PbundleVersion=2.51.2
```

---

## Build Tasks

### Core Build Tasks

| Task                  | Description                                      | Example                                  |
|-----------------------|--------------------------------------------------|------------------------------------------|
| `release`             | Build and package release (interactive/non-interactive) | `gradle release -PbundleVersion=2.51.2` |
| `clean`               | Clean build artifacts and temporary files        | `gradle clean`                           |

### Information Tasks

| Task                | Description                                      | Example                    |
|---------------------|--------------------------------------------------|----------------------------|
| `info`              | Display build configuration information          | `gradle info`              |
| `listVersions`      | List available bundle versions in bin/           | `gradle listVersions`      |

### Task Groups

| Group            | Purpose                                          |
|------------------|--------------------------------------------------|
| **build**        | Build and package tasks                          |
| **help**         | Help and information tasks                       |

---

## Configuration

### build.properties

The main configuration file for the build:

```properties
bundle.name     = git
bundle.release  = 2025.11.1
bundle.type     = tools
bundle.format   = 7z
```

| Property          | Description                          | Example Value  |
|-------------------|--------------------------------------|----------------|
| `bundle.name`     | Name of the bundle                   | `git`          |
| `bundle.release`  | Release version                      | `2025.11.1`    |
| `bundle.type`     | Type of bundle                       | `tools`        |
| `bundle.format`   | Archive format                       | `7z`           |

### releases.properties

Maps Git versions to download URLs:

```properties
2.51.2=https://github.com/Bearsampp/module-git/releases/download/2025.11.1/bearsampp-git-2.51.2-2025.11.1.7z
2.50.1=https://github.com/Bearsampp/module-git/releases/download/2025.7.10/bearsampp-git-2.50.1-2025.7.10.7z
```

If a version is not found in `releases.properties`, the build will check the remote untouched modules repository:
`https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/git.properties`

### gradle.properties

Gradle-specific configuration:

```properties
# Gradle daemon configuration
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.caching=true

# JVM settings
org.gradle.jvmargs=-Xmx2g -XX:MaxMetaspaceSize=512m
```

### Directory Structure

```
module-git/
├── .gradle-docs/          # Gradle documentation
│   └── README.md          # Main documentation
├── bin/                   # Git version configurations
│   ├── git2.50.1/
│   ├── git2.51.2/
│   └── archived/          # Archived versions
│       └── git2.34.0/
├── bearsampp-build/       # External build directory (outside repo)
│   ├── tmp/               # Temporary build files
│   │   ├── bundles_prep/tools/git/  # Prepared bundles
│   │   ├── downloads/git/           # Downloaded archives
│   │   └── extract/git/             # Extracted sources
│   └── tools/git/         # Final packaged archives
│       └── 2025.11.1/     # Release version
│           ├── bearsampp-git-2.51.2-2025.11.1.7z
│           ├── bearsampp-git-2.51.2-2025.11.1.7z.md5
│           └── ...
├── build.gradle           # Main Gradle build script
├── settings.gradle        # Gradle settings
├── build.properties       # Build configuration
└── releases.properties    # Available Git releases
```

---

## Architecture

### Build Process Flow

```
1. User runs: gradle release -PbundleVersion=2.51.2
                    ↓
2. Validate environment and version
                    ↓
3. Resolve download URL from releases.properties or remote
                    ↓
4. Download Git archive (or use cached version)
                    ↓
5. Extract archive to tmp/extract/
                    ↓
6. Create preparation directory (tmp/bundles_prep/)
                    ↓
7. Copy Git files (excluding doc/ directory)
                    ↓
8. Copy custom configuration from bin/git{version}/
   - bearsampp.conf (with @RELEASE_VERSION@ replacement)
   - repos.dat
   - Other custom files
                    ↓
9. Output prepared bundle to tmp/bundles_prep/
                    ↓
10. Package prepared folder into archive in bearsampp-build/tools/git/{bundle.release}/
    - The archive includes the top-level folder: git{version}/
```

### Packaging Details

- **Archive name format**: `bearsampp-git-{version}-{bundle.release}.{7z|zip}`
- **Location**: `bearsampp-build/tools/git/{bundle.release}/`
  - Example: `bearsampp-build/tools/git/2025.11.1/bearsampp-git-2.51.2-2025.11.1.7z`
- **Content root**: The top-level folder inside the archive is `git{version}/` (e.g., `git2.51.2/`)
- **Structure**: The archive contains the Git version folder at the root with all Git files inside

**Archive Structure Example**:
```
bearsampp-git-2.51.2-2025.11.1.7z
└── git2.51.2/              ← Version folder at root
    ├── bearsampp.conf
    ├── repos.dat
    ├── bin/
    │   ├── git.exe
    │   ├── git-bash.exe
    │   └── ...
    ├── cmd/
    ├── etc/
    ├── mingw64/
    └── ...
```

**Verification Commands**:

```bash
# List archive contents with 7z
7z l bearsampp-build/tools/git/2025.11.1/bearsampp-git-2.51.2-2025.11.1.7z | more

# You should see entries beginning with:
#   git2.51.2/bearsampp.conf
#   git2.51.2/bin/git.exe
#   git2.51.2/...

# Extract and inspect with PowerShell
7z x bearsampp-build/tools/git/2025.11.1/bearsampp-git-2.51.2-2025.11.1.7z -o_inspect
Get-ChildItem _inspect\git2.51.2 | Select-Object Name

# Expected output:
#   bearsampp.conf
#   repos.dat
#   bin/
#   cmd/
#   etc/
#   ...
```

**Note**: This archive structure matches other Bearsampp modules where archives contain `{module}{version}/` at the root. This ensures consistency across all Bearsampp modules.

**Hash Files**: Each archive is accompanied by hash sidecar files:
- `.md5` - MD5 checksum
- `.sha1` - SHA-1 checksum
- `.sha256` - SHA-256 checksum
- `.sha512` - SHA-512 checksum

Example:
```
bearsampp-build/tools/git/2025.11.1/
├── bearsampp-git-2.51.2-2025.11.1.7z
├── bearsampp-git-2.51.2-2025.11.1.7z.md5
├── bearsampp-git-2.51.2-2025.11.1.7z.sha1
├── bearsampp-git-2.51.2-2025.11.1.7z.sha256
└── bearsampp-git-2.51.2-2025.11.1.7z.sha512
```

### Version Management

Each Git version can have custom configuration files in `bin/git{version}/`:

- **bearsampp.conf** - Module configuration (with `@RELEASE_VERSION@` placeholder)
- **repos.dat** - Repository configuration
- Any other custom files needed for the specific version

---

## Troubleshooting

### Common Issues

#### Issue: "Version not found"

**Symptom:**
```
Version not found: 2.51.2
```

**Solution:**
1. List available versions: `gradle listVersions`
2. Ensure the version directory exists in `bin/` or `bin/archived/`
3. Check that the version is in `releases.properties` or the remote untouched modules

---

#### Issue: "7-Zip not found"

**Symptom:**
```
7-Zip not found. Please install 7-Zip or set 7Z_HOME environment variable.
```

**Solution:**
1. Install 7-Zip from https://www.7-zip.org/
2. Or set `7Z_HOME` environment variable:
   ```bash
   set 7Z_HOME=C:\Program Files\7-Zip
   ```

---

#### Issue: "Java version too old"

**Symptom:**
```
Java 8+ required
```

**Solution:**
1. Check Java version: `java -version`
2. Install Java 8 or higher from https://adoptium.net/
3. Update JAVA_HOME environment variable

---

#### Issue: "Download failed"

**Symptom:**
```
Failed to download from URL
```

**Solution:**
1. Check internet connection
2. Verify URL in `releases.properties`
3. Check if GitHub releases are accessible
4. Clear download cache: `gradle clean`

---

### Debug Mode

Run Gradle with debug output:

```bash
gradle release -PbundleVersion=2.51.2 --info
gradle release -PbundleVersion=2.51.2 --debug
gradle release -PbundleVersion=2.51.2 --stacktrace
```

### Clean Build

If you encounter issues, try a clean build:

```bash
gradle clean
gradle release -PbundleVersion=2.51.2
```

---

## Migration Guide

### From Ant to Gradle

The project has been fully migrated from Ant to Gradle. Here's what changed:

#### Removed Files

| File              | Status    | Replacement                |
|-------------------|-----------|----------------------------|
| `build.xml`       | ❌ Removed | `build.gradle`             |

#### Command Mapping

| Ant Command                          | Gradle Command                              |
|--------------------------------------|---------------------------------------------|
| `ant release`                        | `gradle release`                            |
| `ant release -Dinput.bundle=2.51.2`  | `gradle release -PbundleVersion=2.51.2`     |
| `ant clean`                          | `gradle clean`                              |

#### Key Differences

| Aspect              | Ant                          | Gradle                           |
|---------------------|------------------------------|----------------------------------|
| **Build File**      | XML (build.xml)              | Groovy DSL (build.gradle)        |
| **Task Definition** | `<target name="...">`        | `tasks.register('...')`          |
| **Properties**      | `<property name="..." />`    | `ext { ... }`                    |
| **Dependencies**    | Manual downloads             | Automatic with caching           |
| **Caching**         | None                         | Built-in incremental builds      |
| **IDE Support**     | Limited                      | Excellent (IntelliJ, Eclipse)    |

---

## Additional Resources

- [Gradle Documentation](https://docs.gradle.org/)
- [Bearsampp Project](https://github.com/bearsampp/bearsampp)
- [Git for Windows](https://gitforwindows.org/)

---

## Support

For issues and questions:

- **GitHub Issues**: https://github.com/bearsampp/module-git/issues
- **Bearsampp Issues**: https://github.com/bearsampp/bearsampp/issues
- **Documentation**: https://bearsampp.com/module/git

---

**Last Updated**: 2025-01-31  
**Version**: 2025.11.1  
**Build System**: Pure Gradle (no wrapper, no Ant)

Notes:
- This project deliberately does not ship the Gradle Wrapper. Install Gradle 8+ locally and run with `gradle ...`.
- Legacy Ant files (e.g., Eclipse `.launch` referencing `build.xml`) are deprecated and not used by the build.
