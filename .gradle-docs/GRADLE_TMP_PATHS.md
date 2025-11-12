# Gradle Build - Ant-Compatible Temporary Paths

## Overview

The Gradle build configuration for module-bruno has been updated to use the same temporary directory structure as Ant builds, ensuring consistency across the Bearsampp build system.

## Path Structure

### Ant Build Paths (from module-composer)

When running `ant release` in module-composer, files are placed in:

```
bearsampp-build/
└── tmp/
    ├── bundles_build/{type}/{module-name}/    # Build staging area
    ├── bundles_prep/{type}/{module-name}/     # Preparation area
    └── bundles_src/                           # Source downloads
```

Where `{type}` is the bundle type (tools, apps, bins, etc.)

### Gradle Build Paths (module-bruno)

The Gradle build now uses the **same structure with type organization**:

```groovy
buildBasePath = file("${rootDir}/bearsampp-build").absolutePath
buildTmpPath = file("${buildBasePath}/tmp").absolutePath
bundleTmpBuildPath = file("${buildTmpPath}/bundles_build/${bundleType}/${bundleName}").absolutePath
bundleTmpPrepPath = file("${buildTmpPath}/bundles_prep/${bundleType}/${bundleName}").absolutePath
bundleTmpSrcPath = file("${buildTmpPath}/bundles_src").absolutePath
```

## Path Definitions

| Variable | Path | Purpose |
|----------|------|---------|
| `buildBasePath` | `E:/Bearsampp-development/bearsampp-build` | Root build directory |
| `buildTmpPath` | `E:/Bearsampp-development/bearsampp-build/tmp` | Temporary build files |
| `bundleTmpBuildPath` | `E:/Bearsampp-development/bearsampp-build/tmp/bundles_build/tools/bruno` | Build staging for bruno |
| `bundleTmpPrepPath` | `E:/Bearsampp-development/bearsampp-build/tmp/bundles_prep/tools/bruno` | Preparation area for bruno |
| `bundleTmpSrcPath` | `E:/Bearsampp-development/bearsampp-build/tmp/bundles_src` | Downloaded source files |

## How It Works

### During Release Build

1. **Preparation Phase** (`bundleTmpPrepPath`)
   - Files are copied to `bearsampp-build/tmp/bundles_prep/tools/bruno/{version}/`
   - Configuration files are merged
   - Ready for compression

2. **Build Phase** (`bundleTmpBuildPath`)
   - Reserved for future use (currently using prep path)
   - Can be used for intermediate build steps

3. **Source Downloads** (`bundleTmpSrcPath`)
   - Downloaded binaries stored in `bearsampp-build/tmp/bundles_src/`
   - Shared across all modules

4. **Final Output**
   - Archives created in `bearsampp-build/tools/bruno/{release}/`
   - Hash files generated alongside archives

## Example: Building Bruno 2.13.0

```bash
gradle release -PbundleVersion=2.13.0
```

**File Flow:**

1. Source: `module-bruno/bin/bruno2.13.0/` or download from releases.properties
2. Prep: `bearsampp-build/tmp/bundles_prep/tools/bruno/bruno2.13.0/`
3. Archive: `bearsampp-build/tools/bruno/2025.10.14/bearsampp-bruno-2.13.0-2025.10.14.7z`
4. Hashes: `.md5`, `.sha1`, `.sha256`, `.sha512` files

## Verification

To verify the paths are correctly configured:

```bash
gradle info
```

This will display all configured paths including the new tmp directories.

## Benefits

1. **Consistency**: Same directory structure as Ant builds
2. **Compatibility**: Works seamlessly with existing Bearsampp build infrastructure
3. **Clarity**: Clear separation of build stages (prep, build, src)
4. **Maintainability**: Easy to understand and debug build process

## Comparison with Previous Gradle Setup

### Before (Local Build Directory)

```
module-bruno/
└── build/
    ├── tmp/           # Local to module
    └── prep/          # Local to module
```

### After (Shared Build Directory with Type Organization)

```
bearsampp-build/
└── tmp/
    ├── bundles_build/tools/bruno/    # Shared location with type
    ├── bundles_prep/tools/bruno/     # Shared location with type
    └── bundles_src/                  # Shared location
```

## Migration Notes

- The change is **backward compatible** - existing builds will continue to work
- The `bearsampp-build/tmp` directory is automatically created if it doesn't exist
- Local `build/` directory is still used for Gradle's internal files (downloads, extract cache)
- Final archives are placed in `bearsampp-build/tools/bruno/{release}/` (unchanged)

## Related Files

- `build.gradle` - Main build configuration with path definitions
- `build.properties` - Bundle configuration (name, release, type, format)
- `releases.properties` - Download URLs for Bruno versions

## See Also

- Ant build configuration: `E:/Bearsampp-development/dev/build/build-commons.xml`
- Ant bundle configuration: `E:/Bearsampp-development/dev/build/build-bundle.xml`
- Module composer example: `E:/Bearsampp-development/module-composer/build.xml`
