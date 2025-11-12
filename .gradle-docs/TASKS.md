# Gradle Tasks Reference Card

Quick reference for all available Gradle tasks in module-git.

## Task Groups

### 🏗️ Build Tasks

#### buildRelease
**Build a release bundle**

```bash
# Build latest version
gradle buildRelease

# Build specific version
gradle buildRelease -PbundleVersion=2.51.2
```

**What it does:**
1. Downloads source from untouched modules
2. Extracts archive
3. Copies files (excluding doc/)
4. Overlays bundle configuration
5. Replaces version placeholders
6. Creates 7z/zip archive

**Output:** `build/bearsampp-git-{version}-{release}.{format}`

**Dependencies:** prepareBundle

---

#### buildAllReleases
**Build all bundle versions**

```bash
# Build all versions sequentially
gradle buildAllReleases

# Build all versions in parallel
gradle buildAllReleases --parallel

# Build with 4 parallel workers
gradle buildAllReleases --parallel --max-workers=4
```

**What it does:**
- Finds all versions in bin/
- Builds each version
- Can run in parallel

**Output:** Multiple files in `build/`

---

#### prepareBundle
**Prepare a bundle for packaging**

```bash
# Prepare latest version
gradle prepareBundle

# Prepare specific version
gradle prepareBundle -PbundleVersion=2.51.2
```

**What it does:**
1. Downloads source
2. Extracts files
3. Verifies git.exe exists
4. Copies to prep directory
5. Overlays configuration
6. Replaces placeholders

**Output:** `../.tmp/tmp/prep/git{version}/`

**Used by:** buildRelease

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

#### buildInfo
**Show build configuration**

```bash
gradle buildInfo
```

**Output:**
```
=== Build Configuration ===
Bundle Name:    git
Bundle Release: 2025.11.1
Bundle Type:    tools
Bundle Format:  7z

Project Dir:    E:/Bearsampp-development/module-git
Build Dir:      E:/Bearsampp-development/module-git/build
Dist Dir:       E:/Bearsampp-development/module-git/release

Available Versions: 2.50.1, 2.51.2
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

### ✅ Verification Tasks

#### verifyBundle
**Verify bundle structure**

```bash
# Verify latest version
gradle verifyBundle

# Verify specific version
gradle verifyBundle -PbundleVersion=2.51.2
```

**What it checks:**
- bearsampp.conf exists
- Required files present

**Output:**
```
Verifying bundle: git2.51.2
  ✓ bearsampp.conf

Bundle structure is valid ✓
```

---

## Task Options

### Common Options

#### -P (Project Property)
Pass properties to the build:

```bash
gradle buildRelease -PbundleVersion=2.51.2
```

#### --info
Show info-level logging:

```bash
gradle buildRelease --info
```

#### --debug
Show debug-level logging:

```bash
gradle buildRelease --debug
```

#### --stacktrace
Show stack traces on errors:

```bash
gradle buildRelease --stacktrace
```

#### --parallel
Enable parallel execution:

```bash
gradle buildAllReleases --parallel
```

#### --max-workers
Set maximum parallel workers:

```bash
gradle buildAllReleases --parallel --max-workers=4
```

#### --offline
Use cached dependencies only:

```bash
gradle buildRelease --offline
```

#### --refresh-dependencies
Force refresh of dependencies:

```bash
gradle buildRelease --refresh-dependencies
```

#### --continuous
Watch for changes and rebuild:

```bash
gradle buildRelease --continuous
```

#### --dry-run
Show what would be executed:

```bash
gradle buildRelease --dry-run
```

---

## Task Combinations

### Clean and Build

```bash
gradle clean buildRelease
```

### Build with Debug

```bash
gradle buildRelease --info --stacktrace
```

### Parallel Build All

```bash
gradle clean buildAllReleases --parallel --max-workers=4
```

### Verify and Build

```bash
gradle verifyBundle buildRelease -PbundleVersion=2.51.2
```

---

## Task Dependencies

```
buildRelease
    ↓
prepareBundle
    ↓
(downloads and prepares files)

buildAllReleases
    ↓
(calls buildRelease for each version)

verifyBundle
    ↓
(checks bundle structure)
```

---

## Quick Reference

| Task | Purpose | Example |
|------|---------|---------|
| `buildRelease` | Build single version | `gradle buildRelease` |
| `buildAllReleases` | Build all versions | `gradle buildAllReleases` |
| `prepareBundle` | Prepare bundle | `gradle prepareBundle` |
| `listVersions` | List versions | `gradle listVersions` |
| `buildInfo` | Show config | `gradle buildInfo` |
| `verifyBundle` | Verify structure | `gradle verifyBundle` |
| `clean` | Clean build | `gradle clean` |
| `tasks` | Show all tasks | `gradle tasks` |

---

## Common Workflows

### Development

```bash
# 1. Check configuration
gradle buildInfo

# 2. List versions
gradle listVersions

# 3. Verify bundle
gradle verifyBundle

# 4. Build
gradle buildRelease
```

### Release

```bash
# 1. Clean
gradle clean

# 2. Build all
gradle buildAllReleases --parallel

# 3. Verify outputs
ls -lh build/
```

### Debugging

```bash
# 1. Clean build
gradle clean

# 2. Build with debug
gradle buildRelease --debug --stacktrace

# 3. Check info
gradle buildInfo --info
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
../.tmp/
├── tmp/
│   ├── src/              # Downloaded sources
│   │   └── git2.51.2/
│   └── prep/             # Prepared bundles
│       └── git2.51.2/
└── dist/                 # Final releases
    └── bearsampp-git-2.51.2-2025.11.1.7z
```

### Cache Directory
```
~/.gradle/
├── caches/              # Build cache
├── wrapper/             # Gradle
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
