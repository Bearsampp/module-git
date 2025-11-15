# Gradle Build API Reference

## Build Script Functions

### Version Management

#### `getAvailableVersions()`

Returns a list of all available Git versions from `bin/` and `bin/archived/` directories.

**Returns**: `List<String>` - Sorted list of version strings

**Example**:
```groovy
def versions = getAvailableVersions()
// Returns: ['2.34.0', '2.50.1', '2.51.2']
```

**Usage in Tasks**:
```groovy
tasks.register('myTask') {
    doLast {
        def versions = getAvailableVersions()
        versions.each { version ->
            println "Found version: ${version}"
        }
    }
}
```

---

#### `loadRemoteGitProperties()`

Loads Git version properties from the modules-untouched GitHub repository.

**Returns**: `Properties` - Properties object with version mappings

**URL**: `https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/git.properties`

**Example**:
```groovy
def remoteProps = loadRemoteGitProperties()
def url = remoteProps.getProperty('2.51.2')
// Returns: https://github.com/.../git-2.51.2.7z
```

**Error Handling**:
- Returns empty `Properties` object on failure
- Logs warning message
- Does not throw exceptions

---

### Download & Extract

#### `downloadAndExtractGit(String version, File destDir)`

Downloads and extracts Git binaries for a specific version.

**Parameters**:
- `version` (String) - Git version (e.g., "2.51.2")
- `destDir` (File) - Destination directory for extraction

**Returns**: `File` - Directory containing extracted Git files

**Process**:
1. Checks `releases.properties` for download URL
2. Falls back to remote `git.properties` if not found
3. Downloads archive (or uses cached version)
4. Extracts using 7-Zip or built-in ZIP support
5. Returns extraction directory

**Example**:
```groovy
def extractDir = downloadAndExtractGit('2.51.2', file('build/extract'))
println "Git extracted to: ${extractDir.absolutePath}"
```

**Throws**:
- `GradleException` - If version not found
- `GradleException` - If download fails
- `GradleException` - If extraction fails
- `GradleException` - If 7-Zip not found (for .7z files)

---

### Utilities

#### `find7ZipExecutable()`

Locates the 7-Zip executable on the system.

**Returns**: `String` - Path to 7z.exe, or `null` if not found

**Search Order**:
1. `7Z_HOME` environment variable
2. Common installation paths:
   - `C:/Program Files/7-Zip/7z.exe`
   - `C:/Program Files (x86)/7-Zip/7z.exe`
   - `D:/Program Files/7-Zip/7z.exe`
   - `D:/Program Files (x86)/7-Zip/7z.exe`
3. System PATH (using `where 7z.exe`)

**Example**:
```groovy
def sevenZip = find7ZipExecutable()
if (sevenZip) {
    println "7-Zip found at: ${sevenZip}"
} else {
    println "7-Zip not found"
}
```

---

#### `generateHashFiles(File file)`

Generates hash files (MD5, SHA1, SHA256, SHA512) for a given file.

**Parameters**:
- `file` (File) - File to generate hashes for

**Returns**: `void`

**Creates**:
- `{file}.md5` - MD5 hash
- `{file}.sha1` - SHA1 hash
- `{file}.sha256` - SHA256 hash
- `{file}.sha512` - SHA512 hash

**Example**:
```groovy
def archive = file('build/output.7z')
generateHashFiles(archive)
// Creates: output.7z.md5, output.7z.sha1, etc.
```

**Throws**:
- `GradleException` - If file doesn't exist

---

#### `calculateHash(File file, String algorithm)`

Calculates hash for a file using specified algorithm.

**Parameters**:
- `file` (File) - File to hash
- `algorithm` (String) - Hash algorithm ('MD5', 'SHA-1', 'SHA-256', 'SHA-512')

**Returns**: `String` - Hexadecimal hash string

**Example**:
```groovy
def file = file('build/output.7z')
def md5 = calculateHash(file, 'MD5')
def sha256 = calculateHash(file, 'SHA-256')
println "MD5: ${md5}"
println "SHA256: ${sha256}"
```

---

## Project Properties

### Extension Properties (`ext`)

#### `bundleName`

**Type**: `String`  
**Default**: `'git'`  
**Source**: `build.properties`

The name of the bundle/module.

