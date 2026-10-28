---
name: driving-mac-apps
description: computer use, drive an app, control the Mac, GUI automation, click through the app, launch the app, open the app, desktop app testing, screenshot an app, AppleScript, osascript, print dialog, print layout, app settings
---

# Driving Mac apps

When you control apps on the user's Mac you share the machine with them. They are typing in other windows, the apps hold their settings and files, and some actions (printing, sending, saving) can't be undone.

## Don't take over the machine

- Launch apps without taking focus: `open -g -a "<App>"` or `open -g <path>`. Never bring an app to the front unless the task needs it.
- Check that it worked. If the app still comes to the front, find another way rather than repeating it while the user is typing.
- Use the first route that can do the job. Only the last one takes over the user's screen:
  1. **No UI:** a CLI, an API, or reading and writing the app's files directly.
  2. **A web page that needs a user's clicks and typing:** the harness's built-in browser, or a headless browser such as Playwright. Both run off the user's screen.
  3. **A native app's UI:** AppleScript or accessibility scripting without `activate`, so the app stays in the background.
  4. **Computer use** on the real screen, only when none of the above can do it.
- If you need the screen to yourself for a while, say so before starting and say when you are done.
- Record the PID of every process you launch and stop only those when you finish. Before calling a process "not mine", check how it was launched.

## Change only what you were asked to

- Don't save, export, print, send or submit unless that is the task. If a save dialog appears that you didn't intend, cancel it.
- Don't change app preferences, presets, registered layouts or defaults. When the job needs a setting, set it for this job only (in the dialog, not the app's defaults), and note the original value so you can restore it.
- Treat physical output as irreversible. Before printing or sending, check every constraint the user gave (paper size, borderless or overspray, margins, copies) against what is on screen, and stop if anything doesn't match.

## Report only what you saw

- Describe app state from what you observed this session (a screenshot, a dialog's values, a file on disk), not from what you meant to do.
- If you couldn't verify something, such as whether a setting was saved, say it is unverified. Never tell the user their settings or files are untouched unless you checked.
- When you finish, list anything you changed in the app, even temporarily, and whether you restored it.
