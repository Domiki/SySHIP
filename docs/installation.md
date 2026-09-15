# Installation

## Windows

1. Download `SySHIP-Setup-<version>.exe` from the [Releases](https://github.com/Domiki/SySHIP/releases) page.
2. Run the installer. It's not a one-click installer — you'll be asked to confirm (or
   change) the install directory, and a desktop shortcut is created for you.
3. Launch SySHIP from the desktop shortcut or Start menu.

If Windows blocks the app or quarantines a file on first launch, this is a false positive
from Windows Defender and/or Smart App Control (the app isn't code-signed yet) — see the
[FAQ](faq.md) for the two-step fix.

## macOS

1. Download `SySHIP-<version>.dmg` from the [Releases](https://github.com/Domiki/SySHIP/releases) page.
2. Open the `.dmg` and drag **SySHIP** into the **Applications** folder shown alongside it.
3. Launch SySHIP from Applications (or Launchpad/Spotlight).

Since the app isn't notarized by Apple yet, the first launch will show a "damaged and can't
be opened" Gatekeeper warning — see the [FAQ](faq.md) for the one-time fix.

## First launch

SySHIP ships with a few sample hull files (`kvlcc.stl`, `kvlcc2.stl`, `kcs.stl`). The
HULL → Import tab's file browser opens to that samples folder by default, so you can try
the app immediately without your own geometry — see [Importing a Hull](hull/import.md).
