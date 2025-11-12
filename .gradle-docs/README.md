# Bearsampp Module Git - Gradle Build Documentation

## Overview

This project uses Gradle to build and package Git module releases for Bearsampp. The build system downloads Git binaries, combines them with custom configurations, and creates distributable archives.

## Quick Start

### Prerequisites

- Java 8 or higher
- Gradle 6.0 or higher
- 7-Zip (for .7z compression)

### Basic Commands

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

## Project Structure

```
module-git/
├── bin/                          # Git version configurations
│   ├── git2.50.1/               # Active version
│   ├── git2.51.2/               # Active version
│   └── archived/                # Archived versions
│       └── git2.34.0/
├── build.gradle                 # Main build script
├── build.properties             # Build configuration
├── releases.properties          # Version download URLs
├── settings.gradle              # Gradle settings
├── gradle.properties            # Gradle configuration
└── .gradle-docs/                # Documentation
```

## Build Directory Structure

The build uses a shared `bearsampp-build` directory structure:

```
bearsampp-build/
├── tmp/
│   ├── downloads/git/           # Downloaded archives
│   ├── extract/git/             # Extracted sources
│   └── bundles_prep/tools/git/  # Prepared bundles
└── tools/git/{release}/         # Final release archives
```

## Configuration Files

### build.properties

Defines bundle configuration:

```properties
bundle.name=git
bundle.release=2025.11.1
bundle.type=tools
bundle.format=7z
```

### releases.properties

Maps versions to download URLs:

```properties
2.51.2=https://github.com/Bearsampp/module-git/releases/download/2025.11.1/bearsampp-git-2.51.2-2025.11.1.7z
2.50.1=https://github.com/Bearsampp/module-git/releases/download/2025.7.10/bearsampp-git-2.50.1-2025.7.10.7z
```

If a version is not in `releases.properties`, the build will check:
`https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/git.properties`

## Available Tasks

### Build Tasks

- **`release`** - Build release package for a specific version
  - Interactive mode: prompts for version selection
  - Non-interactive: use `-PbundleVersion=X.X.X`
  - Generates .7z/.zip archive with hash files (MD5, SHA1, SHA256, SHA512)

### Help Tasks

- **`info`** - Display build configuration information
- **`listVersions`** - List all available versions in bin/ and bin/archived/
- **`tasks`** - Show all available Gradle tasks

### Maintenance Tasks

- **`clean`** - Remove build artifacts

## Build Process

### 1. Version Selection

When running `gradle release` without parameters, you'll see an interactive menu:

```
======================================================================
Available git versions:
======================================================================
  1. 2.34.0           [bin/archived]
  2. 2.50.1           [bin]
  3. 2.51.2           [bin]
======================================================================

Enter version number or full version string:
```

You can:
- Enter a number (e.g., `3`)
- Enter the version string (e.g., `2.51.2`)
- Press Enter to select the last version

### 2. Download & Extract

The build:
1. Checks `releases.properties` for the download URL
2. Falls back to remote `git.properties` if not found
3. Downloads the archive (or uses cached version)
4. Extracts to temporary directory
5. Handles nested folder structures automatically

### 3. Bundle Preparation

The build:
1. Copies Git binaries from extracted source
2. Excludes `doc/**` directory
3. Copies custom configurations from `bin/git{version}/`
4. Replaces `@RELEASE_VERSION@` in `bearsampp.conf`

### 4. Archive Creation

The build:
1. Creates output directory: `bearsampp-build/tools/git/{release}/`
2. Compresses bundle using 7-Zip or ZIP
3. Generates hash files (MD5, SHA1, SHA256, SHA512)
4. Names archive: `bearsampp-git-{version}-{release}.7z`

## Version Management

### Adding a New Version

1. Create configuration directory:
   ```bash
   mkdir bin/git2.52.0
   ```

2. Add required files:
   - `bearsampp.conf` - Module configuration
   - Any custom scripts or configurations

3. Add download URL to `releases.properties`:
   ```properties
   2.52.0=https://github.com/Bearsampp/module-git/releases/download/2025.12.1/bearsampp-git-2.52.0-2025.12.1.7z
   ```

4. Build the release:
   ```bash
   gradle release -PbundleVersion=2.52.0
   ```

### Archiving Old Versions

Move old versions to the archived directory:

```bash
mv bin/git2.34.0 bin/archived/
```

Archived versions remain available for building but are marked as `[bin/archived]` in the version list.

## Advanced Usage

### Custom Build Path

Set a custom build directory via:

1. **build.properties**:
   ```properties
   build.path=D:/custom-build
   ```

2. **Environment variable**:
   ```bash
   set BEARSAMPP_BUILD_PATH=D:/custom-build
   gradle release
   ```

### Non-Interactive CI/CD

For automated builds:

```bash
gradle release -PbundleVersion=2.51.2 --no-daemon --console=plain
```

### Building Multiple Versions

Build multiple versions in sequence:

```bash
gradle release -PbundleVersion=2.50.1
gradle release -PbundleVersion=2.51.2
```

## Troubleshooting

### 7-Zip Not Found

**Error**: `7-Zip not found. Please install 7-Zip or set 7Z_HOME environment variable.`

**Solution**:
1. Install 7-Zip from https://www.7-zip.org/
2. Or set `7Z_HOME` environment variable:
   ```bash
   set 7Z_HOME=C:\Program Files\7-Zip
   ```

### Version Not Found

**Error**: `Version X.X.X not found in releases.properties or remote git.properties`

**Solution**:
1. Add the version to `releases.properties` with a download URL
2. Or add to https://github.com/Bearsampp/modules-untouched/blob/main/modules/git.properties

### Nested Directory Issues

If the extracted archive contains nested folders (e.g., `git2.51.2/git2.51.2/`), the build automatically detects and uses the nested directory.

### Download Failures

If downloads fail:
1. Check internet connection
2. Verify URL in `releases.properties`
3. Check if GitHub releases are accessible
4. Clear download cache: `rm -rf bearsampp-build/tmp/downloads/`

## Output Files

After a successful build, you'll find:

```
bearsampp-build/tools/git/2025.11.1/
├── bearsampp-git-2.51.2-2025.11.1.7z
├── bearsampp-git-2.51.2-2025.11.1.7z.md5
├── bearsampp-git-2.51.2-2025.11.1.7z.sha1
├── bearsampp-git-2.51.2-2025.11.1.7z.sha256
└── bearsampp-git-2.51.2-2025.11.1.7z.sha512
```

## Integration with Bearsampp

The generated archives are designed to be:
1. Extracted to Bearsampp's `bin/git/` directory
2. Configured via `bearsampp.conf`
3. Managed by Bearsampp's module system

## Contributing

When contributing new versions:
1. Test the build locally
2. Verify the archive structure
3. Update `releases.properties`
4. Submit a pull request

## Support

For issues or questions:
- GitHub Issues: https://github.com/Bearsampp/module-git/issues
- Documentation: https://github.com/Bearsampp/module-git/tree/gradle-convert/.gradle-docs
