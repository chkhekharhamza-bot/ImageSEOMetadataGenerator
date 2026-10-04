# Image SEO Metadata Generator (Windows Desktop Application)

A professional, offline-first Windows 10 & 11 desktop application built with **Tauri 2**, **React 19**, **Tailwind CSS**, and a **Rust** backend for lossless image metadata embedding and SEO optimization.

---

## 🌟 Key Features

- **100% Offline & Private:** All images and metadata are processed locally on your PC. No images or keywords are ever uploaded to any cloud server or telemetry service.
- **Single Keyword Generation:** Enter one primary SEO keyword (e.g. `shower personality test`) and automatically generate 12 professional metadata fields.
- **12 Fully Editable Metadata Fields:**
  1. SEO Filename (sanitized, lowercase, hyphen-separated, keyword-rich)
  2. ALT Text (natural English, 8–16 words, accessibility-friendly, no "Image of" prefix)
  3. Image Title (natural Title Case with keyword)
  4. Subject (concise IPTC taxonomy descriptor)
  5. Image Description (1–2 sentences, 25–45 words, suitable for IPTC and WordPress)
  6. Keywords / Tags (8–12 relevant semantic tags, starting with exact focus keyword)
  7. Comments (one professional sentence)
  8. Pinterest Description (120–180 characters, non-clickbait)
  9. Creator (attribution / IPTC By-line)
  10. Copyright Notice (IPTC Rights / EXIF Copyright)
  11. Credit Line (press / agency credit)
  12. Source (archive / repository identifier)
- **Lossless Metadata Embedding:**
  - **JPG / JPEG:** Safe XMP APP1 packet injection without re-encoding DCT coefficients or touching compressed image pixels (100% original quality preserved).
  - **PNG:** Standard W3C `iTXt` chunk injection for Adobe XMP.
  - **WebP:** Standard RIFF `XMP ` chunk embedding.
- **Safe File Handling:**
  - Never overwrites the original image unless explicitly checked by the user.
  - Defaults to native Windows **"Save As"** file dialog.
  - Native Windows file selection (`.jpg`, `.jpeg`, `.png`, `.webp`).
- **Export Options:**
  - One-click copy for each field.
  - **Copy All Metadata** to clipboard.
  - **Download .txt** summary file (`seo-metadata-[keyword].txt`).
  - Production-ready **HTML5 `<img>` embed snippet**.

---

## BUILD WINDOWS EXE WITH GITHUB ACTIONS

You do **not** need to install Node.js, Rust, or Visual Studio Build Tools on your PC. GitHub builds the app in the cloud on a Windows machine and gives you the files to download.

### Step by step

