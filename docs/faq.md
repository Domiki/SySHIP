# FAQ

## The app hangs on "Starting..." forever, or `syship-backend.exe` keeps disappearing (Windows)

This happens because `syship-backend.exe` isn't code-signed yet, so Windows treats it as an unknown, unverified program. This isn't a virus — it's a false positive from two **independent** Windows security layers, and both need to be addressed:

**Step 1 — Restore it from Windows Defender and exclude the folder**

1. Open **Windows Security → Virus & threat protection → Protection history**.
2. Find the quarantined `syship-backend.exe` entry and click **Restore**.
3. To stop it from being quarantined again, add an exclusion: **Virus & threat protection → Manage settings → Add or remove exclusions → Add an exclusion → Folder**, and select the SySHIP install folder (by default `%LocalAppData%\Programs\SySHIP`).

**Step 2 — Check Smart App Control**

If the app is restored but still won't launch (or you see "The system cannot execute the specified program"), **Smart App Control** is likely blocking it. This is a separate, stricter layer that a Defender exclusion does not bypass — it's on by default on new Windows 11 installs and blocks any app it doesn't recognize as trusted.

1. Open **Settings → Privacy & security → Windows Security → App & browser control → Smart App Control**.
2. Turn it **Off**.
   - Note: once turned off, Smart App Control can only be turned back on by resetting or reinstalling Windows.

After both steps, relaunch SySHIP.

We're working on getting the app properly code-signed, which will resolve this permanently.
