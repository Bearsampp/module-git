# Quick Start Guide

## 5-Minute Setup

### Prerequisites Check

```bash
# Check Java
java -version
# Should show Java 8 or higher

# Check Gradle
gradle --version
# Should show Gradle 8.5 or higher

# Check 7-Zip
7z
# Should show 7-Zip version info
```

If missing:
- **Java**: Download from https://adoptium.net/
- **Gradle**: Download from https://gradle.org/install/
- **7-Zip**: Download from https://www.7-zip.org/

### First Build

```bash
# Navigate to project
cd E:/Bearsampp-development/module-git

# Show available versions
gradle listVersions

# Build latest version
gradle buildRelease

# Find output
ls ../build/
```

That's it! Your first build is complete.

## Common Commands

### Build Commands

```bash
# Build latest version
gradle buildRelease

# Build specific version
gradle buildRelease -PbundleVersion=2.51.2

# Build all versions
gradle buildAllReleases

# Build with parallel execution
gradle buildAllReleases --parallel
```

### Information Commands

```bash
# Show build configuration
gradle buildInfo

# List all available versions
gradle listVersions

# Verify bundle structure
gradle verifyBundle
```

### Maintenance Commands

```bash
# Clean build artifacts
gradle clean

# Clean and rebuild
gradle clean buildRelease

# Show all tasks
gradle tasks
```

## Understanding Output

### Build Output Location

```
../.tmp/
├── tmp/
│   ├── src/              # Downloaded sources
│   └── prep/             # Prepared bundles
└── dist/                 # Final releases ← YOUR FILES ARE HERE
    └── bearsampp-git-2.51.2-2025.11.1.7z
```

### Output Filename Format

```
bearsampp-{module}-{version}-{release}.{format}
         ↓        ↓         ↓          ↓
         git    2.51.2   2025.11.1    7z
```

## Troubleshooting

### Build Fails

```bash
# Try with more logging
gradle buildRelease --info

# Or with debug output
gradle buildRelease --debug

# Or with stack traces
gradle buildRelease --stacktrace
```

### Clean Start

```bash
# Remove all build artifacts
gradle clean

# Rebuild from scratch
gradle buildRelease --refresh-dependencies
```

### Version Not Found

```bash
# Check what versions are available
gradle listVersions

# Make sure version exists in bin/
ls bin/
```

## Next Steps

1. **Read the full documentation**: `.gradle-docs/README.md`
2. **Learn about migration**: `.gradle-docs/MIGRATION.md`
3. **Explore the API**: `.gradle-docs/API.md`
4. **Customize the build**: Edit `build.gradle.kts`

## Tips

### Speed Up Builds

```bash
# Use parallel execution
gradle buildAllReleases --parallel --max-workers=4

# Use Gradle daemon (enabled by default)
# Subsequent builds will be faster
```

### IDE Integration

**IntelliJ IDEA:**
1. Open project
2. IDEA automatically detects Gradle
3. Use Gradle tool window for tasks

**VS Code:**
1. Install "Gradle for Java" extension
2. Use Gradle sidebar for tasks

### CI/CD Integration

**GitHub Actions:**
```yaml
- name: Build with Gradle
  run: gradle buildRelease
```

**Jenkins:**
```groovy
stage('Build') {
    steps {
        sh 'gradle buildRelease'
    }
}
```

## Help

- **Documentation**: `.gradle-docs/`
- **Issues**: https://github.com/Bearsampp/module-git/issues
- **Gradle Docs**: https://docs.gradle.org

## Cheat Sheet

| Task | Command |
|------|---------|
| Build latest | `gradle buildRelease` |
| Build specific | `gradle buildRelease -PbundleVersion=X.X.X` |
| Build all | `gradle buildAllReleases` |
| List versions | `gradle listVersions` |
| Show info | `gradle buildInfo` |
| Verify | `gradle verifyBundle` |
| Clean | `gradle clean` |
| Help | `gradle tasks` |

## Example Workflow

```bash
# 1. Check configuration
gradle buildInfo

# 2. List available versions
gradle listVersions

# 3. Verify bundle structure
gradle verifyBundle -PbundleVersion=2.51.2

# 4. Build the release
gradle buildRelease -PbundleVersion=2.51.2

# 5. Check output
ls -lh build/

# 6. Clean up (optional)
gradle clean
```

## Success!

You're now ready to build module-git with Gradle. Happy building! 🚀
