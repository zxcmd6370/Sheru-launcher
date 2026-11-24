
Sheru Launcher
A desktop game launcher for Windows, built on the open-source Hydra 4.4.1 engine. Sheru Launcher manages your game libraries in one place, downloads and launches games, tracks achievements and playtime, and runs a full-screen Big Picture mode for couch play.

The UI is rebranded as Sheru Launcher. The engine, app ID (gg.hydralauncher.hydra), profile folder (%APPDATA%\hydralauncher) and backend endpoints are unchanged.

Version: 4.4.1 · Platform: Windows x64 · License: MIT

Contents
Features
Download
Installation
First run
Settings reference
Download sources
Building from source
Release files
Project structure
Troubleshooting
Credits and license
Features
Library

Catalogue browser with search, filters, cover art and game detail pages.
Import and manage libraries from Steam and Epic Games accounts.
Custom library entries, collections, favourites, playtime tracking and last-played history.
Profile page with your library, achievements and stats.
Downloads

Built-in download manager with queue, pause/resume, speed limits and seeding control.
BitTorrent and direct HTTP downloads, plus multi-part archives with automatic extraction.
Debrid support for faster direct links: AllDebrid, Real-Debrid, Premiumize, TorBox.
Download sources: pluggable JSON feeds that supply download links for catalogue games.
Configurable download directory with automatic relocation of existing files.
Achievements and social

Achievement tracking, completion percentages and per-game progress.
RetroAchievements integration for classic console games.
Global leaderboards and per-game leaderboard pages.
Friends list, chat windows and notifications.
Emulation and compatibility

Emulation support for console platforms, with per-console emulator configuration.
Game compatibility settings and per-game launch options / environment variables.
Interface

Dark desktop UI with adjustable themes, a built-in theme editor, and accessibility options.
Big Picture mode: full-screen gamepad-friendly interface with its own settings.
Screenshot capture with keyboard or gamepad hotkeys.
35 interface languages.
Under the hood

Electron 41, React renderer, TypeScript throughout, 82 runtime dependencies.
Auto-update support through an update manifest (latest.yml).
Self-contained installer and portable build; no external runtime required.
Download
Grab the files from the Releases page of this repository.

File	Size	What it is
SheruLauncher-4.4.1-Setup.exe	193.8 MB	Installer: registers in Add/Remove Programs, creates Start Menu and desktop shortcuts
SheruLauncher-4.4.1-Portable.exe	172.9 MB	Portable build: run it directly, no install, no registry entries, no shortcuts
Uninstall Sheru Launcher.exe	272 KB	The uninstaller placed inside the install folder (prefer the Windows uninstall entry)
SheruLauncher-4.4.1-Setup.exe.blockmap	203 KB	Differential update map for the installer
latest.yml	345 B	Auto-update manifest
SHA256SUMS.txt	290 B	SHA-256 checksums for all executables


Checksums

Auto (Plain text)
Copy
f4cef2c89f3b62b4359e781600694f021ce360b9f3a5724fa536d836d1355f61  SheruLauncher-4.4.1-Portable.exe
6407b07d92fd69d347430daba100ba8138f3b2df8cc8259180bc4ec21d913bb8  SheruLauncher-4.4.1-Setup.exe
ceae8e72fc42f499779d941970623e278aadec071722a5a90d0ea23494d1e3cb  Uninstall Sheru Launcher.exe
Verify on Windows:


powershell
Copy
Get-FileHash .\SheruLauncher-4.4.1-Setup.exe -Algorithm SHA256
Installation
Installer
Run SheruLauncher-4.4.1-Setup.exe.
Choose install for all users or only for me.
Choose the install folder. Defaults:
all users → C:\Program Files\Sheru Launcher
only for me → C:\Users\<you>\AppData\Local\Programs\Sheru Launcher
Any other folder (including another drive such as D:\Sheru Launcher) works.
Finish. The launcher appears in the Start Menu, on the desktop, and in Settings → Apps.
The installer is not code-signed, so Windows SmartScreen may warn about an unknown publisher on first run: choose More info → Run anyway.

Portable
Run SheruLauncher-4.4.1-Portable.exe from anywhere. It unpacks to a temporary folder at each launch and leaves no install footprint. Settings still live in %APPDATA%\hydralauncher, so the portable and installed builds share configuration.

Uninstall
Settings → Apps → Sheru Launcher → Uninstall (or run Uninstall Sheru Launcher.exe from the install folder).

Uninstalling removes the program files, shortcuts and registry entries. Game files, downloads, and the %APPDATA%\hydralauncher profile folder are kept, so re-installing preserves your library and settings.

First run
Sign in (optional). Accounts use the upstream Hydra account service; signing in enables cloud saves, friends, chat and leaderboards. The library, downloads and emulation work without an account.
Set your download folder in Settings → Downloads.
Connect game stores in Settings → Integrations (Steam, Epic Games) to import existing libraries.
Add download sources (optional, see below) if you want the built-in downloader to fetch games.
Settings reference
Page	What it controls
Account	Sign in/out, profile, linked services
General	Start with Windows, close/minimise to tray, language, run-as-admin, hardware acceleration
Appearance	Theme, accent colour, custom themes, theme editor
Downloads	Download folder, speed limits, seeding, extraction, concurrent downloads
Download sources	JSON feeds that provide download links
Integrations	Steam and Epic Games account linking
Emulation	Per-console emulator paths and options
Compatibility	Default launch behaviour and per-game overrides
Content & gameplay	Content filters, gameplay-related preferences
Screenshots	Screenshot folder, keyboard and gamepad hotkeys
Notifications	In-app and desktop notification behaviour
Big Picture	Full-screen mode settings and gamepad options
Debrid	AllDebrid, Real-Debrid, Premiumize, TorBox API keys
Trackers	Global BitTorrent tracker lists


