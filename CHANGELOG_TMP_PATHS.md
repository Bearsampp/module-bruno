# Changelog: Gradle Tmp Paths Update

## Summary

Updated the Gradle build configuration to organize temporary build files by bundle type, matching the Ant build system structure.

## Changes Made

### Path Structure Update

**Before:**
```
bearsampp-build/tmp/bundles_prep/bruno/
bearsampp-build/tmp/bundles_build/bruno/
```

**After:**
```
bearsampp-build/tmp/bundles_prep/tools/bruno/
bearsampp-build/tmp/bundles_build/tools/bruno/
```

### Code Changes in `build.gradle`

```groovy
// OLD
bundleTmpBuildPath = file("${buildTmpPath}/bundles_build/${bundleName}").absolutePath
bundleTmpPrepPath = file("${buildTmpPath}/bundles_prep/${bundleName}").absolutePath

// NEW
bundleTmpBuildPath = file("${buildTmpPath}/bundles_build/${bundleType}/${bundleName}").absolutePath
bundleTmpPrepPath = file("${buildTmpPath}/bundles_prep/${bundleType}/${bundleName}").absolutePath
```

## Benefits

1. **Better Organization**: Bundles are now organized by type (tools, apps, bins, etc.)
2. **Ant Compatibility**: Matches the exact structure used by Ant builds
3. **Scalability**: Easier to manage multiple bundle types in the same build directory
4. **Clarity**: Clear separation between different types of modules

## Example Directory Structure

For Bruno (a tool):
```
bearsampp-build/
└── tmp/
    ├── bundles_build/
    │   └── tools/
    │       └── bruno/
    │           └── bruno2.13.0/
    ├── bundles_prep/
    │   └── tools/
    │       └── bruno/
    │           └── bruno2.13.0/
    └── bundles_src/
```

For other module types (e.g., Apache as a bin):
```
bearsampp-build/
└── tmp/
    ├── bundles_build/
    │   ├── bins/
    │   │   └── apache/
    │   │       └── apache2.4.62/
    │   └── tools/
    │       └── bruno/
    │           └── bruno2.13.0/
    └── bundles_prep/
        ├── bins/
        │   └── apache/
        │       └── apache2.4.62/
        └── tools/
            └── bruno/
                └── bruno2.13.0/
```

## Verification

Run `gradle info` to see the updated paths:

```bash
gradle info
```

Output will show:
```
Tmp Prep:     E:\Bearsampp-development\bearsampp-build\tmp\bundles_prep\tools\bruno
Tmp Build:    E:\Bearsampp-development\bearsampp-build\tmp\bundles_build\tools\bruno
```

## Impact

- **No Breaking Changes**: Existing builds will continue to work
- **Automatic Directory Creation**: Gradle will create the type subdirectories as needed
- **Consistent with Ant**: Now follows the same pattern as Ant-based module builds

## Files Modified

1. `build.gradle` - Updated path definitions to include `${bundleType}`
2. `GRADLE_TMP_PATHS.md` - Updated documentation to reflect new structure

## Date

2025-02-09

## Related

- Issue: Request to match Ant build structure with bundle type organization
- Reference: `module-composer` Ant build configuration
