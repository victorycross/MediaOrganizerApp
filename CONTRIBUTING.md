# Contributing to Media Organizer

Thank you for your interest in contributing to Media Organizer! This document provides guidelines and information for contributors.

## 🤝 How to Contribute

### Reporting Issues
- **Search existing issues** before creating a new one
- **Use the issue templates** when available
- **Provide detailed information** including:
  - macOS version and hardware
  - Steps to reproduce the issue
  - Expected vs actual behavior
  - Screenshots or logs if applicable

### Suggesting Features
- **Check existing feature requests** to avoid duplicates
- **Describe the use case** and why the feature would be valuable
- **Consider the scope** and how it fits with the app's goals
- **Provide mockups or examples** if applicable

### Code Contributions

#### Getting Started
1. **Fork the repository** on GitHub
2. **Clone your fork** locally
3. **Create a feature branch** from `main`
4. **Set up the development environment** (see DEVELOPER_GUIDE.md)

#### Development Guidelines
- **Follow Swift style guidelines** and existing code patterns
- **Write comprehensive tests** for new functionality
- **Update documentation** for any API changes
- **Ensure compatibility** with macOS 12.0+
- **Test on both Intel and Apple Silicon** if possible

#### Pull Request Process
1. **Update documentation** if needed
2. **Add or update tests** for your changes
3. **Ensure all tests pass** locally
4. **Create a pull request** with:
   - Clear title and description
   - Reference to related issues
   - Screenshots for UI changes
   - Testing instructions

## 📋 Development Setup

### Prerequisites
- **Xcode 14.0+** with macOS 12.0+ SDK
- **macOS 12.0+** for development and testing
- **Apple Developer Account** for code signing (optional for development)

### Local Development
```bash
# Clone your fork
git clone https://github.com/yourusername/MediaOrganizerApp.git
cd MediaOrganizerApp

# Open in Xcode
open MediaOrganizerApp.xcodeproj

# Build and run
# Press ⌘+R in Xcode
```

### Running Tests
```bash
# In Xcode: ⌘+U to run all tests
# Or use command line:
xcodebuild test -project MediaOrganizerApp.xcodeproj -scheme MediaOrganizerApp
```

## 🎯 Code Style Guidelines

### Swift Style
- **Use Swift 5.7+ features** where appropriate
- **Follow Apple's API Design Guidelines**
- **Use meaningful variable and function names**
- **Add documentation comments** for public APIs
- **Prefer value types** over reference types when possible

### Architecture
- **Follow MVVM pattern** established in the project
- **Use Combine** for reactive programming
- **Separate concerns** between models, services, and views
- **Keep views lightweight** and delegate business logic to services

### Testing
- **Write unit tests** for models and services
- **Write integration tests** for complex workflows
- **Mock external dependencies** in tests
- **Aim for high test coverage** of critical functionality

## 🔒 Security Guidelines

### App Sandbox
- **Maintain App Sandbox compliance** in all changes
- **Use security-scoped bookmarks** for file access
- **Request minimal permissions** required for functionality
- **Test with sandbox enabled** during development

### Data Privacy
- **No data collection** without explicit user consent
- **Store sensitive data securely** using Keychain when needed
- **Respect user privacy** in all features
- **Document data usage** clearly

## 📚 Documentation

### Code Documentation
- **Document public APIs** with Swift documentation comments
- **Include usage examples** for complex functions
- **Explain non-obvious code** with inline comments
- **Keep documentation up-to-date** with code changes

### User Documentation
- **Update README.md** for user-facing changes
- **Update QUICK_START_GUIDE.md** for workflow changes
- **Add screenshots** for UI changes
- **Update help documentation** as needed

## 🚀 Release Process

### Version Management
- **Follow semantic versioning** (MAJOR.MINOR.PATCH)
- **Update version numbers** in appropriate files
- **Create release notes** describing changes
- **Tag releases** in Git

### Quality Assurance
- **All tests must pass** before release
- **Manual testing** on supported macOS versions
- **Performance testing** for large file sets
- **Security review** for sensitive changes

## 🏷️ Issue Labels

### Type Labels
- `bug` - Something isn't working correctly
- `enhancement` - New feature or improvement
- `documentation` - Documentation improvements
- `question` - Questions about usage or implementation

### Priority Labels
- `priority: high` - Critical issues affecting core functionality
- `priority: medium` - Important improvements or fixes
- `priority: low` - Nice-to-have features or minor issues

### Status Labels
- `status: needs-review` - Awaiting code review
- `status: needs-testing` - Awaiting testing
- `status: blocked` - Blocked by external dependency
- `status: wip` - Work in progress

## 🎉 Recognition

Contributors will be recognized in:
- **CONTRIBUTORS.md** file
- **Release notes** for significant contributions
- **About dialog** in the application (for major contributors)

## 📞 Getting Help

### Communication Channels
- **GitHub Issues** - For bugs and feature requests
- **GitHub Discussions** - For questions and general discussion
- **Developer Documentation** - See DEVELOPER_GUIDE.md

### Code Review
- **Be respectful** and constructive in reviews
- **Focus on the code**, not the person
- **Explain reasoning** behind suggestions
- **Be open to feedback** on your own contributions

## 📜 Code of Conduct

### Our Standards
- **Be respectful** and inclusive
- **Welcome newcomers** and help them learn
- **Focus on constructive feedback**
- **Respect different viewpoints** and experiences

### Unacceptable Behavior
- **Harassment or discrimination** of any kind
- **Trolling or inflammatory comments**
- **Personal attacks** or insults
- **Publishing private information** without consent

### Enforcement
- **Report issues** to project maintainers
- **Violations may result** in temporary or permanent bans
- **Decisions are final** and made by project maintainers

## 🙏 Thank You

Thank you for contributing to Media Organizer! Your contributions help make this tool better for everyone in the macOS community.

---

**Questions?** Feel free to open an issue or start a discussion. We're here to help! 🚀

