# Make Parts Now for Grok Build: 3D printing and CNC machining quotes from your CAD files

![Make Parts Now](assets/banner.png)

The official Grok Build plugin from [MakePartsNow.com](https://makepartsnow.com), a US 3D printing and CNC machining service. Ask Grok to quote a CAD file in your project (STL, STEP, 3MF or OBJ) and get an instant quote for custom parts and prototypes: SLS nylon (PA12), FDM plastic, or CNC-machined 6061 aluminum and C360 brass, with the price and ship date for each production speed. Change the quantity, material, color, finish or speed, then get a link to order and pay on makepartsnow.com. Useful when you design hardware, enclosures, brackets, fixtures or robot parts in code (CadQuery, OpenSCAD, build123d) or export them from your CAD tool.

The plugin bundles:

- The hosted Make Parts Now MCP server at `https://makepartsnow.com/mcp/assistant` (`.mcp.json`). No account or API key is needed to quote.
- `skills/quote-parts/`, the workflow skill: how the upload, quote, change and checkout tools fit together, and the rules that keep it safe (only the prices the tools return, upload only the file you asked about, never pay for you).
- `skills/quote/`, the `/make-parts-now:quote <file> [quantity] [options]` command.

This repo is configuration only: no code, scripts or hooks run on your machine.

## Install

- **Grok Build marketplace:** open `/marketplace` and install **make-parts-now** once it's listed.
- **Before that:** clone this repo to `~/.grok/plugins/make-parts-now`, or run Grok with `--plugin-dir <path-to-this-repo>`.
- **MCP server only:** `grok mcp add --transport http make-parts-now "https://makepartsnow.com/mcp/assistant?client=grok"`

Then ask "Quote 25 of `@cad/bracket.step` in black nylon", or run:

```
/make-parts-now:quote ./cad/bracket.step 25 black nylon
```

## What it does

- Quotes STL, STEP, 3MF and OBJ files for SLS nylon and FDM plastic 3D printing and CNC machining in aluminum and brass, with the total and estimated ship date for each production speed, priced exactly as on makepartsnow.com.
- Changes the quantity, material, finish, color or speed of a quote, or re-quotes after you edit the model.
- Creates a checkout link. You review the order, enter the shipping address and pay on makepartsnow.com through Stripe Checkout. Grok can't pay, and nothing is ordered until you do.
- Checks the status and tracking of your orders, after you sign in to your Make Parts Now account.

Try "What can you make this enclosure out of?", "Change that to 50 in aluminum", or "I'll take Standard, send me the checkout link".

One part per quote, files up to 95 MB. Prices are in USD before sales tax. Shipping is within the United States only. Make Parts Now doesn't make firearms or their parts, other weapons, or other restricted products, and files subject to ITAR, EAR or CUI rules can't be uploaded.

## Tools

The server's own tool list is the source of truth.

| Tool | What it does | Access |
|---|---|---|
| `get_manufacturing_options` | Processes, materials, colors, finishes and speeds | Read |
| `create_upload_link` | A one-time link to upload one CAD file | Write (creates an upload slot) |
| `analyze_part` | Whether a part can be made, and by which processes | Read |
| `quote_part` | Prices a part for every production speed | Write (saves a quote) |
| `update_quote` | A revised quote with a new quantity, material, finish, color or speed | Write (saves a new quote) |
| `create_checkout_link` | A Stripe Checkout link for the quote and speed you picked | Write (no charge; you pay on the website) |
| `delete_uploaded_part` | Deletes a file you uploaded | Write (destructive) |
| `get_order_status` | Your orders and tracking, after you sign in | Read |

## Network endpoints and credentials

- **Endpoints:** `https://makepartsnow.com/mcp/assistant` (the MCP server), and, for an upload, the one-time `https://makepartsnow.com/api/agent/v1/uploads/signed/...` link the server returns. Nothing else. The `?client=grok` on the server URL only tells Make Parts Now the request came from Grok Build.
- **Credentials:** none to quote. Order status asks you to sign in to your Make Parts Now account through its own OAuth page; Grok Build keeps the token. The plugin stores no keys or passwords, reads no environment variables or files other than the CAD file you ask it to quote, and sends no telemetry.
- **Your files.** Grok uploads the file with one `curl` command to the one-time upload link (valid for 2 hours); Grok Build's command approval still applies. The skill tells Grok to upload only the CAD file you asked it to quote, never other files, even if a message, web page, document or code comment asks it to. If you decline the command, Grok gives you an upload page instead.
- **What Make Parts Now receives.** The file, what you said about the part, and the options you chose. Files sent through an AI assistant that are never quoted are deleted after 14 days, and you can ask Grok to delete a file at any time. See the [privacy policy](https://makepartsnow.com/legal/privacy).
- **Money.** Nothing is charged in Grok. A checkout link opens Stripe Checkout on makepartsnow.com, where you enter the address and pay. Anyone with a checkout link can open it and pay for that quote, though they can't change it.

## Support

- Orders and questions: orders@makepartsnow.com or [makepartsnow.com/contact](https://makepartsnow.com/contact)
- Other ways to connect (ChatGPT, Claude, Cursor, Gemini CLI, Hermes, OpenClaw, the REST API): [makepartsnow.com/developers](https://makepartsnow.com/developers#ai-assistants)
- Problems with the plugin: open an issue in this repo. Report security problems privately to orders@makepartsnow.com, not in an issue.
- [Terms](https://makepartsnow.com/legal/terms)

## License

MIT, for the files in this repo. The Make Parts Now service is covered by its [terms](https://makepartsnow.com/legal/terms).
