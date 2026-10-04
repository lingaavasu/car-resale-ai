# Image Fix

This project uses bundled local vehicle images under `public/images/`.

- Hero image: `/images/coupe-studio.png`
- Auth background: `/images/coupe-studio.png`
- All 13 vehicle assets have PNG versions for reliable browser rendering.
- SVG source artwork is also retained and validated.
- No Pexels/CDN download is required for the application images.

After extraction:

```powershell
npm install
npm run dev
```

Then open `http://localhost:3000`.
