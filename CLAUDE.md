# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

```bash
# Build for simulator
xcodebuild -scheme BuildWithCLI -destination 'platform=iOS Simulator,name=iPhone 16' build

# Run tests
xcodebuild -scheme BuildWithCLI -destination 'platform=iOS Simulator,name=iPhone 16' test

# Clean build
xcodebuild -scheme BuildWithCLI clean
```

## Architecture

**BuildWithCLI** is a minimal SwiftUI + SwiftData iOS app targeting iOS 26 / Xcode 26.

### Stack
- **UI**: SwiftUI with `NavigationSplitView` (master-detail layout)
- **Persistence**: SwiftData (`@Model`, `@Query`, `ModelContainer`)
- **Concurrency**: MainActor isolation by default (`SWIFT_DEFAULT_ACTOR_ISOLATION = MainActor`)

### Key Files
- `BuildWithCLIApp.swift` — App entry point; initializes the SwiftData `ModelContainer` with the `Item` schema
- `ContentView.swift` — Main view; uses `@Query` to observe `Item` changes and `@Environment(\.modelContext)` for mutations
- `Item.swift` — SwiftData model (`@Model`) with a single `timestamp: Date` property

### Data Flow
SwiftData drives all state: `@Query` in `ContentView` automatically reflects any `modelContext` insert/delete operations, so there is no manual state synchronization needed.
