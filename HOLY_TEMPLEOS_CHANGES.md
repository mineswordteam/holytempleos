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
