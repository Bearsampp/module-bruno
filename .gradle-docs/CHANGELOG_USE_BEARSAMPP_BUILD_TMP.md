# Changelog: Use bearsampp-build/tmp Instead of Local build/ Folder

## Summary

Updated the Gradle build configuration to use `bearsampp-build/tmp` for all temporary files (downloads, extractions) instead of the local `module-bruno/build/` folder. This centralizes all build artifacts in the shared bearsampp-build directory.

## Changes Made

### 1. New Path Variables Added

```groovy
// Download and extract paths - use bearsampp-build/tmp instead of local build/
bundleTmpDownloadPath = file("${buildTmpPath}/downloads/${bundleName}").absolutePath
bundleTmpExtractPath = file("${buildTmpPath}/extract/${bundleName}").absolutePath
```

### 2. Updated Download Function

**Before:**
```groovy
def downloadDir = file("${buildDir}/downloads")
def extractDir = file("${buildDir}/extract")
```

**After:**
```groovy
def downloadDir = file(bundleTmpDownloadPath)
def extractDir = file(bundleTmpExtractPath)
```

### 3. Updated Release Task

**Before:**
```groovy
def buildExtractPath = file("${buildDir}/extract/${bundleVersion}")
bundleSrcFinal = downloadAndExtractBruno(bundleVersion, file("${buildDir}/extract"))
```

**After:**
```groovy
def tmpExtractPath = file("${bundleTmpExtractPath}/${bundleVersion}")
bundleSrcFinal = downloadAndExtractBruno(bundleVersion, file(bundleTmpExtractPath))
```

### 4. Updated Info Task

Added display of new paths:
```
Tmp Download: E:\Bearsampp-development\bearsampp-build\tmp\downloads\bruno
Tmp Extract:  E:\Bearsampp-development\bearsampp-build\tmp\extract\bruno
```

## New Directory Structure

### Complete bearsampp-build/tmp Layout

```
bearsampp-build/
└── tmp/
    ├── bundles_build/
    │   └── tools/
    │       └── bruno/
    ├── bundles_prep/
    │   └── tools/
    │       └── bruno/
    │           └── bruno2.13.0/    # Prepared files ready for archiving
    ├── bundles_src/                # Reserved for future use
    ├── downloads/
    │   └── bruno/
    │       └── bruno_2.13.0_win_x64.7z    # Downloaded archives
    └── extract/
        └── bruno/
            └── 2.13.0/
                └── Bruno-2.13.0/   # Extracted Bruno binaries
```

## Benefits

1. **Centralized Storage**: All temporary build files in one location
2. **Shared Caching**: Downloaded files can be reused across builds
3. **Easier Cleanup**: Single location to clean all temporary files
4. **Consistency**: Matches Ant build system structure
5. **No Local Clutter**: Keeps module directories clean

## Path Comparison

| Purpose | Old Path | New Path |
|---------|----------|----------|
| Downloads | `module-bruno/build/downloads/` | `bearsampp-build/tmp/downloads/bruno/` |
| Extractions | `module-bruno/build/extract/` | `bearsampp-build/tmp/extract/bruno/` |
| Preparation | `module-bruno/build/prep/` | `bearsampp-build/tmp/bundles_prep/tools/bruno/` |

## Example Build Flow

When building Bruno 2.13.0:

1. **Download**: `bearsampp-build/tmp/downloads/bruno/bruno_2.13.0_win_x64.7z`
2. **Extract**: `bearsampp-build/tmp/extract/bruno/2.13.0/Bruno-2.13.0/`
3. **Prepare**: `bearsampp-build/tmp/bundles_prep/tools/bruno/bruno2.13.0/`
4. **Archive**: `bearsampp-build/tools/bruno/2025.10.14/bearsampp-bruno-2.13.0-2025.10.14.7z`

## Caching Behavior

- Downloaded archives are cached in `bearsampp-build/tmp/downloads/bruno/`
- Extracted files are cached in `bearsampp-build/tmp/extract/bruno/{version}/`
- If files exist, they are reused instead of re-downloading
- Message displayed: "Using cached Bruno binaries from bearsampp-build/tmp"

## Clean Task

The `gradle clean` task still only cleans the local `build/` directory (which now contains only Gradle's internal files like `.gradle` cache). To clean the bearsampp-build/tmp files, you would need to manually delete them or create a separate task.

## Verification

Run `gradle info` to see all paths:

```bash
gradle info
```

Output includes:
```
Tmp Download: E:\Bearsampp-development\bearsampp-build\tmp\downloads\bruno
Tmp Extract:  E:\Bearsampp-development\bearsampp-build\tmp\extract\bruno
```

## Impact

- **No Breaking Changes**: Existing builds continue to work
- **Automatic Migration**: Old `build/` folder files are no longer used
- **Shared Resources**: Multiple modules can share the same download cache
- **Better Organization**: Clear separation of concerns

## Files Modified

1. `build.gradle` - Updated all download/extract paths to use bearsampp-build/tmp
2. `GRADLE_TMP_PATHS.md` - Documentation (needs update)
3. `CHANGELOG_USE_BEARSAMPP_BUILD_TMP.md` - This file

## Date

2025-02-09

## Related Changes

- Previous: Added bundle type to tmp paths (`bundles_prep/tools/bruno/`)
- This change: Moved downloads and extractions to bearsampp-build/tmp
