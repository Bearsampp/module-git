# Gradle Installation Guide

## Prerequisites

Before building module-git, you need to install the following tools:

### 1. Java Development Kit (JDK)

**Required Version:** Java 8 or higher (Java 17 recommended)

#### Windows

**Option A: Using Chocolatey**
```powershell
choco install temurin17
```

**Option B: Manual Installation**
1. Download from https://adoptium.net/
2. Run the installer
3. Set JAVA_HOME environment variable:
   ```powershell
   setx JAVA_HOME "C:\Program Files\Eclipse Adoptium\jdk-17.0.x"
   setx PATH "%PATH%;%JAVA_HOME%\bin"
   ```

#### Linux

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install openjdk-17-jdk
```

**Fedora/RHEL:**
```bash
sudo dnf install java-17-openjdk-devel
```

#### macOS

**Using Homebrew:**
```bash
brew install openjdk@17
```

**Verify Installation:**
```bash
java -version
# Should output: openjdk version "17.x.x" or higher
```

---

### 2. Gradle Build Tool

**Required Version:** Gradle 8.5 or higher

#### Windows

**Option A: Using Chocolatey**
```powershell
choco install gradle
```

**Option B: Using Scoop**
```powershell
scoop install gradle
```

**Option C: Manual Installation**
1. Download from https://gradle.org/releases/
2. Extract to `C:\Gradle`
3. Add to PATH:
   ```powershell
   setx PATH "%PATH%;C:\Gradle\gradle-8.5\bin"
   ```

#### Linux

**Ubuntu/Debian:**
```bash
# Using SDKMAN (recommended)
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install gradle 8.5

# Or using package manager
sudo apt update
sudo apt install gradle
```

**Fedora/RHEL:**
```bash
sudo dnf install gradle
```

#### macOS

**Using Homebrew:**
```bash
brew install gradle
```

**Verify Installation:**
```bash
gradle --version
# Should output: Gradle 8.5 or higher
```

---

### 3. 7-Zip

**Required for:** Creating .7z archives

#### Windows

**Option A: Using Chocolatey**
```powershell
choco install 7zip
```

**Option B: Manual Installation**
1. Download from https://www.7-zip.org/
2. Run the installer
3. Add to PATH:
   ```powershell
   setx PATH "%PATH%;C:\Program Files\7-Zip"
   ```

#### Linux

**Ubuntu/Debian:**
```bash
sudo apt install p7zip-full
```

**Fedora/RHEL:**
```bash
sudo dnf install p7zip p7zip-plugins
```

#### macOS

**Using Homebrew:**
```bash
brew install p7zip
```

**Verify Installation:**
```bash
7z
# Should show 7-Zip version and usage information
```

---

## Verification

After installing all prerequisites, verify your setup:

```bash
# Check Java
java -version
# Expected: openjdk version "17.x.x" or higher

# Check Gradle
gradle --version
# Expected: Gradle 8.5 or higher

# Check 7-Zip
7z
# Expected: 7-Zip version information
```

---

## Environment Variables

### Windows

Set the following environment variables:

```powershell
# JAVA_HOME
setx JAVA_HOME "C:\Program Files\Eclipse Adoptium\jdk-17.0.x"

# Add to PATH
setx PATH "%PATH%;%JAVA_HOME%\bin;C:\Gradle\gradle-8.5\bin;C:\Program Files\7-Zip"
```

### Linux/macOS

Add to your `~/.bashrc` or `~/.zshrc`:

```bash
# JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
export PATH=$JAVA_HOME/bin:$PATH

# Gradle (if installed manually)
export GRADLE_HOME=/opt/gradle/gradle-8.5
export PATH=$GRADLE_HOME/bin:$PATH
```

Then reload:
```bash
source ~/.bashrc  # or source ~/.zshrc
```

---

## Gradle Configuration

### Global Configuration

Create or edit `~/.gradle/gradle.properties`:

```properties
# Performance settings
org.gradle.jvmargs=-Xmx2g -XX:MaxMetaspaceSize=512m
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.daemon=true

# Kotlin
kotlin.code.style=official
```

### Project Configuration

The project includes `gradle.properties` with optimized settings. No additional configuration needed.

---

## First Build Test

After installation, test your setup:

```bash
# Navigate to project
cd E:/Bearsampp-development/module-git

# Show build info
gradle buildInfo

# List available versions
gradle listVersions

