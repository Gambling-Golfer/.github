---
name: ios-development
description: iOS platform patterns, conventions, and validation commands for the Gambling Golfer iOS app. Use this when writing, reviewing, or testing Swift/SwiftUI code.
---

# iOS Development — Gambling Golfer

## Stack

- **Platform**: iOS 17.0+
- **Language**: Swift 5.9+
- **UI Framework**: SwiftUI (no UIKit unless necessary)
- **Architecture**: MVVM with `@Observable` / `ObservableObject`
- **Package Manager**: Swift Package Manager (SPM)
- **Project Generation**: XcodeGen (`project.yml` → `.xcodeproj`)
- **Linting**: SwiftLint
- **Formatting**: SwiftFormat

## Project Structure

```
GamblingGolfer/
├── App/              # App entry, main views, navigation
├── Features/         # Feature modules (vertical slices)
│   ├── Auth/         # Login, signup, password reset
│   ├── Games/        # Game creation, management, play
│   ├── Scores/       # Score entry, history, stats
│   └── Profile/      # User profile, settings
├── Core/
│   ├── Models/       # Data models, DTOs
│   ├── Services/     # Business logic services
│   ├── Networking/   # API client, request/response handling
│   └── Utilities/    # Extensions, helpers
└── UI/
    ├── Components/   # Reusable UI components
    ├── Styles/       # Custom ViewModifiers, colors
    └── Extensions/   # SwiftUI/Foundation extensions
```

## SwiftUI Patterns

### View Extraction
```swift
// ✅ Extract subviews for clarity
struct ProfileView: View {
    var body: some View {
        VStack {
            ProfileHeader()
            ProfileContent()
        }
    }
}
```

### State Management
```swift
@State private var isLoading = false           // View-local state
@StateObject private var viewModel = MyVM()    // Owned ObservableObject
@EnvironmentObject var authManager: AuthManager // Shared state
```

### Async Patterns
```swift
// Use .task for load-on-appear
.task { await viewModel.loadData() }

// Use Task {} from synchronous contexts
Button("Load") { Task { await viewModel.loadData() } }
```

### Error Handling
```swift
do {
    try await service.fetchData()
} catch {
    errorMessage = error.localizedDescription
}
```

## Implementation Checklist

When writing iOS code, ensure:

- [ ] No force unwraps — use `guard let` or optional binding
- [ ] `@MainActor` on UI-updating code
- [ ] Large view bodies extracted into sub-views
- [ ] `async/await` over Combine for new code
- [ ] `// MARK: -` sections for file organization
- [ ] Accessibility labels on interactive elements
- [ ] `CodingKeys` for snake_case → camelCase conversion when needed
- [ ] Sensitive data in Keychain, preferences in UserDefaults
- [ ] Dynamic Type and Dark Mode supported

## Testing Patterns

```swift
import XCTest
@testable import GamblingGolfer

final class ServiceTests: XCTestCase {
    var sut: MyService!

    override func setUp() { super.setUp(); sut = MyService() }
    override func tearDown() { sut = nil; super.tearDown() }

    func testBehavior() async throws {
        // Arrange → Act → Assert
    }
}
```

### URLProtocol Stubbing (for API tests)
Use `URLSessionConfiguration.ephemeral` with custom `protocolClasses` to stub HTTP responses without hitting the network.

## Validation Commands

Run these before committing (in order):

```bash
swiftformat .                    # Format all Swift files
swiftlint                        # Lint check
xcodebuild build -scheme GamblingGolfer -destination 'platform=iOS Simulator,name=iPhone 16,OS=18.6' -quiet
xcodebuild test -scheme GamblingGolfer -destination 'platform=iOS Simulator,name=iPhone 16,OS=18.6' -only-testing:GamblingGolferTests
```

## API Integration

- Backend connection: `http://localhost:3000` (local), `https://api.gamblinggolfer.com` (prod)
- **API docs**: Swagger at `http://localhost:3000/api-docs/` (source of truth)
- All responses follow `{ success, data, meta }` format
- iOS models must match backend DTOs — reference Swagger for exact field names
