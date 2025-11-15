# Quick Reference: Modules-Untouched Integration

## TL;DR

When building a Bruno version not in `releases.properties`, the system automatically:
1. Fetches `bruno.properties` from modules-untouched
2. Uses the URL from bruno.properties if found
3. Falls back to standard URL format if not found
4. Downloads and builds automatically

## URL Resolution Order

```
1. releases.properties (local)
   ↓ (if not found)
2. bruno.properties (modules-untouched)
   ↓ (if not found)
3. Standard URL format (constructed)
```

## Key URLs

- **bruno.properties**: `https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/bruno.properties`
- **Standard format**: `https://github.com/Bearsampp/modules-untouched/releases/download/bruno-{version}/bruno-{version}-win64.7z`

## Build Command

```bash
gradle release -PbundleVersion=X.X.X
```

## What Gets Cached

- **Properties file**: `bearsampp-build/tmp/downloads/bruno/bruno-untouched.properties`
- **Downloaded archives**: `bearsampp-build/tmp/downloads/bruno/`
- **Extracted files**: `bearsampp-build/tmp/extract/bruno/{version}/`

## Example Flow

```
Building version 2.15.0 (not in releases.properties)
  ↓
Fetch bruno.properties from modules-untouched
  ↓
Found 2.15.0 in bruno.properties
  ↓
Download from URL in bruno.properties
  ↓
Extract to cache
  ↓
Build package
  ✓ Success
```

## Common Scenarios

### New Version Available in modules-untouched
```bash
# Just build it - system handles everything
gradle release -PbundleVersion=2.15.0
```

### Version Not in Either Properties File
```bash
# System tries standard URL format
gradle release -PbundleVersion=2.16.0
# If release exists at standard location, it works
# Otherwise, you'll get a clear error message
```

### Cached Build
```bash
# First run: downloads
gradle release -PbundleVersion=2.15.0

# Second run: uses cache
gradle release -PbundleVersion=2.15.0
```

## Error Messages

### Version Not Found
```
Failed to download from modules-untouched: [error]
Tried URL: https://...

Please verify:
1. Version exists in modules-untouched repository
2. URL is correct in bruno.properties
3. You have internet connectivity
```

### Solution
1. Check [modules-untouched releases](https://github.com/Bearsampp/modules-untouched/releases)
2. Verify version number
3. Check internet connection
4. Try manual download if needed

## Manual Override

If automatic download fails, you can manually:

1. Download Bruno binaries
2. Extract to: `bin/bruno{version}/`
3. Add `bearsampp.conf`
4. Run build again

## Adding to releases.properties

After successful build from modules-untouched:

```properties
# Add to releases.properties for future reference
2.15.0 = https://github.com/Bearsampp/module-bruno/releases/download/2025.XX.XX/bearsampp-bruno-2.15.0-2025.XX.XX.7z
```

## Benefits

✅ Automatic download from modules-untouched  
✅ Smart URL resolution  
✅ Caching for efficiency  
✅ Graceful fallback  
✅ Clear error messages  
✅ No breaking changes  

## See Also

- `MODULES_UNTOUCHED_INTEGRATION.md` - Full documentation
- `IMPLEMENTATION_SUMMARY.md` - Technical details
- `build.gradle` - Implementation code