# Build latest version
gradle buildRelease
```

If all commands succeed, your installation is complete!

---

## Troubleshooting

### Java Issues

**Problem:** `java: command not found`

**Solution:**
1. Verify Java is installed: `java -version`
2. Check JAVA_HOME is set: `echo $JAVA_HOME` (Linux/Mac) or `echo %JAVA_HOME%` (Windows)
3. Ensure Java bin directory is in PATH

---

**Problem:** `JAVA_HOME is set to an invalid directory`

**Solution:**
1. Find Java installation:
   - Windows: `where java`
   - Linux/Mac: `which java`
2. Set JAVA_HOME to the JDK directory (not the bin directory)

---

### Gradle Issues

**Problem:** `gradle: command not found`

**Solution:**
1. Verify Gradle is installed: `gradle --version`
2. Check Gradle is in PATH
3. Restart terminal/command prompt

---

**Problem:** `Could not determine java version from 'X.X.X'`

**Solution:**
- Gradle 8.5 requires Java 8 or higher
- Update Java to a compatible version

---

### 7-Zip Issues

**Problem:** `7z: command not found`

**Solution:**
1. Verify 7-Zip is installed
2. Add 7-Zip to PATH
3. On Windows, use `7z` not `7zip`

---

**Problem:** `Cannot run program "7z"`

**Solution:**
- Windows: Add `C:\Program Files\7-Zip` to PATH
- Linux: Install `p7zip-full` package
- macOS: Install via Homebrew

---

## IDE Setup

### IntelliJ IDEA

1. **Open Project**
   - File → Open → Select `module-git` directory
   - IDEA automatically detects Gradle

2. **Configure JDK**
   - File → Project Structure → Project
   - Set Project SDK to Java 17

3. **Gradle Settings**
   - File → Settings → Build, Execution, Deployment → Build Tools → Gradle
   - Gradle JVM: Use Project JDK

4. **Run Tasks**
   - View → Tool Windows → Gradle
   - Expand `module-git → Tasks → bearsampp`
   - Double-click any task to run

### VS Code

1. **Install Extensions**
   - Java Extension Pack
   - Gradle for Java

2. **Open Project**
   - File → Open Folder → Select `module-git`

3. **Configure Java**
   - Ctrl+Shift+P → "Java: Configure Java Runtime"
   - Set Java 17

4. **Run Tasks**
   - Ctrl+Shift+P → "Gradle: Run a Gradle Task"
   - Select task from list

### Eclipse

1. **Install Buildship**
   - Help → Eclipse Marketplace
   - Search "Buildship Gradle Integration"
   - Install

2. **Import Project**
   - File → Import → Gradle → Existing Gradle Project
   - Select `module-git` directory

3. **Run Tasks**
   - Right-click project → Gradle → Refresh Gradle Project
   - Right-click project → Run As → Gradle Build

---

## Upgrading

### Upgrade Java

```bash
# Check current version
java -version

# Upgrade (example for Ubuntu)
sudo apt update
sudo apt install openjdk-17-jdk

# Verify
java -version
```

### Upgrade Gradle

```bash
# Check current version
gradle --version

# Upgrade using SDKMAN
sdk install gradle 8.5
sdk use gradle 8.5

# Or using package manager
# Windows (Chocolatey)
choco upgrade gradle

# macOS (Homebrew)
brew upgrade gradle

# Verify
gradle --version
```

### Upgrade 7-Zip

```bash
# Windows (Chocolatey)
choco upgrade 7zip

# Linux (Ubuntu)
sudo apt update
sudo apt upgrade p7zip-full

# macOS (Homebrew)
brew upgrade p7zip
```

---

## Uninstallation

### Remove Java

**Windows:**
```powershell
# Using Chocolatey
choco uninstall temurin17

# Or use Windows Settings → Apps
```

**Linux:**
```bash
sudo apt remove openjdk-17-jdk
```

**macOS:**
```bash
brew uninstall openjdk@17
```

### Remove Gradle

**Windows:**
```powershell
choco uninstall gradle
```

**Linux:**
```bash
# If installed via SDKMAN
sdk uninstall gradle 8.5

# If installed via package manager
sudo apt remove gradle
```

**macOS:**
```bash
brew uninstall gradle
```

### Remove 7-Zip

**Windows:**
```powershell
choco uninstall 7zip
```

**Linux:**
```bash
sudo apt remove p7zip-full
```

**macOS:**
```bash
brew uninstall p7zip
```

---

## Additional Resources

### Official Documentation

- **Java**: https://adoptium.net/
- **Gradle**: https://gradle.org/install/
- **7-Zip**: https://www.7-zip.org/

### Package Managers

- **Chocolatey (Windows)**: https://chocolatey.org/
- **Homebrew (macOS)**: https://brew.sh/
- **SDKMAN (Linux/macOS)**: https://sdkman.io/

### Gradle Resources

- **User Manual**: https://docs.gradle.org/current/userguide/userguide.html
- **Kotlin DSL**: https://docs.gradle.org/current/userguide/kotlin_dsl.html
- **Performance Guide**: https://docs.gradle.org/current/userguide/performance.html

---

## Next Steps

After completing installation:

1. **Read Quick Start**: [QUICKSTART.md](QUICKSTART.md)
2. **Build Your First Release**: `gradle buildRelease`
3. **Explore Documentation**: [README.md](README.md)
4. **Learn Advanced Features**: [API.md](API.md)

---

**Last Updated:** 2025-01-XX  
**Gradle Version:** 8.5+  
**Java Version:** 8+ (17 recommended)
