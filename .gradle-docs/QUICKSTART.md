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

# Show build information
gradle info

# List available versions
gradle listVersions

# Build a specific version (interactive)
gradle release

# Build a specific version (non-interactive)
gradle release -PbundleVersion=2.51.2
```

That's it! Your first build is complete.

## Common Commands

### Build Commands

```bash
# Build a specific version (interactive)
gradle release

# Build a specific version (non-interactive)
gradle release -PbundleVersion=2.51.2
```

### Information Commands

```bash
# Show build configuration
gradle info

# List all available versions
gradle listVersions
```

### Maintenance Commands

```bash
# Clean build artifacts
gradle clean

# Clean and rebuild
gradle clean release

# Show all tasks
gradle tasks
```

## Understanding Output

### Build Output Location

```
bearsampp-build/
├── tmp/
│   ├── downloads/git/    # Downloaded archives
│   ├── extract/git/      # Extracted sources
│   └── bundles_prep/     # Prepared bundles
└── tools/git/2025.11.1/  # Final releases ← YOUR FILES ARE HERE
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
gradle release --info

# Or with debug output
gradle release --debug

# Or with stack traces
gradle release --stacktrace
```

### Clean Start

```bash
# Remove all build artifacts
gradle clean

# Rebuild from scratch
gradle release --refresh-dependencies
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
4. **Customize the build**: Edit `build.gradle`

## Tips

### Speed Up Builds

```bash
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
  run: gradle release -PbundleVersion=2.51.2
```

**Jenkins:**
```groovy
stage('Build') {
    steps {
        sh 'gradle release -PbundleVersion=2.51.2'
    }
}
```

## Help

- **Documentation**: `.gradle-docs/`
- **Issues**: https://github.com/Bearsampp/module-git/issues
- **Gradle Docs**: https://docs.gradle.org

## Cheat Sheet

| Task             | Command                                  |
|------------------|------------------------------------------|
| Show info        | `gradle info`                            |
| List versions    | `gradle listVersions`                    |
| Build (interact) | `gradle release`                         |
| Build specific   | `gradle release -PbundleVersion=X.X.X`   |
| Clean            | `gradle clean`                           |
| Help             | `gradle tasks`                           |

## Example Workflow

```bash
# 1. Check configuration
gradle info

# 2. List available versions
gradle listVersions

# 3. Build the release
gradle release -PbundleVersion=2.51.2

# 4. Check output
ls -lh ../bearsampp-build/tools/git/2025.11.1/

# 5. Clean up (optional)
gradle clean
```

## Success!

You're now ready to build module-git with Gradle. Happy building! 🚀
