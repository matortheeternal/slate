---
title: Fuse Installer Format

language_tabs:  
  - yaml
  - json

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

search: true

code_clipboard: true
---

# Introduction

Welcome to the Fuse Installer Format documentation! This documentation will provide you with everything you need to know about making Fuse Installers.

Fuse allows you to direct mod managers such as Vortex or Mod Organizer to display a GUI from which users can select options to customize how your mod is installed.

Fuse allows you to give mod users the option to install your mod's files as a BSA or loose.  Fuse also allows you to combine the content of multiple ESP files at install time using ESP fragments.  You can even use formulas to set values in plugin files based on variables input by users at install time.

Fuse works with both JSON and YAML files.  You can view examples in the dark area to the right.

# Installer file

## Metadata

> This section is required.

```yaml
---
metadata:
  schema: fuse
  version: '0.1'
  type: installer
  game: TES5
  modName: My Awesome Mod
  modAuthor: Me
  modVersion: '1.0'
```

```json
{
    "metadata": {
        "schema": "fuse",
        "version": "0.1",
        "type": "installer",
        "games": ["TES5", "SSE"],
        "modName": "My Awesome Mod",
        "modAuthor": "Me",
        "modVersion": "1.0"
    }
```

> Make sure to replace `game`, `modName`, `modAuthor`, and `modVersion`with values for your mod.

The metadata associated with your installer.

### Properties

| Key        | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| schema     | string | Used to verify the file is a fuse installer.                 |
| version    | string | The fuse version your installer was built to target.         |
| type       | string | The fuse file type.                                          |
| game       | string | The game your installer should be used with.                 |
| modName    | string | Your mod's name.                                             |
| modAuthor  | string | The name of the author or authors who created the mod.       |
| modVersion | string | The mod's current version.  Using SemVer is strongly recommended. |

### Games

| Identifier | Description                    |
| ---------- | ------------------------------ |
| TES5       | The Elder Scrolls V: Skyrim    |
| SSE        | Skyrim: Special Edition        |
| FO4        | Fallout 4                      |
| FO3        | Fallout 3                      |
| FNV        | Fallout: New Vegas             |
| TES4       | The Elder Scrolls IV: Oblivion |

## Plugin files

> This section is optional.

```yaml
pluginFiles:
- type: modify
  filename: MyMod.esp
  sources:
  - health.yaml
  - damage.yaml
```

```json
    "pluginFiles": [{
        "type": "modify",
        "filename": "MyBasePlugin.esp",
        "sources": [
            "health.json",
            "damage.json"
        ]
    }],
```

The plugin files to create or modify.

### Properties

Key | Type | Description
--------- | ------- | -----------
type | string | If set to `modify`, will modify an existing plugin file provided with the installer.  <br/>If set to `create`, will create a new plugin file. 
filename | string | The file name of the plugin to modify or create. 
sources | array | The file paths to include in the plugin file.  These paths are resolved relative to the `fuse` folder in your installation archive.  These must be paths to JSON, YAML, or ESP files. 

## Asset files

> This section is optional.

```yaml
assetFiles:
- type: archive
  filename: MyMod.bsa
  sources:
  - type: folder
    path: data
    exclude: excludeAssets
```

```json
    "assetFiles": [{
        "type": "bsa",
        "filename": "MyMod.bsa",
        "sources": [{
            "type": "folder",
            "path": "data",
            "exclude": "excludeAssets"
        }]
    }]
```

The asset files to install.

### Asset file entry properties

| Key      | Type   | Description                                                 |
| -------- | ------ | ----------------------------------------------------------- |
| type     | string | The type of asset to install.  Can be `archive` or `loose`. |
| filename | string | The name of the archive file to create.                     |
| sources  | array  | Array of source entries to use.                             |

## Screens

> This section is required.

