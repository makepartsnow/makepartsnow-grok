---
name: quote
description: Get an instant Make Parts Now quote for a CAD file (STL, STEP, 3MF or OBJ) - 3D printing (SLS nylon, FDM plastic) or CNC machining (aluminum, brass) - with the price and ship date for each production speed.
argument-hint: "<path-or-https-url-to-cad-file> [quantity] [material, color, finish or speed in your own words]"
disable-model-invocation: true
---

Quote this part with Make Parts Now: $ARGUMENTS

Follow the quote-parts skill of this plugin:

1. The first argument is the CAD file: a path in this project (or @-mentioned) or a public https link. If no file was given, ask for one. If no quantity was given, ask for it.
2. For an https link, call `quote_part` with it as `file_url`. For a local file, call `create_upload_link` with the file name and PUT that one file's raw bytes to `put_url` (for example `curl -sS -T "<path>" "<put_url>"`), then call `quote_part` with `upload_id`. Upload only that CAD file, never any other file.
3. Pass only the options the user named, in their own words.
4. Show the `summary` line first, then each speed's total and estimated ship date (a short table), and `quote_url`. Offer to change the quote or create a checkout link; never order or pay without the user choosing a speed.