1. **Create a GitHub repository.** Sign in at [github.com](https://github.com), click **New repository**, give it a name (for example `ImageSEOMetadataGenerator`) and click **Create repository**.
2. **Upload / push this project.** Extract the ZIP first, then upload the *contents* of the project folder so that `package.json`, `src`, `src-tauri` and the hidden-looking `.github` folder sit at the **top level** of the repository (not inside an extra subfolder).
   - Easiest: on the empty repository page click **uploading an existing file**, drag in everything from the extracted folder (make sure the `.github` folder is included), and click **Commit changes**.
   - Or with Git:
     ```bash
     git init
     git add .
     git commit -m "Initial commit"
     git branch -M main
     git remote add origin https://github.com/YOUR-USER/YOUR-REPO.git
     git push -u origin main
     ```
3. **Open GitHub** and go to your repository page.
4. Click the **Actions** tab. (If GitHub asks you to enable workflows, click **I understand my workflows, go ahead and enable them**.)
5. In the left sidebar, open **Build Windows App**.
6. Click **Run workflow** (top right of the run list), choose your branch, and click the green **Run workflow** button.
7. **Wait for the build.** The first run usually takes 10-20 minutes (later runs are faster because of caching). Refresh the page to see the new run appear.
8. **Open the completed workflow run.** A green check mark means success. A red X means the real build failed; open the failed step to see the exact error.
9. Scroll to the **Artifacts** section at the bottom of the run page and download **`ImageSEOMetadataGenerator-Windows`** (a ZIP file). Extract it on your PC.

### What is inside the artifact

| File | Where it comes from |
|---|---|
| `ImageSEOMetadataGenerator.exe` | `src-tauri/target/release/` (the standalone app) |
| `Image SEO Metadata Generator_1.0.0_x64-setup.exe` | `src-tauri/target/release/bundle/nsis/` (recommended installer) |
| `Image SEO Metadata Generator_1.0.0_x64_en-US.msi` | `src-tauri/target/release/bundle/msi/` (MSI installer) |
| `SHA256SUMS.txt` | Checksums of the files above |

The workflow copies only these final files. It never uploads `node_modules` or temporary build files. If one installer type is missing, the others are still uploaded and a warning is shown; if the build itself fails, the workflow fails.

### Windows SmartScreen warning

These installers are **unsigned**, so Windows may show *"Windows protected your PC"* (Microsoft Defender SmartScreen). Click **More info** then **Run anyway**. This is normal for unsigned apps. See "Code signing (future)" below to remove or reduce this warning.

### WebView2

The app uses the standard Tauri WebView2 setup (`downloadBootstrapper`). WebView2 is already installed on Windows 11 and on up-to-date Windows 10. If it is missing, the installer downloads the small WebView2 bootstrapper once during installation. The app itself runs fully offline.

### Notes

- No `package-lock.json` or `Cargo.lock` is included in this ZIP. The first successful GitHub run creates them inside the runner only. To make future builds reproducible, run `npm install` once on any computer and commit the generated `package-lock.json` and `src-tauri/Cargo.lock` (the workflow automatically uses `npm ci` when `package-lock.json` exists).
- The app icons in `src-tauri/icons/` are simple placeholders. To use your own logo, run `npm run tauri icon path/to/logo.png` (a square PNG of at least 1024x1024) on any computer with Node.js, and commit the result.

---

## Optional: Build locally instead

### 📋 System Requirements for Building on Windows

To build the standalone Windows 64-bit `.exe` and `.msi` / NSIS installer, your Windows development machine requires:

1. **Windows 10 or Windows 11 (64-bit)**
2. **Node.js LTS (v20+ or v22+)**
   - Download from: [https://nodejs.org](https://nodejs.org)
3. **Rust & Cargo**
   - Download and run `rustup-init.exe` from: [https://rustup.rs](https://rustup.rs)
   - Choose the default installation option (`1`).
4. **Microsoft Visual Studio Build Tools 2022 (C++ Build Environment)**
   - Download the Visual Studio Installer from: [https://visualstudio.microsoft.com/visual-cpp-build-tools/](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
   - In the installer, select the **"Desktop development with C++"** workload.
   - Ensure the following components are checked:
     - MSVC v143 - VS 2022 C++ x64/x86 build tools
     - Windows 10 SDK or Windows 11 SDK
     - C++ CMake tools for Windows
5. **WebView2 Runtime**
   - Pre-installed on Windows 10 (updated) and Windows 11 by default.

---

## 🚀 Step-by-Step Build Instructions on Windows

### Step 1: Extract the Project
Unzip `image-seo-metadata-generator-desktop.zip` to a folder on your PC (e.g. `C:\Projects\ImageSEOMetadataGenerator`).

### Step 2: Open Terminal
Open **Command Prompt**, **PowerShell**, or **Windows Terminal** and navigate into the project folder:
```powershell
cd C:\Projects\ImageSEOMetadataGenerator
```

### Step 3: Install Frontend Dependencies
```powershell
npm install
```

### Step 4: Run in Desktop Development Mode (Optional)
To test and inspect the desktop application live with hot reload:
```powershell
npm run tauri dev
```
This will compile the Rust backend, start the frontend dev server, and open the native Windows desktop application window.

### Step 5: Build the Production Windows Installer & Executable
To create the standalone Windows `.exe` and the NSIS / MSI installer:
```powershell
npm run tauri build
```

---

## 📁 Output Folder for Executable & Installers

Upon completion of `npm run tauri build`, your production Windows binaries will be generated in:

### 1. NSIS Windows Installer (`.exe` Setup):
```
src-tauri/target/release/bundle/nsis/Image SEO Metadata Generator_1.0.0_x64-setup.exe
```
*This is the recommended standard Windows installer featuring desktop shortcuts, Start Menu entry, and an uninstaller in Windows Add/Remove Programs.*

### 2. Standalone Windows Executable:
```
src-tauri/target/release/ImageSEOMetadataGenerator.exe
```
*Portable binary that can be run directly without running an installation wizard.*

### 3. Windows MSI Installer (WiX):
```
src-tauri/target/release/bundle/msi/Image SEO Metadata Generator_1.0.0_x64_en-US.msi
```
*Enterprise-ready MSI package for enterprise group policy or Intune deployment.*

---

## 🔏 Windows Code Signing & SmartScreen Guide

### Understanding Windows SmartScreen
When distributing a new Windows executable or installer that is unsigned or signed with a newly purchased certificate, Windows Defender SmartScreen may display:
> *"Windows protected your PC — Microsoft Defender SmartScreen prevented an unrecognized app from starting."*

This is standard Windows behavior for binaries that have not yet established reputation or are unsigned. Users can click **"More info"** -> **"Run anyway"**.

### Code signing (future)

Nothing in this repository is signed, and **no certificate is included**. Never commit certificates (`.pfx`), passwords, thumbprints or tokens to the repository. Store them only in **GitHub -> Settings -> Secrets and variables -> Actions**.

> Note: `TAURI_SIGNING_PRIVATE_KEY` is for Tauri *updater* signatures, not Windows Authenticode signing.

**Option A: certificate you can export as a `.pfx`** (follows Tauri's documented approach; untested here because no certificate was available)

1. Base64-encode the certificate on your PC: `certutil -encode certificate.pfx encoded.txt`
2. Create these GitHub Secrets: `WINDOWS_CERTIFICATE` (contents of `encoded.txt`), `WINDOWS_CERTIFICATE_PASSWORD`, and `WINDOWS_CERTIFICATE_THUMBPRINT`.
3. In `.github/workflows/build-windows.yml`, add this step **before** "Build Tauri application", and change the build step to `npm run tauri build -- --config signing.conf.json`:

```yaml
      - name: Import code-signing certificate
        shell: pwsh
        env:
          WINDOWS_CERTIFICATE: ${{ secrets.WINDOWS_CERTIFICATE }}
          WINDOWS_CERTIFICATE_PASSWORD: ${{ secrets.WINDOWS_CERTIFICATE_PASSWORD }}
          WINDOWS_CERTIFICATE_THUMBPRINT: ${{ secrets.WINDOWS_CERTIFICATE_THUMBPRINT }}
        run: |
          New-Item -ItemType Directory -Path certificate | Out-Null
          Set-Content -Path certificate/tempCert.txt -Value $env:WINDOWS_CERTIFICATE
          certutil -decode certificate/tempCert.txt certificate/certificate.pfx
          Remove-Item certificate/tempCert.txt
          Import-PfxCertificate -FilePath certificate/certificate.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString -String $env:WINDOWS_CERTIFICATE_PASSWORD -Force -AsPlainText)
          @{ bundle = @{ windows = @{
              certificateThumbprint = $env:WINDOWS_CERTIFICATE_THUMBPRINT
              digestAlgorithm = "sha256"
              timestampUrl = "http://timestamp.digicert.com"
          } } } | ConvertTo-Json -Depth 5 | Set-Content signing.conf.json
```

**Option B: cloud / hardware-token signing** (Azure Trusted Signing, SSL.com eSigner, DigiCert KeyLocker, etc.). Many certificate authorities no longer issue exportable `.pfx` files. In that case use Tauri's `bundle.windows.signCommand` (for example `"signCommand": "your-sign-tool %1"`, where `%1` is the file to sign) via a `--config` override, and keep the provider's credentials in GitHub Secrets.

Signing removes the "Unknown publisher" label, but SmartScreen reputation can still take time to build, even with a valid certificate.

---

## 🛠️ Troubleshooting Common Windows Build Errors

### 1. `error: linker 'link.exe' not found`
- **Cause:** Visual Studio C++ Build Tools are missing or not in PATH.
- **Solution:** Open the Visual Studio Installer, select **Modify**, check **Desktop development with C++**, and ensure **MSVC v143** and **Windows 10/11 SDK** are installed. Restart your terminal.

### 2. `error: failed to run custom build command for tauri-build`
- **Cause:** Missing build dependencies or older Rust version.
- **Solution:** Run `rustup update` in PowerShell to ensure Rust is at the latest stable version.

### 3. `WebView2Loader.dll` or Runtime Error
- **Cause:** Missing Microsoft Edge WebView2 runtime.
- **Solution:** Download and install the Evergreen WebView2 Runtime from [Microsoft](https://developer.microsoft.com/en-us/microsoft-edge/webview2/).

---

## 🔒 Security Architecture

- **Strict Content Security Policy (CSP):** Scripts, styles, images and connections are limited to the app itself (`default-src 'self'`, `script-src 'self'`, `object-src 'none'`, `base-uri 'self'`). No remote code is loaded and the code base contains no `eval()`.
- **Minimal Tauri capabilities:** Only `core:default`, `dialog:allow-open` and `dialog:allow-save` are granted (see `src-tauri/capabilities/default.json`). The webview has **no direct filesystem permissions** and no shell access.
- **Restricted file access in the backend:** Reading and embedding only accept `.jpg`, `.jpeg`, `.png` and `.webp` files chosen through the native dialog. The original file is never overwritten unless the user explicitly asks.
- **No Network Telemetry:** The application makes zero external API or tracking calls. All algorithms run locally on the user's CPU.
- **No secrets in the repository:** No API keys are used by the app. `.env` files are git-ignored.

---

## 📄 License
Apache-2.0
