# MOSaiC64 PWA 0.6

This folder is ready to deploy as a static Progressive Web App on Vercel.

## Deploy

Upload the contents of this folder as the project root. No build command or framework is required. `index.html` is the application entry point.

Vercel serves the deployment over HTTPS, which is required for browser PWA installation. After the first successful load, `sw.js` keeps the application shell and icons available offline. Game images, settings, profiles and saves remain browser-side data handled by MOSaiC64 itself.

## Included files

- `index.html` — deployment entry point
- `MOSaiC64.html` — identical convenience copy for direct local use
- `manifest.webmanifest` — PWA metadata and install icons
- `sw.js` — offline application-shell service worker
- `icons/` — regular, maskable and Apple-touch icons
- `screenshots/` — install-prompt screenshots
- `vercel.json` — update-safe cache headers for the service worker and app shell

Local `file://` use remains supported by the emulator, but PWA installation and service workers require HTTPS or localhost.
