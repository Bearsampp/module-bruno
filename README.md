# module-bruno
Bruno IDE for bearsampp project
<p align="center"><a href="https://bearsampp.com/contribute" target="_blank"><img width="250" src="img/Bearsampp-logo.svg"></a></p>

[![GitHub release](https://img.shields.io/github/release/bearsampp/module-bruno.svg?style=flat-square)](https://github.com/bearsampp/module-bruno/releases/latest)
![Total downloads](https://img.shields.io/github/downloads/bearsampp/module-bruno/total.svg?style=flat-square)

This is a module of [Bearsampp project](https://github.com/bearsampp/bearsampp) involving PHP.

## Build System

This project uses Gradle for building releases. For complete build documentation, see [.gradle-docs/](.gradle-docs/).

### Quick Start

```bash
# Display build information
gradle info

# Build a specific version
gradle release -PbundleVersion=2.13.0

# List available versions
gradle listVersions

# Verify build environment
gradle verify
```

### Key Features

- ✅ Automatic download from modules-untouched repository
- ✅ Smart URL resolution with 3-tier fallback
- ✅ Efficient caching system
- ✅ Backward compatible with existing builds

For detailed documentation, see:
- [Quick Reference](.gradle-docs/QUICK_REFERENCE.md)
- [Modules-Untouched Integration](.gradle-docs/MODULES_UNTOUCHED_INTEGRATION.md)
- [Complete Documentation Index](.gradle-docs/README.md)

## Documentation and downloads

https://bearsampp.com/module/bruno

## Issues

Issues must be reported on [Bearsampp repository](https://github.com/bearsampp/bearsampp/issues).

## Statistics
![Alt](https://repobeats.axiom.co/api/embed/d4b9f82a31c270b2d20d93e7ccf77113ff5930f2.svg "Repobeats analytics image")
