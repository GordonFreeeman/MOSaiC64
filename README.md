# MOSaiC64 0.6.5

Original, self-contained JavaScript C64 emulator and installable PWA. No third-party emulator implementation or runtime library is incorporated. No Commodore ROMs or game images are bundled.

## Running and hosting

Open `MOSaiC64.html` locally, or serve this folder with `index.html` at the root. For Vercel, use this folder as the static project root with no build step. Keep the manifest, service worker and icons alongside the HTML. PWA installation and offline app caching are available on a suitable secure browser origin; ordinary local HTML execution does not register a service worker.

The two HTML files are byte-identical. The service-worker cache is `mosaic64-pwa-v0.6.5-1`. The update does not intentionally reload a running game. Existing settings and save-state identifiers are retained. Storage remains tied to the browser/origin; the supplied settings export/portable-copy features remain available.

## Keyboard default

The stock layout is **Numpad + RCtrl**, on joystick port 2. Numpad 8/2/4/6 are directions, 7/9/1/3 are diagonals, and Right Ctrl fires. Arrow keys are C64 cursor keys. Left Ctrl is C64 Control, not joystick fire.

Both the HTML selector and the preference schema now agree. The old stock global Arrows + Ctrl preset is migrated once when no custom key overrides or modified custom joystick map are present. Custom maps, other chosen presets and per-game profiles are preserved. Choosing Arrows + Ctrl deliberately after the migration remains persistent. Guide → Keyboard has also been corrected.

**Controls → Joystick → Reset joystick bindings** selects the new default for the current control scope. The Profiles page shows whether that scope is global or a particular game.

## Execution profiles

**Machine → Performance → Emulation profile**:

* **Cycle-accurate emulation · compatibility** is the default and retains the existing per-cycle engine. It is the recommended path for timing-sensitive loaders and effects. Its name does not assert perfect emulation of every chip quirk.
* **Fast / Standard · batched emulation** is a new original execution path, not the old frame-skipping option. It groups RAM-only CPU work into short batches (up to 16 CPU clocks before the next instruction boundary, with additional VIC bus stalls). Accesses to memory-mapped I/O synchronize earlier. CIA and SID quiet stretches are advanced arithmetically to the next internal event; enabled drive CPUs catch up in blocks.

Both use normal full-rate PAL video presentation and SID sample production. There is no intentional 25 fps or alternate-frame cap. Fast changes cross-chip ordering, interrupt observation, write visibility and drive interleaving, so sensitive raster effects, bus tricks, custom loaders and timing tests can differ. Use Accurate for those cases. This is not a port or compatibility equivalent of VICE.

Switching the profile does not reset the C64, discard a disk, reset an enabled drive CPU, or restart its in-flight instruction. It flushes pending emulation work and the output audio reserve. A brief audio reprime on switching is expected. Save states preserve the selected path.

The retired preference `performance` originally meant half-rate video. Old settings using it are migrated to Accurate rather than silently opting into approximate timing. Select Fast explicitly to try the new engine.

## Drive power

**Library → Drives & tape → Drive power** has four persistent switches. Only drive 8 starts enabled; drives 9, 10 and 11 start off.

Off means disconnected, not merely empty. Its drive CPU, VIA timers and rotation do not advance, and it cannot pull down the shared IEC cable or answer direct KERNAL requests. The disk stays inserted and remains in the library. Buffered modified track data is flushed before switching off. Powering on restarts only that drive, with its disk still present.

Switching off during a disk operation interrupts that operation, like unplugging the drive. Prefer doing this when it is idle. Importing a disk does not silently turn an off drive back on. Load & run reports which target must be enabled. Hardware autostart now inserts and addresses the same selected target drive. An empty enabled drive still runs its firmware in hardware mode.

Power settings are global, not part of a game's controller profile. New save states preserve all four power states. Old multi-drive snapshots restore their previously connected devices; that is session restoration, not a change to fresh-install defaults.

## Evidence and limits

See `TEST-REPORT.md` and `reports/` for actual checks and measurements. Accurate mode matched the tested v0.6.4 state, pixels and samples with matching drive configuration. Fast is deliberately an approximation and has not been exhaustively game-tested. The browser tests used Chromium with in-memory markup because local navigation was blocked; persistence was modeled across fresh contexts, not certified on Windows or Android. Benchmarks measure an idle BASIC workload, not Skylark gameplay. No universal compatibility or FPS guarantee is made.
