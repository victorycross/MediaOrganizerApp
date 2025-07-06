# Deployment Guide - Media Organizer

This guide covers the complete deployment process for the Media Organizer application, including building, signing, notarization, and distribution.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Build Configuration](#build-configuration)
3. [Code Signing](#code-signing)
4. [Notarization](#notarization)
5. [Distribution Methods](#distribution-methods)
6. [App Store Submission](#app-store-submission)
7. [Direct Distribution](#direct-distribution)
8. [Continuous Integration](#continuous-integration)
9. [Troubleshooting](#troubleshooting)

## Prerequisites

### Development Environment
- **Xcode 14.0+** with Command Line Tools
- **macOS 12.0+** for development and testing
- **Apple Developer Account** (for code signing and distribution)
- **Valid Certificates** (Developer ID or App Store Distribution)

### Required Certificates
- **Developer ID Application Certificate** (for direct distribution)
- **Mac App Store Distribution Certificate** (for App Store submission)
- **Developer ID Installer Certificate** (for installer packages)

### Provisioning Profiles
- **Developer ID Provisioning Profile** (for direct distribution)
- **Mac App Store Provisioning Profile** (for App Store submission)

## Build Configuration

### Project Settings

#### Target Configuration
```
Target: MediaOrganizerApp
Bundle Identifier: com.yourcompany.mediaorganizer
Version: 1.0.0
Build: 1
Deployment Target: macOS 12.0
Architectures: x86_64, arm64 (Universal)
```

#### Build Settings
```
Code Signing Style: Manual
Development Team: [Your Team ID]
Code Signing Identity: Developer ID Application
Provisioning Profile: [Your Profile]
```

#### Entitlements
Ensure the following entitlements are properly configured:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.security.app-sandbox</key>
    <true/>
    <key>com.apple.security.files.user-selected.read-write</key>
    <true/>
    <key>com.apple.security.files.downloads.read-write</key>
    <true/>
    <key>com.apple.security.network.client</key>
    <false/>
    <key>com.apple.security.print</key>
    <false/>
    <key>com.apple.security.device.camera</key>
    <false/>
    <key>com.apple.security.device.microphone</key>
    <false/>
    <key>com.apple.security.personal-information.location</key>
    <false/>
    <key>com.apple.security.personal-information.addressbook</key>
    <false/>
    <key>com.apple.security.personal-information.calendars</key>
    <false/>
</dict>
</plist>
```

### Build Configurations

#### Debug Configuration
```
Optimization Level: None (-O0)
Swift Compilation Mode: Incremental
Debug Information Format: DWARF with dSYM File
Preprocessor Macros: DEBUG=1
```

#### Release Configuration
```
Optimization Level: Optimize for Speed (-O)
Swift Compilation Mode: Whole Module Optimization
Debug Information Format: DWARF with dSYM File
Strip Debug Symbols: Yes
```

### Build Scripts

#### Pre-build Script
```bash
#!/bin/bash
# Pre-build validation script

echo "Starting pre-build validation..."

# Check for required certificates
security find-identity -v -p codesigning | grep "Developer ID Application" || {
    echo "Error: Developer ID Application certificate not found"
    exit 1
}

# Validate entitlements
if [ ! -f "${PROJECT_DIR}/MediaOrganizerApp/Resources/MediaOrganizer.entitlements" ]; then
    echo "Error: Entitlements file not found"
    exit 1
fi

# Check version consistency
VERSION=$(defaults read "${PROJECT_DIR}/MediaOrganizerApp/Resources/Info.plist" CFBundleShortVersionString)
BUILD=$(defaults read "${PROJECT_DIR}/MediaOrganizerApp/Resources/Info.plist" CFBundleVersion)

echo "Building version ${VERSION} (${BUILD})"
```

#### Post-build Script
```bash
#!/bin/bash
# Post-build validation script

echo "Starting post-build validation..."

APP_PATH="${BUILT_PRODUCTS_DIR}/${PRODUCT_NAME}.app"

# Verify code signing
codesign --verify --verbose "${APP_PATH}" || {
    echo "Error: Code signing verification failed"
    exit 1
}

# Check entitlements
codesign -d --entitlements :- "${APP_PATH}" | grep "com.apple.security.app-sandbox" || {
    echo "Error: Sandbox entitlement not found"
    exit 1
}

echo "Post-build validation completed successfully"
```

## Code Signing

### Manual Code Signing

#### Sign the Application
```bash
# Sign the main application
codesign --force --options runtime --sign "Developer ID Application: Your Name (TEAM_ID)" \
    --entitlements MediaOrganizerApp/Resources/MediaOrganizer.entitlements \
    MediaOrganizerApp.app

# Verify signing
codesign --verify --verbose MediaOrganizerApp.app
```

#### Sign Embedded Frameworks (if any)
```bash
# Sign any embedded frameworks first
find MediaOrganizerApp.app -name "*.framework" -exec \
    codesign --force --options runtime --sign "Developer ID Application: Your Name (TEAM_ID)" {} \;
```

### Automated Code Signing

#### Xcode Build Settings
```
Code Signing Style: Automatic
Development Team: [Your Team ID]
Code Signing Identity: Apple Development (for development)
                      Developer ID Application (for distribution)
```

#### Command Line Build
```bash
# Build with automatic signing
xcodebuild -scheme MediaOrganizerApp \
    -configuration Release \
    -derivedDataPath ./build \
    CODE_SIGN_STYLE=Automatic \
    DEVELOPMENT_TEAM="YOUR_TEAM_ID" \
    CODE_SIGN_IDENTITY="Developer ID Application"
```

## Notarization

### Prepare for Notarization

#### Create App-Specific Password
1. Sign in to [appleid.apple.com](https://appleid.apple.com)
2. Go to Security section
3. Generate an app-specific password for notarization
4. Store the password securely

#### Configure Keychain
```bash
# Store credentials in keychain
xcrun notarytool store-credentials "notarization-profile" \
    --apple-id "your-apple-id@example.com" \
    --team-id "YOUR_TEAM_ID" \
    --password "app-specific-password"
```

### Notarization Process

#### Create Archive
```bash
# Create a zip archive for notarization
ditto -c -k --keepParent MediaOrganizerApp.app MediaOrganizerApp.zip
```

#### Submit for Notarization
```bash
# Submit to Apple for notarization
xcrun notarytool submit MediaOrganizerApp.zip \
    --keychain-profile "notarization-profile" \
    --wait
```

#### Check Notarization Status
```bash
# Check status (if not using --wait)
xcrun notarytool info SUBMISSION_ID \
    --keychain-profile "notarization-profile"
```

#### Staple the Ticket
```bash
# Staple the notarization ticket to the app
xcrun stapler staple MediaOrganizerApp.app

# Verify stapling
xcrun stapler validate MediaOrganizerApp.app
```

### Notarization Script
```bash
#!/bin/bash
# Automated notarization script

APP_NAME="MediaOrganizerApp"
APP_PATH="${APP_NAME}.app"
ZIP_PATH="${APP_NAME}.zip"
KEYCHAIN_PROFILE="notarization-profile"

echo "Starting notarization process for ${APP_NAME}..."

# Create archive
echo "Creating archive..."
ditto -c -k --keepParent "${APP_PATH}" "${ZIP_PATH}"

# Submit for notarization
echo "Submitting for notarization..."
SUBMISSION_ID=$(xcrun notarytool submit "${ZIP_PATH}" \
    --keychain-profile "${KEYCHAIN_PROFILE}" \
    --output-format json | jq -r '.id')

if [ -z "$SUBMISSION_ID" ]; then
    echo "Error: Failed to submit for notarization"
    exit 1
fi

echo "Submission ID: ${SUBMISSION_ID}"

# Wait for completion
echo "Waiting for notarization to complete..."
xcrun notarytool wait "${SUBMISSION_ID}" \
    --keychain-profile "${KEYCHAIN_PROFILE}"

# Check if successful
STATUS=$(xcrun notarytool info "${SUBMISSION_ID}" \
    --keychain-profile "${KEYCHAIN_PROFILE}" \
    --output-format json | jq -r '.status')

if [ "$STATUS" = "Accepted" ]; then
    echo "Notarization successful!"
    
    # Staple the ticket
    echo "Stapling notarization ticket..."
    xcrun stapler staple "${APP_PATH}"
    
    echo "Notarization process completed successfully"
else
    echo "Notarization failed with status: ${STATUS}"
    
    # Get detailed log
    xcrun notarytool log "${SUBMISSION_ID}" \
        --keychain-profile "${KEYCHAIN_PROFILE}"
    
    exit 1
fi

# Clean up
rm "${ZIP_PATH}"
```

## Distribution Methods

### App Store Distribution

#### Prepare for App Store
1. **Create App Store Connect Record**
   - App name and bundle identifier
   - App description and metadata
   - Screenshots and app preview
   - Pricing and availability

2. **Build with App Store Configuration**
   ```bash
   xcodebuild -scheme MediaOrganizerApp \
       -configuration Release \
       -derivedDataPath ./build \
       CODE_SIGN_STYLE=Automatic \
       DEVELOPMENT_TEAM="YOUR_TEAM_ID" \
       CODE_SIGN_IDENTITY="3rd Party Mac Developer Application"
   ```

3. **Create Archive**
   - Product → Archive in Xcode
   - Or use command line: `xcodebuild archive`

4. **Upload to App Store Connect**
   - Use Xcode Organizer
   - Or use `xcrun altool` command

### Direct Distribution

#### Create DMG Installer
```bash
#!/bin/bash
# Create DMG installer script

APP_NAME="Media Organizer"
APP_PATH="MediaOrganizerApp.app"
DMG_NAME="MediaOrganizer-1.0.0.dmg"
VOLUME_NAME="Media Organizer Installer"
SOURCE_FOLDER="dmg_source"

# Create source folder
mkdir -p "${SOURCE_FOLDER}"
cp -R "${APP_PATH}" "${SOURCE_FOLDER}/"

# Create Applications symlink
ln -s /Applications "${SOURCE_FOLDER}/Applications"

# Create DMG
hdiutil create -volname "${VOLUME_NAME}" \
    -srcfolder "${SOURCE_FOLDER}" \
    -ov -format UDZO \
    "${DMG_NAME}"

# Clean up
rm -rf "${SOURCE_FOLDER}"

echo "DMG created: ${DMG_NAME}"
```

#### Sign the DMG
```bash
# Sign the DMG file
codesign --force --sign "Developer ID Application: Your Name (TEAM_ID)" \
    MediaOrganizer-1.0.0.dmg

# Verify DMG signing
codesign --verify --verbose MediaOrganizer-1.0.0.dmg
```

#### Notarize the DMG
```bash
# Submit DMG for notarization
xcrun notarytool submit MediaOrganizer-1.0.0.dmg \
    --keychain-profile "notarization-profile" \
    --wait

# Staple the DMG
xcrun stapler staple MediaOrganizer-1.0.0.dmg
```

### Package Installer

#### Create PKG Installer
```bash
# Build installer package
productbuild --component MediaOrganizerApp.app /Applications \
    --sign "Developer ID Installer: Your Name (TEAM_ID)" \
    MediaOrganizer-1.0.0.pkg
```

#### Notarize PKG
```bash
# Submit PKG for notarization
xcrun notarytool submit MediaOrganizer-1.0.0.pkg \
    --keychain-profile "notarization-profile" \
    --wait

# Staple the PKG
xcrun stapler staple MediaOrganizer-1.0.0.pkg
```

## App Store Submission

### Pre-submission Checklist

- [ ] App follows App Store Review Guidelines
- [ ] All required metadata provided
- [ ] Screenshots and app preview created
- [ ] Privacy policy URL provided (if applicable)
- [ ] App tested on multiple macOS versions
- [ ] Accessibility features tested
- [ ] Performance optimized
- [ ] Memory leaks checked

### Submission Process

#### 1. Create Archive
```bash
# Archive for App Store submission
xcodebuild -scheme MediaOrganizerApp \
    -configuration Release \
    -archivePath ./MediaOrganizerApp.xcarchive \
    archive
```

#### 2. Validate Archive
```bash
# Validate before submission
xcrun altool --validate-app \
    --file MediaOrganizerApp.xcarchive \
    --type macos \
    --username "your-apple-id@example.com" \
    --password "@keychain:altool-password"
```

#### 3. Upload to App Store Connect
```bash
# Upload to App Store Connect
xcrun altool --upload-app \
    --file MediaOrganizerApp.xcarchive \
    --type macos \
    --username "your-apple-id@example.com" \
    --password "@keychain:altool-password"
```

#### 4. Submit for Review
1. Go to App Store Connect
2. Select your app
3. Create new version
4. Fill in version information
5. Submit for review

### App Store Metadata

#### Required Information
- **App Name**: Media Organizer
- **Subtitle**: Intelligent Media File Organization
- **Description**: Comprehensive app description
- **Keywords**: media, organizer, files, photos, videos
- **Category**: Productivity
- **Price**: Free or Paid

#### Screenshots Requirements
- **Required Sizes**: 1280x800, 1440x900, 2560x1600, 2880x1800
- **Format**: PNG or JPEG
- **Content**: Show key features and interface

## Continuous Integration

### GitHub Actions Workflow

```yaml
name: Build and Deploy

on:
  push:
    tags:
      - 'v*'
  workflow_dispatch:

jobs:
  build:
    runs-on: macos-latest
    
    steps:
    - name: Checkout
      uses: actions/checkout@v3
    
    - name: Setup Xcode
      uses: maxim-lobanov/setup-xcode@v1
      with:
        xcode-version: '14.0'
    
    - name: Import Certificates
      env:
        CERTIFICATE_P12: ${{ secrets.CERTIFICATE_P12 }}
        CERTIFICATE_PASSWORD: ${{ secrets.CERTIFICATE_PASSWORD }}
      run: |
        echo "$CERTIFICATE_P12" | base64 --decode > certificate.p12
        security create-keychain -p "" build.keychain
        security import certificate.p12 -k build.keychain -P "$CERTIFICATE_PASSWORD" -T /usr/bin/codesign
        security list-keychains -s build.keychain
        security default-keychain -s build.keychain
        security unlock-keychain -p "" build.keychain
        security set-key-partition-list -S apple-tool:,apple: -s -k "" build.keychain
    
    - name: Build Application
      run: |
        xcodebuild -scheme MediaOrganizerApp \
          -configuration Release \
          -derivedDataPath ./build \
          CODE_SIGN_STYLE=Manual \
          DEVELOPMENT_TEAM="${{ secrets.TEAM_ID }}" \
          CODE_SIGN_IDENTITY="Developer ID Application"
    
    - name: Create DMG
      run: |
        ./scripts/create_dmg.sh
    
    - name: Notarize
      env:
        APPLE_ID: ${{ secrets.APPLE_ID }}
        APPLE_PASSWORD: ${{ secrets.APPLE_PASSWORD }}
        TEAM_ID: ${{ secrets.TEAM_ID }}
      run: |
        ./scripts/notarize.sh
    
    - name: Upload Release
      uses: actions/upload-artifact@v3
      with:
        name: MediaOrganizer-Release
        path: |
          *.dmg
          *.pkg
```

### Jenkins Pipeline

```groovy
pipeline {
    agent { label 'macos' }
    
    environment {
        KEYCHAIN_PASSWORD = credentials('keychain-password')
        APPLE_ID = credentials('apple-id')
        APPLE_PASSWORD = credentials('apple-password')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh '''
                    xcodebuild -scheme MediaOrganizerApp \
                        -configuration Release \
                        -derivedDataPath ./build
                '''
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                    xcodebuild test -scheme MediaOrganizerApp \
                        -destination 'platform=macOS'
                '''
            }
        }
        
        stage('Package') {
            steps {
                sh './scripts/create_dmg.sh'
            }
        }
        
        stage('Notarize') {
            steps {
                sh './scripts/notarize.sh'
            }
        }
        
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '*.dmg, *.pkg', fingerprint: true
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
```

## Troubleshooting

### Common Issues

#### Code Signing Errors

**Problem**: "No signing certificate found"
**Solution**:
```bash
# List available certificates
security find-identity -v -p codesigning

# Import certificate if missing
security import certificate.p12 -k ~/Library/Keychains/login.keychain
```

**Problem**: "Provisioning profile doesn't match"
**Solution**:
1. Download latest provisioning profile
2. Double-click to install
3. Verify in Xcode preferences

#### Notarization Failures

**Problem**: "Invalid bundle structure"
**Solution**:
```bash
# Check bundle structure
find MediaOrganizerApp.app -type f -exec file {} \;

# Ensure all binaries are signed
find MediaOrganizerApp.app -type f \( -name "*.dylib" -o -name "*.framework" \) \
    -exec codesign --verify {} \;
```

**Problem**: "Hardened runtime issues"
**Solution**:
- Add `--options runtime` to codesign command
- Review entitlements for hardened runtime compatibility

#### Build Errors

**Problem**: "Swift compiler error"
**Solution**:
1. Clean build folder (⌘+Shift+K)
2. Delete derived data
3. Restart Xcode

**Problem**: "Missing dependencies"
**Solution**:
```bash
# Reset package dependencies
rm -rf .build
xcodebuild -resolvePackageDependencies
```

### Debug Commands

#### Verify Code Signing
```bash
# Check signing details
codesign -dv --verbose=4 MediaOrganizerApp.app

# Verify entitlements
codesign -d --entitlements :- MediaOrganizerApp.app

# Check for issues
spctl -a -vvv MediaOrganizerApp.app
```

#### Check Notarization Status
```bash
# Get notarization history
xcrun notarytool history --keychain-profile "notarization-profile"

# Get detailed log
xcrun notarytool log SUBMISSION_ID --keychain-profile "notarization-profile"
```

#### Validate Distribution
```bash
# Check Gatekeeper status
spctl --assess --verbose MediaOrganizerApp.app

# Verify stapling
stapler validate MediaOrganizerApp.app

# Check quarantine attributes
xattr -l MediaOrganizerApp.app
```

### Support Resources

- [Apple Developer Documentation](https://developer.apple.com/documentation/)
- [Code Signing Guide](https://developer.apple.com/library/archive/documentation/Security/Conceptual/CodeSigningGuide/)
- [Notarization Documentation](https://developer.apple.com/documentation/security/notarizing_macos_software_before_distribution)
- [App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/)

---

This deployment guide should be updated as Apple's tools and requirements evolve. Always refer to the latest Apple documentation for the most current information.