```groovy
println "Bundle name: ${bundleName}"
// Output: Bundle name: git
```

---

#### `bundleRelease`

**Type**: `String`  
**Default**: `'1.0.0'`  
**Source**: `build.properties`

The release version of the bundle.

```groovy
println "Release: ${bundleRelease}"
// Output: Release: 2025.11.1
```

---

#### `bundleType`

**Type**: `String`  
**Default**: `'tools'`  
**Source**: `build.properties`

The type of bundle (e.g., 'tools', 'bins', 'apps').

```groovy
println "Type: ${bundleType}"
// Output: Type: tools
```

---

#### `bundleFormat`

**Type**: `String`  
**Default**: `'7z'`  
**Source**: `build.properties`

The archive format ('7z' or 'zip').

```groovy
println "Format: ${bundleFormat}"
// Output: Format: 7z
```

---

#### `buildBasePath`

**Type**: `String`  
**Source**: `build.properties`, `BEARSAMPP_BUILD_PATH` env var, or default

The base path for build outputs.

**Priority**:
1. `build.path` in `build.properties`
2. `BEARSAMPP_BUILD_PATH` environment variable
3. Default: `{rootDir}/bearsampp-build`

```groovy
println "Build path: ${buildBasePath}"
// Output: Build path: E:/Bearsampp-development/bearsampp-build
```

---

#### `bundleTmpPrepPath`

**Type**: `String`  
**Computed**: `{buildBasePath}/tmp/bundles_prep/{bundleType}/{bundleName}`

Temporary directory for bundle preparation.

```groovy
println "Prep path: ${bundleTmpPrepPath}"
// Output: E:/Bearsampp-development/bearsampp-build/tmp/bundles_prep/tools/git
```

---

#### `bundleTmpDownloadPath`

**Type**: `String`  
**Computed**: `{buildBasePath}/tmp/downloads/{bundleName}`

Directory for downloaded archives.

```groovy
println "Download path: ${bundleTmpDownloadPath}"
// Output: E:/Bearsampp-development/bearsampp-build/tmp/downloads/git
```

---

#### `bundleTmpExtractPath`

**Type**: `String`  
**Computed**: `{buildBasePath}/tmp/extract/{bundleName}`

Directory for extracted archives.

```groovy
println "Extract path: ${bundleTmpExtractPath}"
// Output: E:/Bearsampp-development/bearsampp-build/tmp/extract/git
```

---

#### `releasesProps`

**Type**: `Properties`  
**Source**: `releases.properties`

Properties object containing version-to-URL mappings.

```groovy
def url = releasesProps.getProperty('2.51.2')
println "Download URL: ${url}"
```

---

## Tasks

### `release`

Builds a release package for a specific Git version.

**Group**: `build`

**Parameters**:
- `-PbundleVersion=X.X.X` (optional) - Specific version to build

**Interactive Mode** (no parameters):
```bash
gradle release
# Prompts for version selection
```

**Non-Interactive Mode**:
```bash
gradle release -PbundleVersion=2.51.2
```

**Process**:
1. Version selection (interactive or parameter)
2. Validate bundle path exists
3. Download and extract Git binaries
4. Prepare bundle directory
5. Copy Git files (excluding docs)
6. Copy custom configurations
7. Replace version placeholders
8. Create archive (7z or zip)
9. Generate hash files

**Output**:
- Archive: `bearsampp-build/tools/git/{release}/bearsampp-git-{version}-{release}.7z`
- Hashes: `.md5`, `.sha1`, `.sha256`, `.sha512`

---

### `listVersions`

Lists all available Git versions from `bin/` and `bin/archived/` directories.

**Group**: `help`

**Usage**:
```bash
gradle listVersions
```

**Output Example**:
```
Available git versions:
------------------------------------------------------------
  2.34.0           [bin/archived]
  2.50.1           [bin]
  2.51.2           [bin]
------------------------------------------------------------
Total versions: 3

To build a specific version:
  gradle release -PbundleVersion=2.51.2
```

---

### `info`

Displays build configuration information.

**Group**: `help`

**Usage**:
```bash
gradle info
```

**Output Example**:
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

### `clean`

Removes build artifacts and temporary files.

**Group**: `build`

**Usage**:
```bash
gradle clean
```

**Removes**:
- `build/` directory (Gradle build directory)

