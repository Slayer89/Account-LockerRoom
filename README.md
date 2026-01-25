# Account LockerRoom

![Windows](https://img.shields.io/badge/platform-Windows-0078D6)
![.NET](https://img.shields.io/badge/.NET-8-512BD4)
![UI](https://img.shields.io/badge/UI-WPF-0B5CAD)
![License](https://img.shields.io/badge/license-Proprietary-important)

**Version:** 2.2.0

Account LockerRoom is a Windows desktop password manager (WPF on .NET 8) with an optional browser extension + native host bridge for autofill and credential capture.

- [What’s in this repo](#whats-in-this-repo)
- [Screenshots](#screenshots)
- [Key features (verified)](#key-features-verified)
- [Architecture](#architecture)
- [Data locations](#data-locations)
- [System requirements](#system-requirements)
- [License](#license)
- [Disclaimer](#disclaimer)

## What’s in this repo

| Component | Purpose | Location |
|---|---|---|
| Desktop app | Main WPF application | `Account LockerRoom/Presentation/AccountLockerRoom.csproj` |
| Core layers | Domain + application services + infrastructure + shared utilities | `Account LockerRoom/Core`, `Account LockerRoom/Application`, `Account LockerRoom/Infrastructure`, `Account LockerRoom/Shared` |
| Updater | Separate updater executable (must sit next to the app EXE) | `Updater/Updater.csproj` |
| Browser extension | MV3 extension for pairing/autofill/capture | `BrowserExtension/AccountLockerRoom.Extension/` |
| Native host (optional) | Native Messaging host / bridge for the extension | `NativeHost/AccountLockerRoom.NativeHost/` |

## Screenshots

| Main program | Security audit |
|---|---|
| ![MainProgram](Assets/Docs/Screenshots/MainProgram.png) | ![SecurityAudit](Assets/Docs/Screenshots/SecurityAudit.png) |

| Quick login | Notes manager (create note) |
|---|---|
| ![QuickLogin](Assets/Docs/Screenshots/QuickLogin.png) | ![NotesManager_CreateNote](Assets/Docs/Screenshots/NotesManager_CreateNote.png) |

## Key features (verified)

### Vault + data

- **SQLite-backed vault** with encrypted credential fields.
- **Local app-data storage** under `%APPDATA%\Account_Lockerroom\` (vault database, notes folder, station list, UI preferences, logs).

### Security

- **Credential encryption** with key-derivation and envelope-style key management (Argon2id-based derivation is used in the key-management path).
- **Two-factor authentication (TOTP)** with QR provisioning and **recovery codes**.
- **Clipboard auto-clear** after copying sensitive fields.
- **Stealth mode**: global hotkey hides the app and requires unlock to return.
- **Virtual keyboard** for password/text entry.
- **Security audit** UI (weak and duplicate password detection; “old passwords” is currently a placeholder).

### Password UX

- **Password generator** and **password strength evaluation** used in registration and note protection flows.

### Notes

- **Personal and account notes** stored on disk under the app data folder.
- **Optional password-protected notes** (BCrypt verification + encrypted content with per-note salt).

### Tools

- **Calculator** tool window.
- **Radio player + station manager**: stations are persisted to `%APPDATA%\Account_Lockerroom\stations.json` and can be merged with an online list.

### Backup, import/export

- **Encrypted backup/export and restore** of the vault database.
- **Credential import** from CSV/Excel (with header validation) and related backup UI.

### Updates

- **Update manifest** model with size + SHA-256 validation.
- The desktop app can stage an update package and **launch `Updater.exe`** (must be adjacent to the desktop app executable).

### Browser integration (optional)

The browser extension pairs with the desktop app and can request credentials for autofill and send captured credentials back for review.

See: `BrowserExtension/AccountLockerRoom.Extension/README.md`

## Architecture

![Architecture diagram](Assets/Docs/Architecture/architecture.svg)

<!-- Diagram source (Mermaid) is maintained in Assets/Docs/Architecture/architecture.mmd -->

## Data locations

Account LockerRoom stores user data under:

- `%APPDATA%\Account_Lockerroom\`
- Example: `C:\Users\<you>\AppData\Roaming\Account_Lockerroom\`

Common files/folders you may see there:

| Name | Purpose |
|---|---|
| `vaults.json` | Vault list |
| `ui_preferences.json` | UI settings |
| `stations.json` | Radio stations |
| `Notes\` | Personal and account notes |
| `Logs\` | Structured logs |

## System requirements

- Windows 10+ recommended
- .NET 8 Desktop Runtime (for the WPF desktop app)
- .NET Framework 4.8 (for `Updater.exe`)

## License

This repository contains proprietary software owned by Websynth. All rights reserved.

## Disclaimer

Account LockerRoom is provided “as is”, without warranty of any kind, express or implied.
