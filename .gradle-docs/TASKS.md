# Gradle Tasks Reference Card

Quick reference for all available Gradle tasks in module-git.

## Task Groups

### 🏗️ Build Tasks

#### release
**Build a release bundle**

```bash
# Build with interactive version selection
gradle release

# Build specific version (non-interactive)
gradle release -PbundleVersion=2.51.2
```

**What it does:**
1. Downloads source from untouched modules
2. Extracts archive
3. Copies files (excluding doc/)
4. Overlays bundle configuration
5. Replaces version placeholders
6. Creates 7z/zip archive
7. Generates hash files (MD5, SHA1, SHA256, SHA512)

**Output:** `bearsampp-build/tools/git/{release}/bearsampp-git-{version}-{release}.{format}`

---

#### clean
**Remove build artifacts**

```bash
gradle clean
```

**What it does:**
- Deletes build/ directory
- Removes all temporary files
- Clears prepared bundles

---

### 📋 Information Tasks

#### info
**Show build configuration**

```bash
gradle info
```

**Output:**
```
================================================================
          Bearsampp Module Git - Build Info
================================================================

Project:        module-git
Version:        2025.11.1
Description:    Bearsampp Module - git

Bundle Properties:
  Name:         git
  Release:      2025.11.1
  Type:         tools
  Format:       7z

Quick Start:
  gradle tasks                          - List all available tasks
  gradle info                           - Show this information
  gradle listVersions                   - List available versions
  gradle release -PbundleVersion=2.51.2 - Build release for version
  gradle clean                          - Clean build artifacts
```

---

#### listVersions
**List all available versions**

```bash
gradle listVersions
```

**Output:**
```
=== Available Git Versions ===

From releases.properties:
  - 2.34.0: https://github.com/...
  - 2.51.2: https://github.com/...

From untouched modules:
  - 2.52.0: https://github.com/...

Local bundle versions in bin/:
  - 2.50.1
  - 2.51.2
```

---

#### tasks
**Show all available tasks**

```bash
# Show all tasks
gradle tasks

# Show all tasks including hidden
gradle tasks --all
```

---

## Task Options

### Common Options

#### -P (Project Property)
Pass properties to the build:

```bash
gradle release -PbundleVersion=2.51.2
```

#### --info
Show info-level logging:

```bash
gradle release --info
```

#### --debug
Show debug-level logging:

```bash
gradle release --debug
```

#### --stacktrace
Show stack traces on errors:

```bash
gradle release --stacktrace
```

#### --offline
Use cached dependencies only:

```bash
gradle release --offline
```

#### --refresh-dependencies
Force refresh of dependencies:

```bash
gradle release --refresh-dependencies
```

#### --dry-run
Show what would be executed:

```bash
gradle release --dry-run
```

---

## Task Combinations

### Clean and Build

```bash
gradle clean release
```

### Build with Debug

```bash
gradle release --info --stacktrace
```

---

## Quick Reference

| Task           | Purpose          | Example                                |
|----------------|------------------|----------------------------------------|
| `release`      | Build version    | `gradle release -PbundleVersion=X.X.X` |
| `listVersions` | List versions    | `gradle listVersions`                  |
| `info`         | Show config      | `gradle info`                          |
| `clean`        | Clean build      | `gradle clean`                         |
| `tasks`        | Show all tasks   | `gradle tasks`                         |

---

## Common Workflows

### Development

```bash
# 1. Check configuration
gradle info

# 2. List versions
gradle listVersions

# 3. Build
gradle release -PbundleVersion=2.51.2
```

### Release

```bash
# 1. Clean
gradle clean

# 2. Build specific version
gradle release -PbundleVersion=2.51.2

# 3. Verify outputs
ls -lh ../bearsampp-build/tools/git/2025.11.1/
```

### Debugging

```bash
# 1. Clean build
gradle clean

# 2. Build with debug
gradle release --debug --stacktrace -PbundleVersion=2.51.2

# 3. Check info
gradle info --info
```

---

## Environment Variables

### JAVA_HOME
Java installation directory:

```bash
# Windows
set JAVA_HOME=C:\Program Files\Java\jdk-17

# Linux/Mac
export JAVA_HOME=/usr/lib/jvm/java-17
```

### GRADLE_OPTS
JVM options for Gradle:

```bash
# Windows
set GRADLE_OPTS=-Xmx2g -XX:MaxMetaspaceSize=512m

# Linux/Mac
export GRADLE_OPTS="-Xmx2g -XX:MaxMetaspaceSize=512m"
```

### GRADLE_USER_HOME
Gradle home directory:

```bash
# Windows
set GRADLE_USER_HOME=C:\Users\username\.gradle

# Linux/Mac
export GRADLE_USER_HOME=~/.gradle
```

---

## Configuration Files

### gradle.properties
Gradle configuration:

```properties
org.gradle.jvmargs=-Xmx2g
org.gradle.parallel=true
org.gradle.caching=true
```

### build.properties
Bundle configuration:

```properties
bundle.name = git
bundle.release = 2025.11.1
bundle.type = tools
bundle.format = 7z
```

### releases.properties
Version mappings:

```properties
2.51.2 = https://github.com/Bearsampp/module-git/releases/download/...
```

---

## Output Locations

### Build Directory
```
bearsampp-build/
├── tmp/
│   ├── downloads/git/    # Downloaded archives
│   ├── extract/git/      # Extracted sources
│   │   └── 2.51.2/
│   └── bundles_prep/     # Prepared bundles
│       └── tools/git/
│           └── git2.51.2/
└── tools/git/2025.11.1/  # Final releases
    ├── bearsampp-git-2.51.2-2025.11.1.7z
    ├── bearsampp-git-2.51.2-2025.11.1.7z.md5
    ├── bearsampp-git-2.51.2-2025.11.1.7z.sha1
    ├── bearsampp-git-2.51.2-2025.11.1.7z.sha256
    └── bearsampp-git-2.51.2-2025.11.1.7z.sha512
```

### Cache Directory
```
~/.gradle/
├── caches/              # Build cache
├── wrapper/             # Gradle wrapper
└── daemon/              # Gradle daemon
```

---

## Error Handling

### Common Errors

#### Version Not Found
```
Version X.X.X not found in releases.properties or untouched versions
```
**Solution:** Add version to releases.properties or check internet connection

#### git.exe Not Found
```
git.exe not found in .../bin/
```
**Solution:** Verify download URL and archive structure

#### 7z Not Found
```
Cannot run program "7z"
```
**Solution:** Install 7-Zip and add to PATH

#### Java Not Found
```
ERROR: JAVA_HOME is not set
```
**Solution:** Install Java and set JAVA_HOME

---

## Performance Tips

### Enable Caching
```properties
# gradle.properties
org.gradle.caching=true
```

### Enable Parallel
```properties
# gradle.properties
org.gradle.parallel=true
```

### Increase Memory
```properties
# gradle.properties
org.gradle.jvmargs=-Xmx4g
```

### Use Daemon
```properties
# gradle.properties
org.gradle.daemon=true
```

---

## See Also

- **[Quick Start](.gradle-docs/QUICKSTART.md)** - Get started quickly
- **[Complete Guide](.gradle-docs/README.md)** - Full documentation
- **[API Reference](.gradle-docs/API.md)** - Detailed API docs
- **[Migration Guide](.gradle-docs/MIGRATION.md)** - Migrating from Ant

---

**Last Updated:** 2025-01-XX  
**Version:** 1.0.0
