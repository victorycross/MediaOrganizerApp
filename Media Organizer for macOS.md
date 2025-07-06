# Media Organizer for macOS

A powerful, intelligent media file organizer built with SwiftUI that helps you discover, categorize, and organize your photos, videos, and audio files with ease.

## Features

### 🔍 Intelligent File Discovery
- **Advanced Scanning**: Recursively scan directories with customizable depth limits
- **Smart Filtering**: Filter by file type (images, videos, audio) with support for all common formats
- **Hidden File Support**: Option to include or exclude hidden files and directories
- **Batch Processing**: Efficient processing of large file collections
- **Progress Tracking**: Real-time progress updates with estimated completion times

### 📁 Flexible Organization
- **Multiple Strategies**: Organize by type, date, size, or keep files flat
- **Conflict Resolution**: Smart handling of duplicate files and naming conflicts
- **Dry Run Mode**: Preview organization operations before execution
- **Backup Creation**: Optional backup of original files before moving
- **Integrity Verification**: Checksum validation to ensure file integrity

### 🎯 Advanced Features
- **iCloud Drive Integration**: Seamless integration with iCloud Drive files
- **Metadata Extraction**: Extract and display comprehensive file metadata
- **Duplicate Detection**: Identify and handle duplicate files intelligently
- **Job Management**: Queue and manage multiple organization jobs
- **Real-time Monitoring**: Live progress tracking and status updates

### 🔒 Security & Privacy
- **Sandbox Compliant**: Fully compliant with macOS App Sandbox requirements
- **Security-Scoped Bookmarks**: Secure file access with user permission
- **Privacy Focused**: No data collection or external communication
- **Permission Management**: Granular control over file access permissions

### 💻 Professional Interface
- **Native macOS Design**: Built with SwiftUI for a native macOS experience
- **Menu Bar Integration**: Optional menu bar item for quick access
- **Keyboard Shortcuts**: Comprehensive keyboard shortcuts for power users
- **Notifications**: System notifications for operation completion and errors
- **Dark Mode Support**: Full support for macOS Dark Mode

## System Requirements

- **macOS**: 12.0 (Monterey) or later
- **Architecture**: Intel x64 or Apple Silicon (Universal Binary)
- **Memory**: 4GB RAM minimum, 8GB recommended
- **Storage**: 100MB for application, additional space for file operations

## Installation

