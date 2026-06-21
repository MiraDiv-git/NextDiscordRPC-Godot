# Next Discord RPC

An open-source GDExtension plugin for **Godot Engine 4.x** that brings modern Discord Rich Presence integration into your game using the new, lightweight **Discord Social SDK**.

[![License: MPL 2.0](https://img.shields.io/badge/License-MPL_2.0-brightgreen.svg)](https://opensource.org/licenses/MPL-2.0)
[![Godot](https://img.shields.io/badge/Godot_Engine-4.x-blue.svg)](https://godotengine.org/)


## 📌 Navigation

- [About](#-about)
- [Download & Installation](#-download--installation)
- [API Documentation](#-api-documentation)
- [Building from Source](#-building-from-source)
- [Getting Discord SDK](#-getting-discord-sdk)
- [License](#️-license)
- [Contributing](#-contributing)


## 📖 About

**Next Discord RPC** is built for engineering pragmatism. Unlike outdated solutions based on the deprecated [Discord Game SDK](https://docs.discord.com/developers/developer-tools/game-sdk), this plugin utilizes the current [Discord Social SDK](https://discord.com/developers/social-sdk) via native C++ (GDExtension). 

### Key Features:
- **Pure Performance:** Written in C++ utilizing `godot-cpp`, ensuring minimal overhead and native execution speed.
- **Steam Deck & Linux Friendly:** Uses modern IPC channels that work flawlessly under Proton/Wine layers without breaking context.


## 📥 Download & Installation

1. Navigate to the [Releases](https://github.com/MiraDiv-git/NextDiscordRPC-Godot/releases) page and download the latest archive.
2. Copy the `NextDiscordRPC` directory into your Godot project's `addons/` folder:
   ```text
   your-project/
   └── addons/
       └── NextDiscordRPC/
    ```
3. Open Godot, navigate to **Project -> Project Settings -> Plugins** and enable **Next Discord RPC**.
4. In **Project Settings**, find the **Discord RPC** category and set your **Application ID** 
   (obtained from the [Discord Developer Portal](https://discord.com/developers/applications)).
> [!NOTE]
> You may need to restart Godot to enable the plugin

## 🛠 Building from Source
To keep the repository clean and legal, all proprietary Discord binaries are completely excluded from the source tree. Follow these steps to compile the plugin locally:

### Prerequisites
- SCons build system installed.
- A compatible C++17/C++20 compiler (GCC/Clang on Linux, MSVC on Windows).

### Step-by-Step Setup

1. Clone the repository with its submodules:
```bash
git clone --recursive https://github.com/MiraDiv-git/NextDiscordRPC-Godot.git
```
2. Download the official Discord Social SDK from the [Discord Developer Portal](https://discord.com/developers/home).
3. Follow [THIS TUTORIAL](#-getting-discord-sdk) to place the SDK header files and libraries into the correct directories, so the linker can see them.

### Compilation
You can use the automated build.sh script or run SCons manually.

If you need to build manually, you can use one of platforms:
- windows
- linux

and one of targets:
- template_debug
- template_release

For example:
```bash
scons platform=windows target=template_release
``` 
Compiled .so or .dll artifacts will be generated in `project/addons/NextDiscordRPC/bin/`.


## 📦 Getting Discord SDK

### Step-by-Step Guide:

1. Log in to the [Discord Developer Portal](https://discord.com/developers/home).
2. Create a new application (this is mandatory to obtain your `App ID`).
3. In the left sidebar, navigate to **Games -> Social SDK**.
4. Fill out the application form (this is a formality, access is granted instantly).
5. Head over to the **Downloads** section and grab the latest zip archive.
   - *I highly recommend using version **1.9.15780**, as the plugin is actively built and tested against it.*

### Directory Hierarchy Setup
Unpack the contents of the downloaded SDK archive into `src/lib`.<br>
Finally, this should contain:
```text
NextDiscordRPC
├── scr
│   └── lib
│       ├── discord_social_sdk
│       └── godot-cpp
```


## 📄 API Documentation

The plugin automatically manages its own lifecycle (initialization, event loops via callbacks, and graceful shutdown) in the background when enabled. 

As a developer, you only need to interact with the global singleton interface using these **2 primary methods** to control your game's status:

### Methods

* `set_activity(String details, String state)`
  Sets the main text fields of the Rich Presence status (e.g., "In Main Menu", "Exploring Voxel World").
* `set_large_image(String key, String text)`
  Configures the large asset image key (configured in your Discord Developer Dashboard) and its hover tooltip text.

### 💡 Example Usage (GDScript)

```gdscript
extends Node

func _ready() -> void:
    # Set the initial game status when the level loads
    Discord.set_activity("Surviving the night", "Level 14")
    Discord.set_large_image("game_logo", "The coolest logo!")
```

### 🧱 Under the Hood (For Contributors)

If you are modifying the core GDExtension source code, the following internal methods are registered in ClassDB and handled automatically by the plugin's lifecycle manager:

* `initialize(app_id)`: Establishes the core socket connection.
* `run_callbacks()`: Ticks the underlying Discord SDK event loop inside the engine's main processing thread.
* `set_timestamp_start()`: Sets the session timer.
* `shutdown()`: Gracefully tears down the pipeline on exit.
* `clear_activity()`: Clears the active Rich Presence status from the user's Discord profile instantly.


## ⚖️ License

The source code of this plugin is licensed under the Mozilla Public License 2.0 (MPL 2.0).<br>
You can freely use, modify, and distribute this plugin, even in commercial closed-source games.<br>
Any modifications or bug fixes made strictly to this plugin's source files must be made public under the same MPL 2.0 license.

>[!NOTE]
> This project is an independent wrapper and is not affiliated with Discord Inc. The Discord Social SDK binaries belong entirely to Discord Inc.


## 🤝 Contributing

Contributions are heavily welcome! Especially if you want to optimize the SCons architecture, clean up the C++ pipeline, or fix cross-platform linker edge cases.

Seriously, pull requests are appreciated. Writing raw C++ boilerplate can be an absolute pain. Let's make this wrapper better together.