# Gradle Build Documentation

This directory contains comprehensive documentation for the Gradle build system used in the module-bruno project.

## 📚 Documentation Index

### Quick Start
- **[QUICK_REFERENCE.md](QUICK_REFERENCE.md)** - Quick reference guide for common tasks and commands

### Feature Documentation
- **[MODULES_UNTOUCHED_INTEGRATION.md](MODULES_UNTOUCHED_INTEGRATION.md)** - Complete guide to the modules-untouched integration feature
- **[FEATURE_SUMMARY.md](FEATURE_SUMMARY.md)** - High-level overview of the smart module download system
- **[CONFIGURATION_SUMMARY.md](CONFIGURATION_SUMMARY.md)** - Configuration overview and version resolution strategy

### Technical Documentation
- **[IMPLEMENTATION_SUMMARY.md](IMPLEMENTATION_SUMMARY.md)** - Technical implementation details and architecture
- **[GRADLE_APACHE_COMPARISON.md](GRADLE_APACHE_COMPARISON.md)** - Comparison between Gradle and Apache Ant builds
- **[GRADLE_TMP_PATHS.md](GRADLE_TMP_PATHS.md)** - Documentation of temporary path structure and usage

### Changelogs
- **[CHANGELOG_MODULES_UNTOUCHED.md](CHANGELOG_MODULES_UNTOUCHED.md)** - Changelog for modules-untouched integration
- **[CHANGELOG_TMP_PATHS.md](CHANGELOG_TMP_PATHS.md)** - Changelog for temporary paths implementation
- **[CHANGELOG_USE_BEARSAMPP_BUILD_TMP.md](CHANGELOG_USE_BEARSAMPP_BUILD_TMP.md)** - Changelog for bearsampp-build/tmp usage

## 🚀 Getting Started

### For New Users
1. Start with [QUICK_REFERENCE.md](QUICK_REFERENCE.md) for basic commands
2. Read [FEATURE_SUMMARY.md](FEATURE_SUMMARY.md) for an overview of capabilities
3. Refer to [MODULES_UNTOUCHED_INTEGRATION.md](MODULES_UNTOUCHED_INTEGRATION.md) for automatic download features

### For Developers
1. Review [IMPLEMENTATION_SUMMARY.md](IMPLEMENTATION_SUMMARY.md) for technical details
2. Check [GRADLE_APACHE_COMPARISON.md](GRADLE_APACHE_COMPARISON.md) for migration information
3. Consult [GRADLE_TMP_PATHS.md](GRADLE_TMP_PATHS.md) for path structure

### For Maintainers
1. Review all changelogs for historical context
2. Check [MODULES_UNTOUCHED_INTEGRATION.md](MODULES_UNTOUCHED_INTEGRATION.md) for version management
3. Refer to [IMPLEMENTATION_SUMMARY.md](IMPLEMENTATION_SUMMARY.md) for system architecture

## 📖 Documentation Categories

### Build System
- Gradle configuration and setup
- Task definitions and usage
- Path management
- Build lifecycle

### Version Management
- Automatic version resolution
- modules-untouched integration
- URL resolution priority
- Caching strategies

### Migration & Comparison
- Gradle vs Apache Ant
- Path structure changes
- Compatibility notes

### Troubleshooting
- Common issues and solutions
- Error messages explained
- Debug procedures

## 🔗 Quick Links

### Common Tasks
```bash
# Display build information
gradle info

# Build a specific version
gradle release -PbundleVersion=X.X.X

# Build all versions
gradle releaseAll

# List available versions
gradle listVersions

# Verify build environment
gradle verify
```

### Key Features
- ✅ Automatic download from modules-untouched
- ✅ Smart URL resolution (3-tier fallback)
- ✅ Efficient caching system
- ✅ Backward compatible with existing builds
- ✅ Clear error messages and logging

## 📝 Document Descriptions

### QUICK_REFERENCE.md
Quick start guide with common commands, scenarios, and troubleshooting tips. Perfect for day-to-day usage.

### MODULES_UNTOUCHED_INTEGRATION.md
Complete documentation of the automatic download feature that fetches Bruno binaries from the modules-untouched repository when versions aren't in releases.properties.

### FEATURE_SUMMARY.md
High-level overview of the smart module download system, including the three-tier URL resolution strategy and key benefits.

### IMPLEMENTATION_SUMMARY.md
Technical implementation details including build flow diagrams, function descriptions, and testing scenarios.

### GRADLE_APACHE_COMPARISON.md
Comparison between the Gradle build system and the legacy Apache Ant build, highlighting differences and improvements.

### GRADLE_TMP_PATHS.md
Documentation of the temporary path structure used by the build system, including download, extract, and preparation directories.

### CHANGELOG_MODULES_UNTOUCHED.md
Complete changelog for the modules-untouched integration feature, including all changes, impacts, and migration notes.

### CHANGELOG_TMP_PATHS.md
Changelog documenting the implementation of the temporary paths system.

### CHANGELOG_USE_BEARSAMPP_BUILD_TMP.md
Changelog for the transition to using the shared bearsampp-build/tmp directory structure.

## 🛠️ Build System Overview

### URL Resolution Priority
1. **Local releases.properties** (Primary)
2. **modules-untouched bruno.properties** (Secondary)
3. **Standard URL format** (Fallback)

### Cache Locations
- **Downloads**: `bearsampp-build/tmp/downloads/bruno/`
- **Extractions**: `bearsampp-build/tmp/extract/bruno/`
- **Preparations**: `bearsampp-build/tmp/bundles_prep/tools/bruno/`

### Build Output
- **Archives**: `bearsampp-build/tools/bruno/{release}/`
- **Hash files**: `.md5`, `.sha1`, `.sha256`, `.sha512`

## 🔍 Finding Information

### By Topic
- **Version Management**: MODULES_UNTOUCHED_INTEGRATION.md, QUICK_REFERENCE.md
- **Build Configuration**: IMPLEMENTATION_SUMMARY.md, GRADLE_TMP_PATHS.md
- **Migration**: GRADLE_APACHE_COMPARISON.md, CHANGELOG_*.md
- **Troubleshooting**: QUICK_REFERENCE.md, MODULES_UNTOUCHED_INTEGRATION.md

### By User Type
- **End Users**: QUICK_REFERENCE.md, FEATURE_SUMMARY.md
- **Developers**: IMPLEMENTATION_SUMMARY.md, GRADLE_APACHE_COMPARISON.md
- **Maintainers**: All CHANGELOG_*.md files

## 📞 Support

For issues or questions:
1. Check the relevant documentation file
2. Review error messages in build output
3. Consult the troubleshooting sections
4. Check the modules-untouched repository
5. Report issues to the Bearsampp project

## 🔄 Updates

This documentation is maintained alongside the build system. When making changes to the build system:
1. Update relevant documentation files
2. Add entries to appropriate changelogs
3. Update this index if adding new documents
4. Keep examples and commands current

## 📄 License

This documentation is part of the module-bruno project and follows the same license as the main project.

---

**Last Updated**: 2025  
**Build System Version**: Gradle 7.0+  
**Module**: Bruno IDE for Bearsampp
