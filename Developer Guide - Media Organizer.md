# Developer Guide - Media Organizer

This guide provides comprehensive information for developers working on the Media Organizer application, including architecture overview, coding standards, and contribution guidelines.

## Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [Project Structure](#project-structure)
3. [Core Components](#core-components)
4. [Development Setup](#development-setup)
5. [Coding Standards](#coding-standards)
6. [Testing Guidelines](#testing-guidelines)
7. [Security Considerations](#security-considerations)
8. [Performance Guidelines](#performance-guidelines)
9. [Deployment Process](#deployment-process)
10. [Contributing](#contributing)

## Architecture Overview

### Design Principles

The Media Organizer follows these key architectural principles:

- **MVVM Pattern**: Model-View-ViewModel architecture with SwiftUI
- **Reactive Programming**: Combine framework for data flow and state management
- **Separation of Concerns**: Clear separation between UI, business logic, and data layers
- **Dependency Injection**: Loose coupling through dependency injection
- **Security First**: Sandbox compliance and security-scoped resource access
- **Performance Oriented**: Efficient file operations and memory management

### Technology Stack

- **UI Framework**: SwiftUI with Combine for reactive programming
- **Language**: Swift 5.7+
- **Minimum Target**: macOS 12.0 (Monterey)
- **Frameworks**: 
  - Foundation (Core functionality)
  - AVFoundation (Media metadata)
  - CryptoKit (Checksums and security)
  - UniformTypeIdentifiers (File type detection)
  - UserNotifications (System notifications)

### System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Presentation Layer                   │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │   SwiftUI   │ │   Views     │ │  ViewModels │           │
│  │   Views     │ │             │ │             │           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                       Business Logic Layer                  │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │  Services   │ │  Managers   │ │   Models    │           │
│  │             │ │             │ │             │           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                         Data Layer                          │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │ File System │ │   Security  │ │  Utilities  │           │
│  │             │ │             │ │             │           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
└─────────────────────────────────────────────────────────────┘
```

## Project Structure

```
MediaOrganizerApp.xcodeproj/
├── MediaOrganizerApp/
│   ├── App/                          # Application entry points
│   │   ├── MediaOrganizerApp.swift   # Main app structure
│   │   ├── AppDelegate.swift         # App delegate for advanced features
│   │   └── ContentView.swift         # Root content view
│   │
│   ├── Models/                       # Data models
│   │   ├── MediaFile.swift           # Core media file model
│   │   ├── ScanConfiguration.swift   # Scan settings model
│   │   ├── ConsolidationJob.swift    # Job management model
│   │   ├── AppSettings.swift         # Application settings
│   │   └── LogEntry.swift            # Logging model
│   │
│   ├── Services/                     # Business logic services
│   │   ├── FileDiscoveryService.swift      # File scanning service
│   │   ├── MetadataExtractor.swift         # Metadata extraction
│   │   ├── FileConsolidationService.swift  # File organization
│   │   ├── DiskSpaceMonitor.swift          # Disk space monitoring
│   │   ├── iCloudIntegrationService.swift  # iCloud integration
│   │   └── LogManager.swift                # Logging service
│   │
│   ├── Views/                        # SwiftUI views
│   │   ├── MainWindowView.swift      # Main window structure
│   │   ├── SidebarView.swift         # Navigation sidebar
│   │   ├── HeaderView.swift          # Header with controls
│   │   ├── ScanView.swift            # File scanning interface
│   │   ├── FileListView.swift        # File listing and preview
│   │   ├── OrganizeView.swift        # Organization configuration
│   │   ├── JobsView.swift            # Job management
│   │   ├── LogsView.swift            # Logging interface
│   │   ├── SettingsView.swift        # Application settings
│   │   ├── StatisticsView.swift      # Analytics and statistics
│   │   ├── AboutView.swift           # About dialog
│   │   ├── PreviewView.swift         # Media preview
│   │   ├── DryRunResultView.swift    # Dry run results
│   │   └── JobDetailsView.swift      # Detailed job information
│   │
│   ├── Advanced/                     # Advanced features
│   │   ├── MenuBarManager.swift      # Menu bar integration
│   │   ├── NotificationManager.swift # System notifications
│   │   └── KeyboardShortcutManager.swift # Keyboard shortcuts
│   │
│   ├── Utilities/                    # Utility classes and extensions
│   │   ├── FileSystemUtils.swift     # File system operations
│   │   ├── SecurityUtils.swift       # Security and sandboxing
│   │   ├── Constants.swift           # Application constants
│   │   ├── Helpers.swift             # Helper functions
│   │   └── Extensions/               # Swift extensions
│   │       ├── String+Extensions.swift
│   │       ├── URL+Extensions.swift
│   │       ├── Data+Extensions.swift
│   │       └── Array+Extensions.swift
│   │
│   ├── Resources/                    # Application resources
│   │   ├── MediaOrganizer.entitlements # App entitlements
│   │   └── Info.plist                # Application info
│   │
│   └── Tests/                        # Test files
│       ├── MediaFileTests.swift      # Model tests
│       ├── FileDiscoveryServiceTests.swift # Service tests
│       └── ValidationScript.swift    # Comprehensive validation
│
├── README.md                         # Project documentation
├── DEVELOPER_GUIDE.md               # This file
├── LICENSE                          # License information
└── .gitignore                       # Git ignore rules
```

## Core Components

### Models

#### MediaFile
The core model representing a media file with metadata and operations.

```swift
class MediaFile: ObservableObject, Codable, Identifiable, Equatable {
    let id: UUID
    let url: URL
    let fileName: String
    let fileExtension: String
    let mediaType: MediaType
    let fileSize: Int64
    let creationDate: Date?
    let modificationDate: Date?
    
    // Computed properties and methods
    func calculateChecksum() throws -> String
    func isDuplicate(of other: MediaFile) throws -> Bool
    func validateFile() throws -> Bool
}
```

#### ScanConfiguration
Configuration object for file discovery operations.

```swift
class ScanConfiguration: ObservableObject, Codable {
    @Published var rootDirectories: [URL] = []
    @Published var includeHiddenFiles: Bool = false
    @Published var followSymlinks: Bool = false
    @Published var maxDepth: Int = 10
    @Published var includeImages: Bool = true
    @Published var includeVideos: Bool = true
    @Published var includeAudio: Bool = true
    @Published var extractMetadata: Bool = true
    @Published var batchSize: Int = 100
}
```

### Services

#### FileDiscoveryService
Singleton service responsible for discovering and scanning media files.

```swift
class FileDiscoveryService: ObservableObject {
    static let shared = FileDiscoveryService()
    
    @Published var isScanning: Bool = false
    @Published var progress: Double = 0.0
    @Published var discoveredFiles: [MediaFile] = []
    @Published var lastError: Error?
    
    func startScan(with configuration: ScanConfiguration)
    func cancelAllOperations()
    func pauseAllOperations()
    func resumeAllOperations()
}
```

#### FileConsolidationService
Service for organizing and moving files according to specified strategies.

```swift
class FileConsolidationService: ObservableObject {
    static let shared = FileConsolidationService()
    
    @Published var isProcessing: Bool = false
    @Published var activeJobs: [ConsolidationJob] = []
    @Published var completedJobs: [ConsolidationJob] = []
    
    func createJob(name: String, files: [MediaFile], destination: URL, options: ConsolidationOptions) -> ConsolidationJob
    func performDryRun(for job: ConsolidationJob) throws -> DryRunResult
    func executeJob(_ job: ConsolidationJob)
    func pauseJob(_ job: ConsolidationJob)
    func cancelJob(_ job: ConsolidationJob)
}
```

### Views

#### MVVM Pattern Implementation

Views follow the MVVM pattern with clear separation of concerns:

```swift
struct ScanView: View {
    @StateObject private var scanConfiguration = ScanConfiguration()
    @StateObject private var fileDiscoveryService = FileDiscoveryService.shared
    @State private var showingDirectoryPicker = false
    
    var body: some View {
        VStack {
            // UI implementation
        }
        .onAppear {
            // View lifecycle
        }
        .onChange(of: fileDiscoveryService.isScanning) { isScanning in
            // React to state changes
        }
    }
}
```

## Development Setup

### Prerequisites

1. **Xcode 14.0+** with macOS 12.0+ SDK
2. **macOS 12.0+** for development and testing
3. **Git** for version control
4. **Swift 5.7+** (included with Xcode)

### Initial Setup

```bash
# Clone the repository
git clone https://github.com/your-repo/media-organizer.git
cd media-organizer

# Open in Xcode
open MediaOrganizerApp.xcodeproj

# Install any dependencies (managed by SPM)
# Dependencies are automatically resolved by Xcode
```

### Development Environment

#### Xcode Configuration
- **Deployment Target**: macOS 12.0
- **Swift Language Version**: Swift 5
- **Code Signing**: Development team required
- **Capabilities**: App Sandbox, File Access, User Notifications

#### Build Configurations
- **Debug**: Development builds with debugging symbols
- **Release**: Optimized builds for distribution

### Running the Application

```bash
# Build and run in Xcode (⌘+R)
# Or use command line:
xcodebuild -scheme MediaOrganizerApp -configuration Debug build
```

## Coding Standards

### Swift Style Guide

Follow the [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) and these additional conventions:

#### Naming Conventions
```swift
// Classes and Structs: PascalCase
class FileDiscoveryService { }
struct MediaFile { }

// Variables and Functions: camelCase
var isScanning: Bool = false
func startScan(with configuration: ScanConfiguration) { }

// Constants: camelCase or SCREAMING_SNAKE_CASE for global constants
let maxRetryAttempts = 3
let DEFAULT_BATCH_SIZE = 100

// Enums: PascalCase with camelCase cases
enum MediaType {
    case image
    case video
    case audio
    case other
}
```

#### Code Organization
```swift
// MARK: - Properties
private let fileManager = FileManager.default
@Published var isScanning: Bool = false

// MARK: - Initialization
init() {
    setupObservers()
}

// MARK: - Public Methods
func startScan(with configuration: ScanConfiguration) {
    // Implementation
}

// MARK: - Private Methods
private func setupObservers() {
    // Implementation
}
```

#### Error Handling
```swift
// Use specific error types
enum MediaFileError: LocalizedError {
    case fileNotFound
    case invalidFormat
    case accessDenied
    
    var errorDescription: String? {
        switch self {
        case .fileNotFound:
            return "File not found"
        case .invalidFormat:
            return "Invalid file format"
        case .accessDenied:
            return "Access denied"
        }
    }
}

// Proper error propagation
func processFile(at url: URL) throws -> MediaFile {
    guard FileManager.default.fileExists(atPath: url.path) else {
        throw MediaFileError.fileNotFound
    }
    
    // Process file
    return try MediaFile(url: url)
}
```

#### Async/Await Usage
```swift
// Use async/await for asynchronous operations
func scanDirectory(_ url: URL) async throws -> [MediaFile] {
    return try await withThrowingTaskGroup(of: [MediaFile].self) { group in
        // Concurrent processing
    }
}

// Combine with @MainActor for UI updates
@MainActor
func updateProgress(_ progress: Double) {
    self.progress = progress
}
```

### SwiftUI Best Practices

#### View Composition
```swift
// Break down complex views into smaller components
struct FileListView: View {
    var body: some View {
        VStack {
            FileListHeaderView()
            FileListContentView()
            FileListFooterView()
        }
    }
}

// Use ViewBuilder for conditional content
@ViewBuilder
private func statusIndicator() -> some View {
    if isScanning {
        ProgressView()
    } else {
        Image(systemName: "checkmark.circle")
    }
}
```

#### State Management
```swift
// Use appropriate property wrappers
@State private var searchText = ""           // Local state
@StateObject private var service = Service() // Object lifecycle
@ObservedObject var configuration: Config    // External object
@EnvironmentObject var settings: AppSettings // Shared state
```

### Documentation Standards

#### Code Documentation
```swift
/// Discovers media files in specified directories
/// 
/// This service provides comprehensive file discovery capabilities with support for
/// filtering, metadata extraction, and progress tracking.
///
/// - Important: Always call `startScan(with:)` on the main thread
/// - Note: Large directory scans may take significant time
class FileDiscoveryService: ObservableObject {
    
    /// Starts a new file discovery operation
    /// 
    /// - Parameter configuration: Scan configuration with directories and options
    /// - Throws: `FileDiscoveryError` if scan cannot be started
    /// - Returns: Void, results are published through `discoveredFiles`
    func startScan(with configuration: ScanConfiguration) throws {
        // Implementation
    }
}
```

#### README and Guides
- Use clear, concise language
- Provide code examples for complex features
- Include troubleshooting sections
- Keep documentation up to date with code changes

## Testing Guidelines

### Test Structure

#### Unit Tests
```swift
class MediaFileTests: XCTestCase {
    var tempDirectory: URL!
    
    override func setUpWithError() throws {
        tempDirectory = FileManager.default.temporaryDirectory
            .appendingPathComponent(UUID().uuidString)
        try FileManager.default.createDirectory(at: tempDirectory, 
                                               withIntermediateDirectories: true)
    }
    
    override func tearDownWithError() throws {
        try FileManager.default.removeItem(at: tempDirectory)
    }
    
    func testMediaFileInitialization() throws {
        // Test implementation
    }
}
```

#### Integration Tests
```swift
class FileDiscoveryServiceTests: XCTestCase {
    func testFullScanWorkflow() throws {
        let service = FileDiscoveryService()
        let config = ScanConfiguration()
        
        let expectation = XCTestExpectation(description: "Scan completion")
        
        // Test async operations with expectations
        service.$isScanning
            .dropFirst()
            .sink { isScanning in
                if !isScanning {
                    expectation.fulfill()
                }
            }
            .store(in: &cancellables)
        
        service.startScan(with: config)
        wait(for: [expectation], timeout: 10.0)
        
        XCTAssertGreaterThan(service.discoveredFiles.count, 0)
    }
}
```

### Test Coverage

Maintain high test coverage for:
- **Models**: Data integrity and business logic
- **Services**: Core functionality and error handling
- **Utilities**: Helper functions and extensions
- **Integration**: End-to-end workflows

### Performance Testing
```swift
func testScanPerformance() throws {
    measure {
        // Performance-critical code
    }
}
```

## Security Considerations

### Sandbox Compliance

The application must comply with macOS App Sandbox requirements:

#### Entitlements
```xml
<key>com.apple.security.app-sandbox</key>
<true/>
<key>com.apple.security.files.user-selected.read-write</key>
<true/>
<key>com.apple.security.files.downloads.read-write</key>
<true/>
```

#### Security-Scoped Bookmarks
```swift
// Create bookmarks for persistent access
func createBookmark(for url: URL) throws -> Data {
    return try url.bookmarkData(
        options: .withSecurityScope,
        includingResourceValuesForKeys: nil,
        relativeTo: nil
    )
}

// Resolve bookmarks for access
func resolveBookmark(_ bookmarkData: Data) throws -> URL {
    var isStale = false
    let url = try URL(
        resolvingBookmarkData: bookmarkData,
        options: .withSecurityScope,
        relativeTo: nil,
        bookmarkDataIsStale: &isStale
    )
    
    guard url.startAccessingSecurityScopedResource() else {
        throw SecurityError.accessDenied
    }
    
    return url
}
```

### Data Protection

#### Sensitive Data Handling
- Never store user file paths in plain text
- Use security-scoped bookmarks for persistent access
- Encrypt sensitive configuration data
- Clear sensitive data from memory when not needed

#### Privacy Compliance
- No data collection without explicit consent
- No network communication for user data
- Local processing only
- Clear privacy policy and data usage

## Performance Guidelines

### Memory Management

#### Efficient File Processing
```swift
// Process files in batches to manage memory
func processFilesInBatches(_ files: [URL], batchSize: Int = 100) async throws {
    for batch in files.chunked(into: batchSize) {
        try await processBatch(batch)
        
        // Allow memory cleanup between batches
        try await Task.sleep(nanoseconds: 10_000_000) // 10ms
    }
}
```

#### Resource Cleanup
```swift
// Proper resource management
func processFile(at url: URL) throws {
    let accessed = url.startAccessingSecurityScopedResource()
    defer {
        if accessed {
            url.stopAccessingSecurityScopedResource()
        }
    }
    
    // Process file
}
```

### Concurrent Processing

#### Task Groups for Parallel Processing
```swift
func scanDirectoriesConcurrently(_ directories: [URL]) async throws -> [MediaFile] {
    return try await withThrowingTaskGroup(of: [MediaFile].self) { group in
        for directory in directories {
            group.addTask {
                try await self.scanDirectory(directory)
            }
        }
        
        var allFiles: [MediaFile] = []
        for try await files in group {
            allFiles.append(contentsOf: files)
        }
        return allFiles
    }
}
```

#### Progress Reporting
```swift
// Efficient progress updates
private var progressUpdateTimer: Timer?

func startProgressUpdates() {
    progressUpdateTimer = Timer.scheduledTimer(withTimeInterval: 0.1, repeats: true) { _ in
        DispatchQueue.main.async {
            self.updateProgress()
        }
    }
}
```

## Deployment Process

### Build Configuration

#### Release Build Settings
- **Optimization Level**: Optimize for Speed (-O)
- **Swift Compilation Mode**: Whole Module Optimization
- **Debug Information**: Strip Debug Symbols
- **Code Signing**: Distribution certificate

#### Entitlements Verification
```bash
# Verify entitlements
codesign -d --entitlements :- MediaOrganizer.app
```

### Distribution

#### App Store Distribution
1. Archive the application in Xcode
2. Validate the archive
3. Upload to App Store Connect
4. Submit for review

#### Direct Distribution
1. Create a Developer ID signed build
2. Notarize the application with Apple
3. Create a DMG installer
4. Distribute through website or other channels

### Continuous Integration

#### GitHub Actions Example
```yaml
name: Build and Test

on: [push, pull_request]

jobs:
  test:
    runs-on: macos-latest
    steps:
    - uses: actions/checkout@v3
    - name: Build and Test
      run: |
        xcodebuild test -scheme MediaOrganizerApp -destination 'platform=macOS'
```

## Contributing

### Development Workflow

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Pull Request Guidelines

#### Before Submitting
- [ ] Code follows style guidelines
- [ ] All tests pass
- [ ] New tests added for new functionality
- [ ] Documentation updated
- [ ] No breaking changes (or clearly documented)

#### PR Description Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] Manual testing completed

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
```

### Code Review Process

#### Review Criteria
- **Functionality**: Does the code work as intended?
- **Performance**: Are there any performance implications?
- **Security**: Does the code follow security best practices?
- **Maintainability**: Is the code readable and well-structured?
- **Testing**: Is there adequate test coverage?

#### Review Timeline
- Initial review within 2 business days
- Follow-up reviews within 1 business day
- Approval required from at least one maintainer

### Release Process

#### Version Numbering
Follow [Semantic Versioning](https://semver.org/):
- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes (backward compatible)

#### Release Checklist
- [ ] Version number updated
- [ ] Changelog updated
- [ ] All tests passing
- [ ] Documentation updated
- [ ] Release notes prepared
- [ ] App Store metadata updated (if applicable)

---

This developer guide is a living document. Please keep it updated as the project evolves and new patterns emerge.

