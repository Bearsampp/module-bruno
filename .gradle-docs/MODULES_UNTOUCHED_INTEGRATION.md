# Modules-Untouched Integration

## Overview

The build system now automatically fetches Bruno binaries from the [modules-untouched repository](https://github.com/Bearsampp/modules-untouched) when a version is not found in `releases.properties`.

## How It Works

### Automatic Fallback Mechanism

When building a release with `gradle release -PbundleVersion=X.X.X`:

1. **First Check**: The build system checks if the version exists in `releases.properties`
2. **Fallback to modules-untouched**: If not found, it automatically:
   - Fetches `bruno.properties` from: `https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/bruno.properties`
   - Looks up the version in the fetched properties file
   - If found, uses the URL from bruno.properties
   - If not found in bruno.properties, constructs URL using standard format
3. **Download**: Downloads the archive from the determined URL
4. **Cache**: Downloaded files are cached in `bearsampp-build/tmp/downloads/bruno/`
5. **Extract**: Extracts to `bearsampp-build/tmp/extract/bruno/{version}/`
6. **Build**: Proceeds with normal build process

### URL Resolution Priority

The system resolves download URLs in this order:

1. **Local releases.properties** (highest priority)
   - File: `releases.properties` in module-bruno project
   - Example: `2.13.0 = https://github.com/Bearsampp/module-bruno/releases/download/...`

2. **modules-untouched bruno.properties**
   - URL: `https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/bruno.properties`
   - Example: `2.15.0 = https://github.com/Bearsampp/modules-untouched/releases/download/bruno-2.15.0/bruno-2.15.0-win64.7z`

3. **Standard URL format** (fallback)
   - Pattern: `https://github.com/Bearsampp/modules-untouched/releases/download/bruno-{version}/bruno-{version}-win64.7z`
   - Used when version not found in either properties file

### URL Format

The standard format expects releases in modules-untouched to follow this pattern:
```
Release Tag: bruno-{version}
Asset Name:  bruno-{version}-win64.7z
```

For example, for Bruno version 2.15.0:
- Release Tag: `bruno-2.15.0`
- Download URL: `https://github.com/Bearsampp/modules-untouched/releases/download/bruno-2.15.0/bruno-2.15.0-win64.7z`

## Usage Examples

### Building a New Version

If you want to build Bruno version 2.15.0 that doesn't exist in `releases.properties`:

```bash
# Create the version directory structure
mkdir bin/bruno2.15.0

# Add bearsampp.conf to the directory
# (Copy from another version and update as needed)

# Run the build - it will automatically fetch from modules-untouched
gradle release -PbundleVersion=2.15.0
```

### Expected Output

#### Scenario 1: Version found in modules-untouched bruno.properties

```
Bruno binaries not found
Downloading Bruno 2.15.0...

Version 2.15.0 not found in releases.properties
Fetching from modules-untouched repository...
Checking modules-untouched repository...
Fetching bruno.properties from modules-untouched...
  https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/bruno.properties
  Successfully loaded 25 versions from modules-untouched
Found version 2.15.0 in modules-untouched bruno.properties
Downloading from:
  https://github.com/Bearsampp/modules-untouched/releases/download/bruno-2.15.0/bruno-2.15.0-win64.7z
  Downloading to: E:\Bearsampp-development\bearsampp-build\tmp\downloads\bruno\bruno-2.15.0-win64.7z
  Download complete from modules-untouched
  Extracting archive...
  Extraction complete
  Found Bruno directory: Bruno

NOTE: Version 2.15.0 was downloaded from modules-untouched repository
      Consider adding it to releases.properties for future builds
```

#### Scenario 2: Version not in bruno.properties (using standard format)

```
Bruno binaries not found
Downloading Bruno 2.16.0...

Version 2.16.0 not found in releases.properties
Fetching from modules-untouched repository...
Checking modules-untouched repository...
Fetching bruno.properties from modules-untouched...
  https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/bruno.properties
  Successfully loaded 25 versions from modules-untouched
Version 2.16.0 not found in modules-untouched bruno.properties
Attempting to construct URL based on standard format...
  https://github.com/Bearsampp/modules-untouched/releases/download/bruno-2.16.0/bruno-2.16.0-win64.7z
  Downloading to: E:\Bearsampp-development\bearsampp-build\tmp\downloads\bruno\bruno-2.16.0-win64.7z
  Download complete from modules-untouched
  Extracting archive...
  Extraction complete
  Found Bruno directory: Bruno

NOTE: Version 2.16.0 was downloaded from modules-untouched repository
      Consider adding it to releases.properties for future builds
```

## Error Handling

If the download fails, you'll see a detailed error message:

```
Failed to download from modules-untouched: [error details]

Tried URL: https://github.com/Bearsampp/modules-untouched/releases/download/bruno-2.15.0/bruno-2.15.0-win64.7z

Please verify:
1. Version 2.15.0 exists in modules-untouched repository
2. The release format matches: bruno-{version}/bruno-{version}-win64.7z
3. You have internet connectivity
```

## Caching

Downloaded files are cached to avoid repeated downloads:

- **Download Cache**: `bearsampp-build/tmp/downloads/bruno/`
- **Extract Cache**: `bearsampp-build/tmp/extract/bruno/{version}/`

If you run the build again for the same version, it will use the cached files:

```
Using cached file: E:\Bearsampp-development\bearsampp-build\tmp\downloads\bruno\bruno-2.15.0-win64.7z
```

## Adding to releases.properties

After successfully building a version from modules-untouched, you may want to add it to `releases.properties` for future reference:

```properties
# Add to releases.properties
2.15.0 = https://github.com/Bearsampp/module-bruno/releases/download/2025.XX.XX/bearsampp-bruno-2.15.0-2025.XX.XX.7z
```

This way, future builds will use the official module-bruno release instead of modules-untouched.

## Benefits

1. **Automatic**: No manual download required for new versions
2. **Seamless**: Works transparently with existing build process
3. **Cached**: Downloads are cached to avoid repeated fetches
4. **Fallback**: Provides a reliable source when releases.properties is not updated
5. **Flexible**: Supports both official releases and untouched sources

## Requirements

- Internet connectivity to download from GitHub
- 7-Zip installed for extracting .7z archives
- Proper directory structure in modules-untouched repository

## Troubleshooting

### Version Not Found in modules-untouched

If the version doesn't exist in modules-untouched:
1. Check the [modules-untouched releases](https://github.com/Bearsampp/modules-untouched/releases)
2. Verify the version number is correct
3. Manually download and place in `bin/bruno{version}/`

### Download Fails

If download fails:
1. Check internet connectivity
2. Verify the URL format matches expectations
3. Check if GitHub is accessible
4. Try downloading manually and placing in cache directory

### Extraction Fails

If extraction fails:
1. Verify 7-Zip is installed
2. Check the downloaded file is not corrupted
3. Try deleting the cached file and re-downloading

## Implementation Details

### Functions Added

1. **`versionExistsInReleases(String version)`**
   - Checks if a version exists in local releases.properties
   - Returns boolean

2. **`fetchModulesUntouchedProperties()`**
   - Fetches bruno.properties from modules-untouched repository
   - URL: `https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/bruno.properties`
   - Caches the file locally in `bearsampp-build/tmp/downloads/bruno/`
   - Returns Properties object or null if fetch fails
   - Handles errors gracefully with warnings

3. **`downloadFromModulesUntouched(String version, File destDir)`**
   - First attempts to fetch bruno.properties from modules-untouched
   - Looks up version in bruno.properties
   - Falls back to standard URL format if not found
   - Handles caching
   - Returns downloaded File

4. **Modified `downloadAndExtractBruno(String version, File destDir)`**
   - Now checks releases.properties first
   - Falls back to modules-untouched if not found
   - Provides informative messages
   - Maintains backward compatibility

## Future Enhancements

Possible improvements:
1. Support for alternative archive formats (zip, tar.gz)
2. Configurable URL patterns
3. Automatic version detection from modules-untouched
4. Integration with GitHub API for version listing
5. Automatic releases.properties updates
