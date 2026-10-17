# Holy TempleOS — remaster step 1 (by Minesword Team, based on TempleOS)

Changed files:
- src/Kernel/KMain.ZC — OS name string is now "Holy TempleOS Vx.xx"
- src/Doc/Start.DD, Welcome.DD, AboutZealOS.DD — new name + "made by Minesword Team, based on TempleOS"
- src/System/WallPaper.ZC — top bar shows name, "by Minesword Team | based on TempleOS"
- src/System/ZSplash.ZC — boot splash: new motto list + "Made by Minesword Team / Based on TempleOS"
- src/System/Gr/GrPalette.ZC — new default 16-colour palette (deep indigo base, gold accents)
- src/PersonalMenu.DD — menu header "Holy TempleOS Apps"
- zealbooter/limine.conf — entry "Holy TempleOS LiveCD", resolution 1920x1080

Build: cd build && ./build-iso.sh   (needs Linux, qemu, xorriso, sudo, git)

Step 2 (also untested, not yet built):
- src/Misc/OSInstall.ZC — clearer boot loader prompt (recommends L)
- src/Doc/Start.DD — "Based on TempleOS - made by Minesword Team", Start here / Install links
- src/PersonalMenu.DD — "Holy TempleOS Games" launcher (29 games)
- .github/workflows/build.yml — uploads the ISO as an artifact, timeout 30 min

Build on GitHub: push to a repo's master (or Actions > Run workflow), then download "HolyTempleOS-ISOs".

Step 3 (untested, not yet built):
- src/Once.ZC — friendly live-disc welcome and clearer "install?" question
- src/Misc/OSInstall.ZC — clearer VM/erase warning, "I = fresh install, U = upgrade", friendlier reboot message
- src/Doc/Install.DD — "Easy install (3 steps)" section at the top

Not done: smooth fonts (ZealOS uses fixed 8x8 bitmap text) and a graphical wallpaper (needs testing on a real build).

Step 4 (after first real boot test of the BIOS ISO in QEMU):
- Booted OK: name "Holy TempleOS V2.03.120", Start page text, installer prompts.
- Fixed: PersonalMenu.DD restored to the original (editing it broke Ctrl-M: out-of-memory while loading its binary sprite section).
- New: src/HolyGames.DD - plain-text games menu (29 games), linked from the Start page ("Games").
- Fixed: palette change moved into the palette the OS really boots with (gr_palette_slate); gr32_palette_std restored.
- Fixed: live-disc welcome text also changed in src/Demo/AcctExample/Once.ZC (the copy the live CD uses).
- Note: the BIOS ISO boots at 1024x768; 1920x1080 only applies via Limine (UEFI ISO).

Step 5:
- src/Kernel/KStart16.ZC — BIOS boot now requests 1920x1080 (falls back to 1024x768 if the VBE list has no such mode)
- src/System/Boot/LimineMHDIns.ZC — installed Limine config uses 1920x1080

Step 6 (tested by patching a built ISO in QEMU):
- src/PersonalMenu.DD: "Holy TempleOS Apps" header + games block INSIDE the text part (before the NUL/binary section). Ctrl-M opens fine at 1920x1080.
  (The earlier OutMem crash came from a build where the block had been appended AFTER the binary section.)

Step 7 - Holy Launcher (graphical home screen):
- src/Misc/HolyLauncher.ZC: full-screen launcher with big colored tiles (Install, Welcome, Help, Terminal + 16 games), clock, hover highlight, click to start, Esc = terminal.
- Live disc (src/Once.ZC and src/Demo/AcctExample/Once.ZC): answering N to "install?" opens the launcher.
- src/Doc/Start.DD: button "Open Holy Launcher".
- Tested in QEMU (software emulation): screen renders, hover highlight works, Esc returns to the terminal.
- NOT verified: launching games/Install from a tile (games did not start within 4 minutes even from the plain terminal in the slow emulator).
- Lesson: do not use SettingsPush/WinMax/Pop in the launcher (it froze the terminal on exit).

