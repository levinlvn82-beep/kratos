# LAUNCH CHECKLIST & DEPLOYMENT VERIFICATION

This checklist reflects the true, verified operational status of the TrustLens prototype.
Per engineering integrity guidelines, only items verified as completed are checked `[x]`.
Items requiring external domain registration, owner credentials, or legal counsel actions remain unchecked `[ ]`.

---

## Shipped Status Checklist (Per Brief Specification)

- [ ] **Custom domain connected over HTTPS**: Project configured for Vercel with target domain `power` / `power.vercel.app`. Awaiting final DNS record pointing and live HTTPS certificate provisioning by owner.
- [x] **SVG and PNG favicons, Open Graph image, `site.webmanifest`**: Configured and verified in `public/` (`favicon.svg`, `favicon.png`, `og-image.png`, `site.webmanifest`) and linked in `index.html`.
- [x] **`robots.txt` and `sitemap.xml`**: Active in `public/`, referencing `https://power.vercel.app/sitemap.xml`.
- [x] **No builder or "made with AI" badge in the shipped site**: Zero external builder watermarks, AI badges, or fake promotional branding.
- [x] **Privacy, Terms and Cookie pages live**: Fully rendered in `LegalPages.tsx` with zero cookies, memory-only session traces, DPDP Act 2023 alignment, and qualified legal disclaimers.
- [ ] **Business details filled in**: Owner name ("Levin"), hosting platform ("Vercel"), and target domain ("power") are populated. Postal mailing address, contact email, and grievance officer contact remain pending owner provision (strictly uninvented per brief rules).
- [x] **Image, font and icon licenses listed**: Fully enumerated in `CREDITS.md` (OFL for Silkscreen, Space Mono, and Inter; MIT for React Bits; 100% original inline SVGs).
- [ ] **`XAI_API_KEY` set only in the host's environment variables**: `.env` is strictly gitignored, `.env.example` shipped with empty values, client code has zero keys, and `api/explain.ts` is configured to read from Vercel's server environment. Awaiting owner key entry in Vercel project settings dashboard.
- [ ] **Trademark search for "TrustLens" done by the owner**: Legal availability search for "TrustLens" in target jurisdictions to be completed by owner (Levin).
- [ ] **Hackathon AI-disclosure rules checked by the owner**: Hackathon submission rules regarding AI-assisted generation and LLM API usage to be certified by owner (Levin).

---

## Technical Build & Asset Verification
- [x] **Vite + React + TypeScript static build**: Compiles cleanly with zero errors (`dist/`).
- [x] **Single-file static build**: Bundled via `vite-plugin-singlefile` into `dist-singlefile/index.html`.
- [x] **Self-hosted OFL typography**: `@fontsource/silkscreen`, `@fontsource/space-mono`, and `@fontsource/inter` bundled locally. Zero Google Fonts or external network requests.
- [x] **Zero emoji in UI**: All visual status indicators use original inline SVGs with distinct shapes for each uncertainty level (HIGH, MEDIUM, CONFLICT, ABSTAIN, BLOCKED).
- [x] **Deterministic offline engine**: Okapi BM25 ranking, regex privacy scanner, and uncertainty gating operate 100% client-side without any server dependencies.
- [x] **Security Headers & CSP**: `vercel.json` provides strict Content-Security-Policy (`connect-src 'self'`), `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `Referrer-Policy`, and `Permissions-Policy`.
- [x] **Security Contact**: Configured at `public/.well-known/security.txt`.
- [x] **Custom 404 page**: Shipped at `public/404.html` matching design language with zero external dependencies.
