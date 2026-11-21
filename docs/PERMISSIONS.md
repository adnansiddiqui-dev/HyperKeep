# Permissions

HyperKeep asks only for what it uses. Nothing is granted silently; every Xiaomi switch is opened
for the user to flip.

| Permission / switch | Required? | Why | Where the user grants it |
|---|---|---|---|
| **Modify system settings** (`WRITE_SETTINGS`) | Yes, to change the list | Android's gate for writing `Settings.System`; with HyperKeep's target level it also covers the non-public `MILLET_NO_RESTRICT_APP` key | HyperOS permission review on first launch, or Settings → *Modify system settings* |
| **Autostart** (MIUI app-op 10008) | Yes, for background repairs | Without it HyperOS refuses to start HyperKeep's job when its process isn't running (`AutoStartManagerService: Reject service`) | HyperKeep's App info page (one switch) |
| **Battery saver: No restrictions** for HyperKeep | Strongly recommended | Stops Greezer freezing HyperKeep; puts it in the EXEMPTED standby bucket so its trigger job isn't deferred | HyperKeep's battery page (deep link) |
| Access list of installed apps (`com.android.permission.GET_INSTALLED_APPS`) | Recommended | China HyperOS can hide other apps from the app list without it | Permission review / permission editor |
| Internet (`INTERNET`) | For updates | Asks HyperKeep's GitHub release for the newest version (about once a day, in a job Android runs when the phone is online, even if HyperKeep is closed; or when you tap Check) and downloads it when you tap Install. Sends nothing about you or your phone; you can turn automatic checks off | Install-time |
| Install apps (`REQUEST_INSTALL_PACKAGES`) | For updates | Hands HyperKeep's own update to Android's installer; you confirm every install | Install-time; the installer asks to confirm |
| Notifications (`POST_NOTIFICATIONS`) | Optional | Alerts when an app couldn't be put back, and one notification when a new version is out | System notification settings |
| `RECEIVE_BOOT_COMPLETED` | Optional | "Start after reboot"; the receiver component is disabled when the option is off | Install-time |

On Android 17, declaring `INTERNET` also adds `ACCESS_LOCAL_NETWORK` (HyperOS: *Connect to nearby
devices*). HyperKeep never talks to devices on your network, so leave it off.

Not requested: `QUERY_ALL_PACKAGES` (package visibility filtering doesn't apply to
HyperKeep's target level), location, storage, accessibility, overlay, device admin,
`WRITE_SECURE_SETTINGS`.

## What is impossible without more privilege

- Setting Xiaomi's per-app battery policy for another app programmatically, in either
  direction (its provider requires a signature permission; even the shell is refused). HyperKeep
  only *reads* its effects: it warns when an app you keep is on *No restrictions* and opens that
  app's battery page so you can choose *Battery saver (recommended)*.
- Unlocking an app in Recents. Xiaomi's lock call refuses every app that isn't a system app
  (verified: `Permission Denial`), and the saved copy of the list (`locked_apps`) is only read
  when the phone starts. HyperKeep only *reads* the locks and shows you how to unlock a kept app.
- Seeing packages in Second space / Dual apps, or the ~34 packages HyperOS hides from all apps.
- Overriding HyperOS's night-time freeze or GMS network limits.
- Writing `MILLET_NO_RESTRICT_APP` from an app targeting API ≥ 23 (see RESEARCH).