**Note**: Does not remove `bearsampp-build/` directory (shared across modules)

---

## Custom Task Examples

### Example 1: Build Multiple Versions

```groovy
tasks.register('buildMultiple') {
    group = 'build'
    description = 'Build multiple versions'
    
    doLast {
        def versions = ['2.50.1', '2.51.2']
        versions.each { version ->
            println "Building version ${version}..."
            exec {
                commandLine 'gradle', 'release', "-PbundleVersion=${version}"
            }
        }
    }
}
```

Usage:
```bash
gradle buildMultiple
```

---

### Example 2: Verify All Versions

```groovy
tasks.register('verifyAll') {
    group = 'verification'
    description = 'Verify all version configurations'
    
    doLast {
        def versions = getAvailableVersions()
        def errors = []
        
        versions.each { version ->
            def bundleFolder = "${bundleName}${version}"
            def bundlePath = file("bin/${bundleFolder}")
            
            if (!bundlePath.exists()) {
                bundlePath = file("bin/archived/${bundleFolder}")
            }
            
            def conf = new File(bundlePath, 'bearsampp.conf')
            if (!conf.exists()) {
                errors << "Missing bearsampp.conf for ${version}"
            }
        }
        
        if (errors.isEmpty()) {
            println "✓ All versions verified successfully"
        } else {
            errors.each { println "✗ ${it}" }
            throw new GradleException("Verification failed")
        }
    }
}
```

Usage:
```bash
gradle verifyAll
```

---

### Example 3: Generate Version Report

```groovy
tasks.register('versionReport') {
    group = 'help'
    description = 'Generate version report'
    
    doLast {
        def versions = getAvailableVersions()
        def report = new File(buildDir, 'version-report.txt')
        
        report.text = """
Git Module Version Report
Generated: ${new Date()}

Total Versions: ${versions.size()}

Versions:
${versions.collect { "  - ${it}" }.join('\n')}

URLs:
${versions.collect { version ->
    def url = releasesProps.getProperty(version) ?: 'Not in releases.properties'
    "  ${version}: ${url}"
}.join('\n')}
        """.stripIndent()
        
        println "Report generated: ${report.absolutePath}"
    }
}
```

Usage:
```bash
gradle versionReport
```

---

## Error Handling

### Common Exceptions

#### `GradleException`

Thrown for build failures:

```groovy
throw new GradleException("Version not found")
```

**Common Causes**:
- Version not found in bin/ directory
- Download URL not found
- 7-Zip not installed
- Extraction failed
- Invalid archive format

#### Catching Exceptions

```groovy
tasks.register('safeRelease') {
    doLast {
        try {
            // Build logic
        } catch (GradleException e) {
            logger.error("Build failed: ${e.message}")
            // Handle error
        }
    }
}
```

---

## Logging

### Log Levels

```groovy
logger.error("Error message")    // Always shown
logger.warn("Warning message")   // Shown by default
logger.lifecycle("Info message") // Shown by default
logger.info("Debug message")     // Use --info flag
logger.debug("Trace message")    // Use --debug flag
```

### Usage

```bash
# Normal output
gradle release

# Verbose output
gradle release --info

# Debug output
gradle release --debug

# Quiet output
gradle release --quiet
```

---

## Configuration

### Gradle Properties

Create `gradle.properties` in project root:

```properties
# Performance
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.daemon=true
org.gradle.configureondemand=true

# Memory
org.gradle.jvmargs=-Xmx2g -XX:MaxMetaspaceSize=512m

# Logging
org.gradle.logging.level=lifecycle
```

### Build Properties

Edit `build.properties`:

```properties
bundle.name=git
bundle.release=2025.11.1
bundle.type=tools
bundle.format=7z
build.path=D:/custom-build  # Optional custom path
```

---

## Environment Variables

### `BEARSAMPP_BUILD_PATH`

Override default build directory:

```bash
set BEARSAMPP_BUILD_PATH=D:/custom-build
gradle release
```

### `7Z_HOME`

Specify 7-Zip installation directory:

```bash
set 7Z_HOME=C:/Program Files/7-Zip
gradle release
```

---

## Return Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Build failure |
| 2 | Configuration error |

Check return code:

```bash
gradle release
echo Exit code: %ERRORLEVEL%
```
