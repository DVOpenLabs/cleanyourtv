# Guided and AI-assisted setup

Launcher settings → **Setup help** offers an optional remote-control wizard. Choose a Home look,
favorite apps, position and readability preferences, then review the draft. Back and Cancel save
nothing. Apply defaults to a **new local profile**, so the previous Home remains available.
This does not create or switch accounts inside streaming apps.

AI is an optional way of preparing that same draft, not a requirement for using Home. The launcher
contains no model, API key, cloud chat, paid inference, microphone collection or AI network client.
You choose whether to share a local app inventory with an assistant you already use. Titles,
wallpaper files, viewing history and TV account information are not included in that inventory.

## Use your assistant

1. Open Setup help → AI setup help to copy the prompt or export a template using Android’s file picker.
   Some TVs have no file picker; the desktop tool below is an alternative with authorized debugging.
2. Ask the assistant what you want your Home to feel like. Supply only the template/inventory you
   choose to share. Exact installed app IDs are required; omit `favorites` to retain the current order.
3. Save its answer as JSON, import it and inspect the review. Unknown fields and unsupported values
   are rejected. Applying a draft requires a choice on the TV; a desktop request cannot apply it.

Example, without changing favorites:

```json
{
  "schema": 1,
  "name": "Evening Home",
  "settings": {
    "experience": "clean",
    "position": "bottom",
    "text": "auto",
    "textSize": 110,
    "strongFocus": true
  }
}
```

Supported Android settings: `experience` (`legacy`, `glass`, `clean`, `cinema`), `position`
(`top`, `middle`, `bottom`), `columns` (2–8), `text` (`auto`, `light`, `dark`), `textSize`
(85, 100, 110, 120 percent), `names` (`theme`, `all`, `selected`, `hidden`), `strongFocus`
(boolean), `idleSeconds` (0, 30, 60, 120). Unspecified settings stay as they are. A shared textSize
sets all three size groups; the manual size editor still offers independent clock/header/name sizes.
Favorite lists contain 1–32 unique visible installed package IDs. Maximum draft size: 16 KiB.

Commands, URLs, wallpapers, API keys, PINs, permissions, profile switching, TV settings and app
installation are outside this contract. Weather and content permissions remain separate explicit
choices. A draft is rejected if the profile or Home configuration changes before applying it.

## Desktop bridge

From this fork’s checkout, with the launcher visible and debugging already authorized:

```sh
python tools/freedom_setup.py --prompt
python tools/freedom_setup.py --validate /path/to/draft.json
python tools/freedom_setup.py --tv <tv-ip> --inventory-out /private/path/inventory.json
python tools/freedom_setup.py --tv <tv-ip> --stage /path/to/draft.json
```

Use `--package` only for the existing product identity on your own build. The inventory output must
be a new file. Staging uses a single-use token that expires after two minutes if it is not opened. The on-TV review
never saves anything until Apply, and still rejects a changed Home/profile revision.
The normal Android shell authorization boundary remains in force. This tool does not turn debugging
on, install software, change Home or contact an AI provider. Keep inventories and drafts private.

## webOS preview

The manual-launch webOS adapter accepts the same draft vocabulary with `glass`, `clean` and `cinema`
looks. `legacy` is Android-specific and is rejected. Favorites must match the reviewed LG app catalog,
not Android package IDs. Native LG installation and app-launch compatibility still require device tests.
See [the webOS preview guide](../platforms/webos/LAUNCHER.md).
