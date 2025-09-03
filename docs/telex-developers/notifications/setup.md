---
sidebar_position: 2
title: Installation and  Setup
---

# Installation and Setup

This section explains how to set up the Telex Start notification app for development. The app is primarily built and run on **Windows**, but optional instructions for **Ubuntu** are included at the end.


## Windows Setup (Recommended)

### Step 1: Install Required Tools

Make sure the following are installed:

- **Visual Studio Code**
- **C++ Extension Pack for VS Code**  
  - Open VS Code → Extensions (`Ctrl+Shift+X`) → Search “C++ Extension Pack” → Install

- **Visual Studio Build Tools 2022**  
  - Download from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/)  
  - During installation, select:  
  ✅ Desktop development with C++

- **CMake** version 3.18 or higher. 
  - Confirm with:  
```bash
cmake --version
```

### Step 2: Cloning the Repository
Run this to clone the repository for the telex notification app:

```bash
git clone https://github.com/your-org/telex-start.git
cd telex-start
```


### Step 3: Setting Up vcpkg
If you don’t already have vcpkg installed, run this at the root of the telex-start repo

```bash
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg
git checkout <stable-tag>
bootstrap-vcpkg.bat
```
Then install dependencies using the project’s manifest:
```bash
.\vcpkg\vcpkg.exe install --triplet x64-windows-static
```

Dependencies include:
- wxwidgets
- nlohmann-json
- cpp-httplib (with OpenSSL)
- ixwebsocket

These are defined in the project’s vcpkg.json which should be present in the root of the Telex Start repo.


### Step 4: Build the App with CMake
Open Developer PowerShell for Visual Studio, then From the root of the Telex Start repo, run:

```bash
cmake -S . -B build/windows/debug ^
  -D CMAKE_BUILD_TYPE=Debug ^
  -D CMAKE_TOOLCHAIN_FILE=%CD%/vcpkg/scripts/buildsystems/vcpkg.cmake ^
  -D VCPKG_TARGET_TRIPLET=x64-windows-static

cmake --build build/windows/debug
```
This will generate telexstart_d.exe in the build folder.

> **Required file:** `CMakeLists.txt`.
>
> This file defines the build process and links platform-specific libraries. Ensure it is located in the root of the project.


### Step 5: ▶️ Running the App
Navigate to the build folder and run:

```bash
build/windows/debug/telexstart.exe
```
> The app will launch as a GUI application with a system tray icon.

### Notes
Once the app is running, you’ll be able to:
- See the Telex notication icon in the system tray
- Log in with your Telex credentials
- Start receiving notification events.



