# Media Organizer for macOS

![Platform](https://img.shields.io/badge/platform-macOS-blue)
![Swift](https://img.shields.io/badge/swift-5.7+-orange)
![Xcode](https://img.shields.io/badge/xcode-14.0+-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![CI](https://github.com/victorycross/MediaOrganizerApp/workflows/CI/badge.svg)

A powerful, intelligent media file organizer built with SwiftUI that helps you discover, categorize, and organize your photos, videos, and audio files with ease.

![Workflow Diagram](assets/workflow_diagram.png)

## ✨ Features

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

![Interface Overview](assets/interface_overview.png)

## 🚀 Quick Start

### System Requirements
- **macOS**: 12.0 (Monterey) or later
- **Architecture**: Intel x64 or Apple Silicon (Universal Binary)
- **Memory**: 4GB RAM minimum, 8GB recommended
- **Storage**: 100MB for application, additional space for file operations

### Installation

#### Option 1: Download Release (Recommended)
1. Download the latest release from the [Releases](https://github.com/victorycross/MediaOrganizerApp/releases) page
2. Open the downloaded `.dmg` file
3. Drag "Media Organizer" to your Applications folder
4. Launch the application from Applications or Spotlight

#### Option 2: Build from Source
```bash
# Clone the repository
git clone https://github.com/victorycross/MediaOrganizerApp.git
cd MediaOrganizerApp

# Open in Xcode
open MediaOrganizerApp/MediaOrganizerApp.xcodeproj

# Build and run (⌘+R)
```

### Basic Workflow

![Organization Strategies](assets/organization_strategies.png)

1. **📁 Scan**: Use the scan view to discover media files in your chosen directories
2. **📋 Review**: Browse discovered files in the files view with preview capabilities  
3. **⚙️ Organize**: Configure organization settings and preview changes with dry run
4. **✅ Execute**: Run the organization job to move files to their new locations
5. **📊 Monitor**: Track progress in the jobs view and review logs for details

## 📖 Documentation

- **[📚 User Guide](docs/README.md)** - Complete user documentation
- **[🚀 Quick Start Guide](docs/QUICK_START_GUIDE.md)** - Visual step-by-step guide
- **[👨‍💻 Developer Guide](docs/DEVELOPER_GUIDE.md)** - Technical documentation
- **[🚀 Deployment Guide](docs/DEPLOYMENT.md)** - Distribution instructions
- **[📋 Project Summary](docs/PROJECT_SUMMARY.md)** - Executive overview

## ⌨️ Keyboard Shortcuts

### File Operations
- `⌘N` - New Scan
- `⌘⇧Q` - Quick Scan (Global)
- `⌘O` - Organize Files
- `⌘R` - Refresh Current View

### Navigation
- `⌘1-6` - Switch between views (Scan, Files, Organize, Jobs, Logs, Statistics)
- `⌘F` - Focus Search
- `⌘Space` - Pause All Operations

### Window Management
- `⌘,` - Preferences
- `⌘⌥S` - Toggle Sidebar
- `⌘⌃F` - Toggle Fullscreen

## 🛠️ Development

### Prerequisites
- **Xcode 14.0+** with macOS 12.0+ SDK
- **macOS 12.0+** for development and testing
- **Apple Developer Account** for code signing (optional for development)

### Building
```bash
# Clone the repository
git clone https://github.com/victorycross/MediaOrganizerApp.git
cd MediaOrganizerApp

# Open in Xcode
open MediaOrganizerApp/MediaOrganizerApp.xcodeproj

# Build and run
# Press ⌘+R in Xcode
```

### Testing
```bash
# Run tests in Xcode: ⌘+U
# Or use command line:
xcodebuild test -project MediaOrganizerApp/MediaOrganizerApp.xcodeproj -scheme MediaOrganizerApp
```

### Project Structure
```
MediaOrganizerApp/
├── .github/                    # GitHub templates and workflows
├── MediaOrganizerApp/          # Xcode project and Swift source code
│   ├── App/                    # Application entry points
│   ├── Models/                 # Data models and business logic
│   ├── Services/               # Core services and managers
│   ├── Views/                  # SwiftUI user interface
│   ├── Advanced/               # Advanced features and integrations
│   ├── Utilities/              # Helper classes and extensions
│   ├── Resources/              # App resources and configuration
│   └── Tests/                  # Comprehensive test suite
├── docs/                       # Documentation files
├── assets/                     # Images and visual assets
├── README.md                   # This file
├── LICENSE                     # MIT License
└── .gitignore                  # Git ignore rules
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

### Quick Contribution Guide
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Follow Swift style guidelines and existing code patterns
- Write comprehensive tests for new functionality
- Update documentation for any API changes
- Ensure compatibility with macOS 12.0+

## 🐛 Issues and Support

- **🐛 Bug Reports**: Use our [bug report template](.github/ISSUE_TEMPLATE/bug_report.md)
- **✨ Feature Requests**: Use our [feature request template](.github/ISSUE_TEMPLATE/feature_request.md)
- **❓ Questions**: Check the [documentation](docs/) or open a discussion

## 📊 Project Stats

- **Language**: Swift 5.7+
- **Framework**: SwiftUI with Combine
- **Architecture**: MVVM pattern
- **Lines of Code**: 8,000+ production code
- **Test Coverage**: Comprehensive unit and integration tests
- **Documentation**: 12,000+ words

## 🏆 Features Showcase

### Advanced File Discovery
![Step 1 - Scan](assets/step1_scan.png)

### Rich File Preview
![Step 2 - Review](assets/step2_review.png)

### Intelligent Organization
![Step 3 - Organize](assets/step3_organize.png)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built with [SwiftUI](https://developer.apple.com/xcode/swiftui/) and [Combine](https://developer.apple.com/documentation/combine)
- Follows [Apple's Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- Inspired by the need for better media file organization on macOS

## 📈 Roadmap

- [ ] AI-powered file categorization
- [ ] Cloud storage integration (Dropbox, Google Drive)
- [ ] Advanced metadata editing
- [ ] Plugin system for extensibility
- [ ] iOS companion app

---

**Made with ❤️ for the macOS community**

*Star ⭐ this repository if you find it helpful!*