Download sources
The downloader needs at least one download source: a URL to a JSON feed describing where a game's files can be fetched. The launcher ships with none, so this list is empty on a fresh install and everything else still works.

Add a source in Settings → Download sources → Add source. The URL must be a direct link to the JSON file (a raw GitHub/Gist link, not a page that renders it). Format:


JSON
Copy
{
  "name": "My Source",
  "downloads": [
    {
      "title": "Game Title",
      "fileSize": "12.4 GB",
      "uris": ["https://example.com/file.torrent"],
      "uploadDate": "2026-11-24T00:00:00.000Z"
    }
  ]
}
uris accepts magnet links and direct HTTP links. After adding, press Sync sources — the entries appear as download options on each game page.

Only add sources you are entitled to use. This project does not host, bundle or endorse any third-party download feed.

Building from source
Requirements
Windows 10/11 x64
Node.js 20 or newer and npm
~10 GB free disk space (dependencies plus two packaged outputs)
Build

Shell
Copy
npm install
Create a .env file in the project root. Without it the app fails at startup with Cannot read properties of undefined (reading 'includes'), because the build inlines these values:


env
Copy
MAIN_VITE_API_URL=https://hydra-api-us-east-1.losbroxas.org
MAIN_VITE_AUTH_URL=https://auth.hydralauncher.gg
MAIN_VITE_CHECKOUT_URL=https://checkout.hydralauncher.gg
MAIN_VITE_EXTERNAL_RESOURCES_URL=https://assets.hydralauncher.gg
RENDERER_VITE_EXTERNAL_RESOURCES_URL=https://assets.hydralauncher.gg
MAIN_VITE_WS_URL=wss://ws.hydralauncher.gg
MAIN_VITE_LAUNCHER_SUBDOMAIN=
Then:


Shell
Copy
npm run build          # typecheck + compile to out/
npx electron-builder --win   # installer + portable into dist/
Useful variants:

Command	Result
npm run dev	Dev server with hot reload
npm run build:win	Compile and package in one step
npx electron-builder --win nsis	Installer only
npx electron-builder --win portable	Portable only
npx electron-builder --win --dir	Unpacked folder, fastest for testing
npm run typecheck	TypeScript checks for main/preload and renderer


Packaging takes roughly 4–6 minutes per target at this app's size.

Release files
release/ contains the artifacts and metadata ready to attach to a GitHub release:


Auto (Plain text)
Copy
release/
├── SheruLauncher-4.4.1-Setup.exe          installer
├── SheruLauncher-4.4.1-Setup.exe.blockmap differential update map
├── SheruLauncher-4.4.1-Portable.exe       portable build
├── Uninstall Sheru Launcher.exe           uninstaller copy
├── latest.yml                             auto-update manifest
├── SHA256SUMS.txt                         checksums
└── RELEASE_NOTES.md                       ready-to-paste release body
GitHub rejects repository files over 100 MB, so the executables belong in Releases (assets up to 2 GB each), not in the repo history.

Project structure

Auto (Plain text)
Copy
├── src/
│   ├── main/            Electron main process: windows, IPC, updater, downloads
│   ├── preload/         Bridge exposed to the renderer
│   ├── renderer/        React UI (desktop)
│   ├── big-picture/     Full-screen gamepad UI
│   ├── locales/         35 language translation files
│   └── types/           Shared TypeScript types
├── native/              Native addons
├── legendary/           Epic Games CLI integration (bundled per platform)
├── ludusavi/            Save-game backup tool
├── build/               Installer resources, icons, NSIS include
├── resources/           Runtime assets
├── scripts/             Build and packaging hooks
└── electron-builder.yml Packaging configuration
Packaging settings of note: appId: gg.hydralauncher.hydra, productName: Sheru Launcher, win.executableName: Sheru Launcher, NSIS with oneClick: false and allowToChangeInstallationDirectory: true, desktop shortcut always created, publish set to GitHub.

Troubleshooting
SmartScreen blocks the installer. It's unsigned. More info → Run anyway.

App window opens empty. The build did not get its .env at compile time, so it is pointing at localhost. Rebuild with the .env above.

"A JavaScript error occurred in the main process … reading 'includes'". Same cause: MAIN_VITE_API_URL was missing when the bundle was compiled. Add .env and rebuild.

Downloads never start. No download source is configured, or the source's feed is unreachable. Check Settings → Download sources and press Sync sources.

Sign-in fails. Accounts are handled by the upstream Hydra service; the launcher does not host its own auth server. Offline features keep working without an account.

Second launch does nothing. The launcher is single-instance: it focuses the running window instead of starting another copy.

Credits and license
Sheru Launcher is a rebranded build of Hydra Launcher 4.4.1 by Los Broxas, distributed under the MIT License. It also bundles work from:

legendary — open-source Epic Games client
Ludusavi — save-game backup tool
7-Zip — archive extraction
Electron, React, Radix UI, Sentry and other open-source libraries listed in package.json

Auto (Plain text)
Copy
MIT License

Copyright (c) 2024 Los Broxas

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
