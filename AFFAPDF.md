# AffaPdf (AFFA fork of PdfCraft)

Private AFFA branding of [PdfCraft](https://github.com/storytold/pdfcraft) for WMS.

## License

Upstream is MIT OR Apache-2.0. Keep `LICENSE-*` and `NOTICE`. ArtCraft trademarks in `docs/brand/` must not ship in public AFFA builds.

## Remotes

```bash
# Origin is private GitHub fork:
origin https://github.com/wahyuutomoputra/affa-pdf.git

# Upstream for sync:
git remote add upstream https://github.com/storytold/pdfcraft.git
git fetch upstream
git merge upstream/main
```

## Thin rebrand (done)

- [x] Web title + background -> AffaPdf (`apps/pdfcraft-web/index.html`)
- [x] App title and window sync -> AffaPdf (`crates/ui-egui/src/app.rs`, `crates/ui-egui/src/menus.rs`)
- [x] About dialog -> AffaPdf (`crates/ui-egui/src/dialogs.rs`)
- [x] Accent color -> WMS navy `#2E4262` (`crates/ui-egui/src/theme.rs`)
- [x] Hide public community chrome / Discord button (`crates/ui-egui/src/chrome.rs`)

## Run web (local, for WMS Vite proxy)

```bash
cd apps/pdfcraft-web
trunk serve
# http://127.0.0.1:8766
```

WMS FE can proxy `/affapdf-app` -> that server (same origin for `?host=parent`).

## Release build (WMS prod / static)

```bash
cd apps/pdfcraft-web
trunk build --release --public-url /affapdf-app/
# -> ../../dist/web (index.html + hashed .js / .wasm + snippets/)
```

Prod path on wms2: `/var/www/prod-fe/affapdf-app/` (sibling of `dist/` and `affaword-app/`).

## Host Bridge (?host=parent)

PdfCraft uses the identical `postMessage` protocol as WordCraft:
- `pdfcraft:ready` (app to host): `{ type, version }`
- `pdfcraft:open` (host to app): `{ type: "pdfcraft:open", name, bytes }`
- `pdfcraft:save` (app to host): `{ type: "pdfcraft:save", name, saveAs, bytes }`
- `pdfcraft:dirty` (app to host): `{ type: "pdfcraft:dirty", dirty }`
- `pdfcraft:failed` (app to host): `{ type: "pdfcraft:failed", error }`