Step 8 - bright theme + automatic launcher (tested in QEMU by patching a built ISO):
- Bright palette (white background, navy text, colorful accents) in System/Gr/GrPalette.ZC and Home/PaletteEditor/Palettes/Slate.ZC.
- src/Misc/HolyLauncher.ZC v2: gradient header, 20 colorful tiles with drawn icons, hover frame. Starts automatically from src/Once.ZC and src/Demo/AcctExample/Once.ZC (no more install question at boot).
- The Install tile runs HolyInstall: if the disc has no Limine files (the PublicDomain-BIOS ISO is live-only upstream) it shows a message; otherwise runs the installer.
- To INSTALL, use the ZealOS-BSD2-UEFI ISO (it contains Limine-BIOS-HDD.HH).

Step 9 - Launcher v3 (tested in QEMU by patching a built ISO):
- Boot PIN lock (default PIN 0000, change it in Settings). Terminal and Install also ask for the PIN.
- Home (cannot be closed): GAMES, SETTINGS, TERMINAL, INSTALL, HELP, WELCOME.
- GAMES screen with all game tiles and BACK.
- SETTINGS: themes (Bright / Dark / Classic), change PIN, lock now, Wi-Fi / Bluetooth / Network status tiles.
  Wi-Fi and Bluetooth are NOT supported by ZealOS (no drivers); the tiles only say so.
- Bottom panel (move the mouse to the bottom edge): clock, date, Wi-Fi/Bluetooth/Theme/Lock/Reboot buttons, notifications.
- Theme and PIN are kept in memory only (reset on reboot).
- NOT possible in ZealOS: a Chrome-class web browser, Wi-Fi, Bluetooth, real OS-level security (it has no user accounts or memory protection).
- Not verified: Terminal / Install after the PIN, game start from tiles, install on the BSD2-UEFI ISO.

Step 10 - Launcher v4:
- Wi-Fi / Bluetooth / Network tiles and panel buttons removed.
- New FILES (graphical file manager): drive buttons, path bar, UP / NEW / COPY / CUT / PASTE / RENAME / DELETE (two-step) / OPEN / RUN, folders first, sizes and dates, scroll buttons.
  Keyboard: u/Backspace up, n new, c copy, x cut, v paste, r rename, d delete, o/Enter open, e run, j/k move, 1-9 switch drive. Tested on the RAM drive (B:): create folder, copy, paste, delete work. The disc (T:) is read-only.
- Graphical install page (steps + START INSTALL / CANCEL) before the existing installer; it needs the BSD2-UEFI ISO (boot loader files).
- Keys on Home: g games, f files, s settings, i install. Esc goes back to Home from any screen.
- NOT possible: USB flash / phone (ZealOS has no USB driver), real video/music players (no codecs, PC speaker only).

Step 11 - Launcher v5 (windowed desktop):
- Desktop (Home) is permanent: it has no close action; Esc does nothing there.
- Apps open as windows over the desktop: GAMES, FILES, SETTINGS, INSTALL. Each window has minimize, maximize and close buttons,
  can be moved by its title bar and resized by dragging its edges/corners, and has a button in the bottom taskbar.
- PIN is optional: set it in Settings > SET PIN (4 digits). Without a PIN the desktop opens directly. With a PIN the lock appears at boot
  and before TERMINAL / INSTALL. The PIN is saved to C:/Home/Holy.PIN on an installed system (on the live disc it only lasts until reboot).
- Games, Help, Welcome and Terminal open in normal ZealOS Terminal windows (native close / resize).
- Tested in QEMU: desktop, Files window (list, open, drag-resize from the top edge), taskbar button. Not yet tested: dragging by the title,
  maximize/minimize, Settings > SET PIN, Install window.

Step 12 - names and boot errors (after a VirtualBox / USB test):
- Visible "ZealOS" texts in the debugger banner, boot sector, installer, Limine menu entry, docs and tours now say "Holy TempleOS".
- Boot errors now explain the fix: "Boot disc not found. Attach the ISO as an optical drive on the SATA (AHCI) controller. USB boot is not supported."
- VirtualBox: the ISO must be on the SATA controller as an optical drive (not IDE). ZealOS has no USB driver, so it cannot boot from a USB stick.