### From Release (Recommended)
1. Download the latest release from the [Releases](https://github.com/your-repo/media-organizer/releases) page
2. Open the downloaded `.dmg` file
3. Drag "Media Organizer" to your Applications folder
4. Launch the application from Applications or Spotlight

### Building from Source
```bash
# Clone the repository
git clone https://github.com/your-repo/media-organizer.git
cd media-organizer

# Open in Xcode
open MediaOrganizerApp.xcodeproj

# Build and run (⌘+R)
```

## Quick Start

### First Launch
1. **Grant Permissions**: The app will request access to scan your files
2. **Choose Directories**: Select the folders you want to organize
3. **Configure Settings**: Set your preferred organization strategy
4. **Start Scanning**: Begin discovering your media files

### Basic Workflow
1. **Scan**: Use the scan view to discover media files in your chosen directories
2. **Review**: Browse discovered files in the files view with preview capabilities
3. **Organize**: Configure organization settings and preview changes with dry run
4. **Execute**: Run the organization job to move files to their new locations
5. **Monitor**: Track progress in the jobs view and review logs for details

## User Guide

### Scanning for Files

#### Basic Scan
1. Navigate to the **Scan** tab
2. Click **Add Directory** to select folders to scan
3. Configure scan options:
   - **Include Hidden Files**: Scan hidden files and directories
   - **Max Depth**: Limit how deep to scan into subdirectories
   - **File Types**: Choose which media types to include
4. Click **Start Scan** to begin discovery

#### Advanced Options
- **Batch Size**: Number of files to process simultaneously
- **Extract Metadata**: Extract detailed file information during scan
- **Follow Symlinks**: Follow symbolic links during scanning

### Organizing Files

#### Organization Strategies
- **By Type**: Group files by media type (Images, Videos, Audio)
- **By Date**: Organize by creation or modification date
- **By Size**: Group files by size ranges
- **Flat**: Keep all files in a single directory

#### Conflict Resolution
- **Skip**: Skip files that would cause conflicts
- **Overwrite**: Replace existing files with new ones
- **Rename**: Automatically rename conflicting files
- **Keep Both**: Preserve both files with unique names

#### Dry Run
Always use dry run mode to preview changes before executing:
1. Configure your organization settings
2. Click **Dry Run** to preview operations
3. Review the proposed changes in the results view
4. Adjust settings if needed
5. Click **Execute** to perform the actual organization

### Job Management

#### Monitoring Jobs
- View active jobs in the **Jobs** tab
- Track progress with real-time updates
- Pause, resume, or cancel jobs as needed
- View detailed job information and logs

#### Job History
- Review completed jobs and their results
- Access detailed logs for troubleshooting
- Export job reports for record keeping

### Settings and Preferences

#### General Settings
- **Launch at Login**: Start the app automatically
- **Menu Bar**: Show menu bar item for quick access
- **Notifications**: Configure notification preferences
- **Theme**: Choose light, dark, or system theme

#### Performance Settings
- **Concurrent Operations**: Number of simultaneous operations
- **Memory Cache**: Cache size for improved performance
- **Batch Processing**: Optimize for large file collections

#### Security Settings
- **File Access**: Manage security-scoped bookmarks
- **Backup Options**: Configure backup behavior
- **Verification**: Enable file integrity checking

## Keyboard Shortcuts

### File Operations
- `⌘N` - New Scan
- `⌘⇧Q` - Quick Scan (Global)
- `⌘O` - Organize Files
- `⌘R` - Refresh Current View

### Navigation
- `⌘1` - Scan View
- `⌘2` - Files View
- `⌘3` - Organize View
- `⌘4` - Jobs View
- `⌘5` - Logs View
- `⌘6` - Statistics View

### Operations
- `⌘Space` - Pause All Operations
- `⌘Escape` - Cancel All Operations
- `⌘F` - Focus Search
- `⌘K` - Clear Search

### Window Management
- `⌘⌥S` - Toggle Sidebar
- `⌘⌥P` - Toggle Preview
- `⌘⌃F` - Toggle Fullscreen

### Application
- `⌘,` - Preferences
- `⌘⇧I` - About
- `⌘⇧⌥M` - Toggle Menu Bar (Global)

## Troubleshooting

### Common Issues

#### Permission Denied Errors
**Problem**: Cannot access certain directories or files
**Solution**: 
1. Check System Preferences > Security & Privacy > Privacy
2. Ensure Media Organizer has Full Disk Access if needed
3. Re-grant permissions in the app's scan configuration

#### Slow Scanning Performance
**Problem**: Scanning takes too long
**Solution**:
1. Reduce the maximum scan depth
2. Exclude unnecessary file types
3. Increase batch size in settings
4. Close other resource-intensive applications

#### iCloud Files Not Found
**Problem**: iCloud files appear as placeholders
**Solution**:
1. Enable "Auto-download iCloud files" in settings
2. Ensure sufficient local storage space
3. Check iCloud sync status in System Preferences

#### High Memory Usage
**Problem**: Application uses too much memory
**Solution**:
1. Reduce memory cache size in settings
2. Process files in smaller batches
3. Restart the application periodically for large operations

### Getting Help

#### Log Files
Access detailed logs in the **Logs** view:
1. Navigate to the Logs tab
2. Filter by error level or category
3. Export logs for support requests

#### Support Channels
- **GitHub Issues**: Report bugs and feature requests
- **Documentation**: Comprehensive guides and API reference
- **Community**: User forums and discussions

## Advanced Usage

### Automation and Scripting

#### AppleScript Support
```applescript
tell application "Media Organizer"
    scan directory "/Users/username/Pictures"
    organize files with strategy "byType"
end tell
```

#### Command Line Interface
```bash
# Scan a directory
media-organizer scan --path "/Users/username/Pictures" --depth 5

# Organize files
media-organizer organize --strategy byDate --destination "/Users/username/Organized"
```

### Integration with Other Apps

#### Finder Integration
- Right-click context menu for quick scanning
- Services menu integration
- Quick Look plugin for media preview

#### Workflow Integration
- Automator actions for batch processing
- Shortcuts app integration
- Third-party workflow tools

## Development

### Building from Source

#### Prerequisites
- Xcode 14.0 or later
- macOS 12.0 SDK or later
- Swift 5.7 or later

#### Build Instructions
```bash
# Clone the repository
git clone https://github.com/your-repo/media-organizer.git
cd media-organizer

# Install dependencies (if any)
# Dependencies are managed through Swift Package Manager

# Open in Xcode
open MediaOrganizerApp.xcodeproj

# Build for development
xcodebuild -scheme MediaOrganizerApp -configuration Debug

# Build for release
xcodebuild -scheme MediaOrganizerApp -configuration Release
```

#### Running Tests
```bash
# Run unit tests
xcodebuild test -scheme MediaOrganizerApp

# Run validation script
swift run ValidationScript --validate
```

### Contributing

#### Code Style
- Follow Swift API Design Guidelines
- Use SwiftLint for code formatting
- Maintain comprehensive test coverage
- Document public APIs with Swift DocC

#### Pull Request Process
1. Fork the repository
2. Create a feature branch
3. Make your changes with tests
4. Update documentation as needed
5. Submit a pull request with detailed description

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built with SwiftUI and Combine frameworks
- Uses Apple's AVFoundation for metadata extraction
- Inspired by the need for better media file organization
- Thanks to the macOS development community for guidance and support

## Changelog

### Version 1.0.0 (Current)
- Initial release with core functionality
- File discovery and organization features
- iCloud Drive integration
- Comprehensive user interface
- Advanced job management
- Security and privacy compliance

### Planned Features
- AI-powered content recognition
- Advanced duplicate detection algorithms
- Cloud storage integration (Dropbox, Google Drive)
- Batch editing capabilities
- Advanced metadata editing
- Plugin system for extensibility

---

For more information, visit our [website](https://your-website.com) or check out the [documentation](https://docs.your-website.com).

