# EPCollaboratif

![Swift](https://img.shields.io/badge/Swift-5.0-F05138?logo=swift&logoColor=white)
![iOS](https://img.shields.io/badge/iOS-18%2B-000000?logo=apple&logoColor=white)
![CI](https://github.com/Fenriz1349/EPCollaboratif/actions/workflows/ci.yml/badge.svg)

Fork of a collaborative iOS project. Main contribution: setting up a full CI/CD pipeline
and fixing the automated versioning configuration.

---

## 🔧 Contribution — CI/CD & Versioning

### GitHub Actions Pipeline (`ci.yml`)

4-job pipeline triggered on every PR targeting `main`:

- **build** — build number bump (`agvtool`), compilation on iPhone 17 simulator
- **test** *(depends on build)* — unit tests, `.xcresult` bundle uploaded as artifact
- **deploy** *(depends on build + test)* — simulated TestFlight/App Store Connect
  deployment (foundation for future automation)
- **report** *(always runs)* — human-readable test summary posted to GitHub Actions
  run summary via Apple's native `xcresulttool`

> Jobs are skipped on `release-please--*` branches to avoid unnecessary runs
> on release PRs.

**Notable technical decisions:**
- Apple's native `xcresulttool` used instead of third-party actions
  (`kishikawakatsumi/xcresulttool`, `test-summary/action`) that are broken
  with recent Xcode versions — no fragile external dependency

### Versioning (`release-please`)

Fixed `release-please-config.json` configuration:
- Replaced invalid parameters (`search-value`/`replace-value`) with the
  `// x-release-please-version` annotation in `Version.xcconfig`
- Git tags now follow the standard `vX.Y.Z` format (removed component prefix)

---

## 🛠️ Tech stack

- Swift / SwiftUI
- GitHub Actions
- release-please / Conventional Commits
- xcresulttool (Apple)
- agvtool

---

## 📁 Pipeline structure

```
.github/
└── workflows/
    ├── ci.yml        # Main pipeline
    └── release.yml   # Release management
```
