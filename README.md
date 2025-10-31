# Unity Essentials

This module is part of the Unity Essentials ecosystem and follows the same lightweight, editor-first approach.
Unity Essentials is a lightweight, modular set of editor utilities and helpers that streamline Unity development. It focuses on clean, dependency-free tools that work well together.

All utilities are under the `UnityEssentials` namespace.

```csharp
using UnityEssentials;
```

## Installation

Install the Unity Essentials entry package via Unity's Package Manager, then install modules from the Tools menu.

- Add the entry package (via Git URL)
    - Window → Package Manager
    - "+" → "Add package from git URL…"
    - Paste: `https://github.com/CanTalat-Yakan/UnityEssentials.git`

- Install or update Unity Essentials packages
    - Tools → Install & Update UnityEssentials
    - Install all or select individual modules; run again anytime to update

---

# Directory Attribute

> Quick overview: Add a folder picker to string fields. Stores a project-relative path under Assets, with keyboard activation support.

A small PropertyDrawer that turns a string field into a “Select Directory …” button in the Inspector. The chosen folder is saved as a path relative to your project’s `Assets/` directory.

![screenshot](Documentation/Screenshot.png)

## Features
- One-click folder picker on string fields marked with `[Directory]`
- Stores project-relative paths like `Assets/MyFolder/SubFolder`
- Button label shows the current value (prefixed with `./`)
- Keyboard activation supported when the button has focus
- Inspector-only; no runtime overhead

## Requirements
- Unity Editor 6000.0+ (Editor-only; attribute lives in Runtime for convenience)
- Depends on the Unity Essentials Inspector Hooks module (for keyboard focus helper)

Tip: If the button doesn’t respond to Enter/Space, ensure the Inspector Hooks module is installed.

## Usage
Basic

```csharp
using UnityEngine;
using UnityEssentials;

public class Exporter : MonoBehaviour
{
    [Directory]
    public string OutputFolder; // e.g., "Assets/Builds" after selection
}
```

Accessing the folder at runtime/editor scripts

```csharp
// The field stores a project-relative path under Assets.
// For AssetDatabase APIs, you can use it directly, e.g.:
//   var asset = AssetDatabase.LoadAssetAtPath<Object>(OutputFolder + "/SomeAsset.asset");
// For System.IO operations, convert to an absolute path:
//   var absolute = Path.Combine(Application.dataPath, OutputFolder.Substring("Assets".Length));
```

## How It Works
- CustomPropertyDrawer for `[Directory]` renders a single button next to the field label
- On click, opens a folder panel starting at `Assets/`
- If the selected path is inside the project’s `Assets/` folder, it saves the relative path (e.g., `Assets/MyFolder`)
- If the selected path is outside `Assets/`, the value is left unchanged
- The button label shows either “Select Directory …” or the current value prefixed with `./`

## Notes and Limitations
- Supported field types: `string` only; other types show an inline error
- Selection is restricted to folders under `Assets/`
- Does not create folders; you must create them yourself or via editor scripts
- Serialized as plain string; no validation beyond ensuring the chosen path is under `Assets/`
- Multi-object editing: applies to the currently inspected object’s value

## Files in This Package
- `Runtime/DirectoryAttribute.cs` – `[Directory]` attribute marker
- `Editor/DirectoryDrawer.cs` – Folder picker drawer (relative-to-Assets handling, keyboard activation)
- `Runtime/UnityEssentials.DirectoryAttribute.asmdef` – Runtime assembly definition
- `Editor/UnityEssentials.DirectoryAttribute.Editor.asmdef` – Editor assembly definition (references Inspector Hooks)

## Tags
unity, unity-editor, attribute, propertydrawer, directory, folder, path, inspector, tools, workflow
