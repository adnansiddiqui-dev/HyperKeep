# Using HyperKeep

## First launch

1. HyperOS first shows its own **"Review permissions"** list for HyperKeep (Xiaomi shows it for
   apps like this; HyperKeep can't change it). Only the first two switches matter: turn on
   **Access list of installed apps** and **Modify system settings**, leave everything else off,
   and tap **OK**.
2. If either of those two is off, HyperKeep opens on **"Two permissions to start"** and goes no
   further. Each permission has an **Allow** button that opens the right page:
   - *Modify system settings* opens Android's page with one switch, **Allow modifying system
     settings**.
   - *Access list of installed apps* opens Xiaomi's permission page: tap **Other permissions**,
     then turn on **Access list of installed apps**.

   Come back to HyperKeep and the screen clears by itself. (It comes back the same way if one is
   ever turned off later.)
3. Read the two short pages.
4. On **Two switches, then you're done**, tap each row, switch it on in Xiaomi's page, and come
   back. Each row turns to **Done** by itself (HyperKeep re-checks as you return):
   - **Autostart for HyperKeep**: lets HyperKeep fix the list while it's closed.
   - **Battery: No restrictions**: so HyperOS never freezes HyperKeep itself.

   **Start HyperKeep** only works once both are done.
5. Tap **Start HyperKeep**. Home opens with nothing turned on yet: choose your apps there,
   starting with **Google Play services** (it delivers most apps' notifications), then your chat
   and mail apps.

## Every day: one screen, one switch per app

- The **top card** says, in one sentence, whether everything is fine ("Notifications are
  flowing") or the one thing to tap next. Your kept apps ride on a flowing line at the bottom of
  the card; an amber ring and dot mean that app needs a tap.
- **Keep flowing** lists your apps (it appears once you've turned one on). Each row says
  *Flowing*, or what's wrong:
  - *Autostart is off · Tap to fix*: tap the row, then **Open settings**, switch **Autostart**
    on and come back. HyperKeep confirms with "All set".
  - *Removed by Xiaomi · Tap to fix*: tap the row to put it back right away (HyperKeep also does
    this by itself within seconds).
  - *No restrictions · Tap to save battery*: the app's Xiaomi battery setting is *No
    restrictions*. HyperKeep already stops HyperOS from freezing it, so that setting only uses
    extra battery (it also lets the app skip Android's Doze). Tap the row, then **Open
    settings**, choose **Battery saver (recommended)** and come back.
  - *Locked in Recents · Tap to unlock*: the app is locked in Recents, which keeps it running
    all the time. HyperKeep already keeps its messages coming, so the lock only uses memory and
    battery. Open Recents, long-press the app, tap the lock to unlock it, and come back.
- **Suggested** lists popular apps that often miss notifications on HyperOS (messaging, social,
  email, work chat, calls), only the ones installed on your phone that you don't keep flowing
  yet. Flip one on, or tap **Turn on all**. HyperKeep never adds them by itself.
- **Other apps** lists your other apps. Flip the switch to start keeping one flowing.
  **Show system apps** reveals background apps too; searching always searches everything.
- **Turning an app on** adds it to Xiaomi's no-freeze list immediately. If its Autostart is off,
  or its battery is set to *No restrictions*, HyperKeep shows what to change (both are on the
  same Xiaomi page). Xiaomi only lets *you* change these, so HyperKeep opens the exact page
  instead of changing them. If the app is locked in Recents, the same sheet ends with a step
  to unlock it there.
- **Turning an app off** takes it off the list. If its Autostart is on, HyperKeep offers to open
  the page so you can turn that off too (optional). An app on *No restrictions* stays on the
  list, because Xiaomi keeps it there for that setting.
- Tap an app's name for its page: the same switch, and a short checklist (on the list,
  Autostart, battery, and a Recents lock if it has one).
- The **⟳** button at the top checks for a new HyperKeep right away; if there is one, it opens the
  update (it shows a download icon with a dot while an update is waiting).
- The **ⓘ** button (grouped with Settings) opens **About**: HyperKeep's GitHub page and the developer's links (website,
  LinkedIn, Facebook, WhatsApp).

## Settings

- **Protect in the background**: the main on/off switch.
- **Instant fixing**: fixes in under a second using a small silent notification. Only needed if
  notifications are still late.
- **Start after restart** and **Alerts** (only when an app drops off and can't be put back).
- **What HyperKeep needs**: the status of each permission; tap a row to open its page.
- **Appearance**: System, Light or Dark.
- **Updates**: your version, **Check** (or **Update** when a new version is out) and **Check
  automatically** (once a day while HyperKeep is open; it only asks GitHub for the newest version).
- **Advanced** (for troubleshooting): History, Xiaomi's list (the exact value, add by package
  name, restore backups including the original), Smart/Eco checking, Fix automatically, Reconnect
  Google push, Export/Import, Diagnostics.

## If something looks wrong

- *"One permission needed"* → tap **Allow** and turn on **Modify system settings**.
- *"Turn on Autostart for HyperKeep"* → tap **Open settings** and switch Autostart on.
- *"One battery setting"* → choose *No restrictions* for HyperKeep. HyperKeep is the only app
  that needs it.
- *"<app> uses extra battery"* → tap **Fix** and choose *Battery saver (recommended)* for that
  app.
- Notifications still late for one app → check that its row says *Flowing* (Autostart on). If
  it does, try **Instant fixing** in Settings.

## Updating HyperKeep

1. When a new version is out, Home shows **Update available** (and Settings → Updates says so).
   Tap **Update**, read *What's new*, then tap **Install update**. HyperKeep downloads it from its
   GitHub page and checks it before installing.
2. Xiaomi's installer takes over. On China ROMs it asks several times: **Continue** → *Couldn't
   find ICP registration info* → **Install** → tick *I understand this app wasn't security-checked*
   → **Update** → *Unverified app* → **Install anyway**.
3. Done: HyperKeep is the new version. Your apps, settings and permissions stay.

An update that adds a permission shows HyperOS's
**Review permissions** list once more. Turn on the first two again (**Access list of installed
apps**, **Modify system settings**), leave the rest off (including *Connect to nearby devices*),
and tap **OK**. Tap **Later** in the update sheet to hide the Home card for that version.

## Undo everything

Settings → turn off **Protect in the background**. Settings → Advanced → Xiaomi's list → Backups →
*Original value* → Restore. Then uninstall; HyperOS removes HyperKeep from its list by itself.
