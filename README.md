# TrustLens: Closing the Trust Gap in AI Assistants

> **A deterministic browser-native RAG engine with Okapi BM25 retrieval, explicit uncertainty gating, multi-version conflict resolution, and automated privacy redaction.**

---

## 1. Executive Overview

Most current enterprise AI assistants operate as opaque probabilistic black boxes: they hallucinate plausible facts when evidence is missing, silently interpolate between conflicting document revisions, and leak confidential identifiers (such as salaries, credit card numbers, or social security numbers) into model context windows.

**TrustLens** replaces black-box inference with a deterministic retrieval-and-gating pipeline:
- **Verbatim Evidence**: Retrieves exact passages from verified corpus documents using plain-code Okapi BM25.
- **Unambiguous Uncertainty Gating**: Evaluates retrieval scores against explicit mathematical thresholds, terminating in one of five distinct states: `HIGH`, `MEDIUM`, `CONFLICT`, `ABSTAIN`, or `BLOCKED`.
- **Conflict Resolution**: Detects contradictory policies across document versions (e.g. 30-day vs. 14-day refund policies) and surfaces both with version dates instead of picking one silently.
- **Client-Side Privacy Scanners**: Automatically masks confidential fields as `[REDACTED: TYPE]` and blocks direct adversarial extraction probes.
- **Optional Grok Explain Layer**: Opt-in plain-language rephrasing powered by xAI, strictly quarantined behind zero-trust server guards and lexical grounding validators.
- **100% Offline Capable**: In standard deterministic mode, TrustLens executes entirely inside browser memory with zero network dependencies.

---

## 2. Mandatory Dummy-Data & Synthetic Corpus Disclaimer

> **IMPORTANT NOTICE:** All corporate policies, employee names, account numbers, salaries, and security incident reports in the TrustLens corpus are **100% fictional and synthetic**.
>
> - **Social Security / Tax Numbers**: Clearly invalid test formats such as `000-12-3456`.
> - **Payment Cards**: Standard test card PANs (e.g., `4532-1234-5678-9012`, `4000-1234-5678-9010`).
> - **Email Addresses**: Strictly restricted to the IANA-reserved `example.com` domain.
> - **Phone Numbers**: Clearly invalid fictitious ranges (`555-0100` to `555-0199`).
> - **API Keys**: Synthetic tokens explicitly prefixed with `DUMMY-AKIA-`.
> - **Personnel & Entities**: Fictional names ("Apex Core", "NovaPay", "John Doe", "Sarah Jenkins").
>
> **Evaluation Scope:** TrustLens has been tested exclusively against this synthetic 10-document test bed. It has **not** been benchmarked against authentic enterprise production data and does **not** provide an absolute or unconditional privacy guarantee.

---

## 3. Project File Structure

```
trustlens/
├── .env.example               # Template for optional xAI environment variables
├── .gitignore                  # Prevents .env and credentials from being committed
├── CREDITS.md                  # Attribution for fonts (OFL), React Bits (MIT), and assets
├── LAUNCH.md                   # Verification checklist of shipped requirements
├── README.md                   # System documentation and reproduction guide
├── index.html                  # HTML entrypoint with self-hosted fonts & security meta
├── package.json                # Project dependencies and script definitions
├── tsconfig.json               # TypeScript compiler configuration
├── vercel.json                 # Vercel deployment headers (CSP, HSTS, X-Frame-Options)
├── vite.config.ts              # Vite bundler configuration
├── vite.singlefile.config.ts   # Optional single-file inlined bundle build config
├── vitest.config.ts            # Vitest unit-test configuration
├── api/
│   └── explain.ts              # Secure serverless function (Vercel Edge / Node)
├── public/
│   ├── .well-known/
│   │   └── security.txt        # Responsible disclosure policy
│   ├── 404.html                # Styled custom 404 error page
│   ├── favicon.png             # Raster icon
│   ├── favicon.svg             # Original vector lens favicon
│   ├── og-image.png            # Open Graph social preview
│   ├── robots.txt              # Crawler permissions
│   ├── site.webmanifest        # PWA / web application manifest
│   ├── sitemap.xml             # XML sitemap
│   └── _headers                # Static headers fallback
├── reports/                    # Verified test and accessibility audit reports
│   └── accessibility-axe-report.json
└── src/
    ├── App.tsx                 # Root application shell & modal orchestrator
    ├── main.tsx                # React DOM mount point
    ├── api/
    │   └── explainClient.ts    # Client caller & strict output grounding validator
    ├── components/
    │   ├── AnswerCarousel.tsx  # 3D coverflow carousel of the 5 answer states
    │   ├── CommandPalette.tsx  # Ctrl/Cmd+K keyboard command navigation
    │   ├── ConflictDiffView.tsx# Visual diff viewer for contradictory policies
    │   ├── CorpusNodeCluster.tsx# Original 3D document node cluster SVG
    │   ├── DocumentPack.tsx    # 10 synthetic documents with vector legend
    │   ├── EvidenceLog.tsx     # In-memory audit log with CSV/JSON export
    │   ├── Footer.tsx          # Compliance footer with copyright & links
    │   ├── GrokExplainModal.tsx# DPDP-compliant consent dialog for xAI
    │   ├── GuidedTour.tsx      # 60-second interactive guided walkthrough
    │   ├── Hero.tsx            # Hero section with headline and 3-column layout
    │   ├── HeroLensArt.tsx     # Original precision optical lens SVG art
    │   ├── HowItWorks.tsx      # Interactive SVG pipeline architecture diagram
    │   ├── Icons.tsx           # Original inline SVG icons (zero emoji)
    │   ├── LegalPages.tsx      # DPDP Act 2023 Privacy Notice, Terms & Cookie Audit
    │   ├── Limitations.tsx     # 5 real failure modes with live reproduction
    │   ├── LiveAssistant.tsx   # Authoritative verbatim console & threshold slider
    │   ├── PrivacyTests.tsx    # Planted vector attack harness & coverage matrix
    │   ├── ReadmeSection.tsx   # In-app architecture & setup documentation
    │   ├── RightRail.tsx       # Desktop tool rail with theme toggle and exports
    │   ├── TestSuite.tsx       # 20 benchmark tests with comparative scoreboard
    │   ├── TopBar.tsx          # Boxed header & floating theme-aware glass pill
    │   └── ReactBits/
    │       ├── DecryptedText.tsx# Verbatim text decryption reveal
    │       └── SplitText.tsx   # Headline character mask animation
    ├── engine/
    │   ├── baselineEngine.ts   # Naive baseline retriever (no gating, no redaction)
    │   ├── benchmark.ts        # Live 20-test evaluation scoreboard runner
    │   ├── bm25.ts             # Plain-code Okapi BM25 ranking algorithm
    │   ├── corpus.ts           # 10 synthetic documents and chunk manifest
    │   ├── privacy.ts          # Regex DLP scanners and prompt-injection detector
    │   ├── testCases.ts        # 20 benchmark test vectors with ground truth
    │   ├── trustEngine.ts      # Core uncertainty gating & conflict analyzer
    │   └── types.ts            # TypeScript interfaces and data models
    ├── styles/
    │   └── index.css           # Design tokens, hairlines, and print stylesheet
    ├── test/
    │   ├── accessibility.test.ts      # axe-core WCAG AA compliance test
    │   ├── benchmark-suite.test.ts    # 20/20 benchmark passing test
    │   ├── chunking-bm25.test.ts      # BM25 tokenization and scoring tests
    │   ├── conflict-detection.test.ts # Contradictory policy resolution tests
    │   ├── explain-guard.test.ts      # Server guards & output grounding tests
    │   ├── privacy-redaction.test.ts  # DLP regex masking and blocking tests
    │   └── setup.ts                   # Vitest DOM environment setup
    └── utils/
        ├── corpusHash.ts       # Deterministic SHA-256 corpus hash utility
        ├── export.ts           # Client-side CSV/JSON serialization
        ├── motion.ts           # GSAP, ScrollTrigger & Lenis coordination
        └── strings.ts          # Centralized dictionary for internationalization
```

---

## 4. Setup & Local Development

### Prerequisites
- Node.js version 18.0 or higher
- npm version 9.0 or higher

### Installation
```bash
# Clone the repository
git clone https://github.com/levin/trustlens.git
cd trustlens

# Install dependencies (strictly local, zero external CDN scripts)
npm install
```

### Development Server
```bash
npm run dev
```
Open `http://localhost:5173` to access the application.

### Running Automated Unit Tests
```bash
npm test
```
Executes all 20 Vitest unit test suites covering BM25 ranking, conflict resolution, DLP privacy redaction, serverless request guards, output grounding validation, and axe-core accessibility.

### Production Static Build
```bash
npm run build
```
Generates an optimized static distribution in `dist/`.

### Single-File Portable Build
```bash
npm run build:singlefile
```
Bundles the entire web application (HTML, CSS, JS, and inlined font assets) into a standalone portable HTML file in `dist-singlefile/index.html`.

---

## 5. How to Reproduce Benchmark Tests

1. Start the development server (`npm run dev`) or open the production deployment.
2. In the top navigation bar, click **Tests** or press `Ctrl+K` and select **"View 20 Automated Tests"**.
3. Click the orange button labelled **"RUN ALL 20 TESTS LIVE"**.
4. The browser executes all 20 benchmark queries through both the deterministic **TrustLens** engine and the naive **Baseline** engine.
5. The live scoreboard will display:
   - **Grounded-Answer Rate**: TrustLens achieves 100% (6/6) vs. Baseline 66.7% (4/6).
   - **Correct Abstention Rate**: TrustLens achieves 100% (5/5) vs. Baseline 0% (0/5; always hallucinates answers).
   - **Citation Precision**: TrustLens achieves 100% (10/10) vs. Baseline 0% (0/10; zero citations).
   - **Confidential Vectors Leaked**: TrustLens leaks 0 secrets (0/4) vs. Baseline 3 leaks (3/4).
   - **Conflict-Detection Rate**: TrustLens achieves 100% (3/3) vs. Baseline 0% (0/3; silently picks one).

---

## 6. Optional Grok "Explain" Layer & Vercel Configuration

The Grok layer is **disabled by default** and operates strictly on an opt-in basis. It rephrases already-retrieved, already-redacted passages in plain language. It **never** performs document retrieval, **never** decides confidence, and **never** replaces the authoritative verbatim source reply.

### Client-Side Privacy & DPDP Consent
- Before the first off-device call, the user is presented with an itemized consent modal detailing the exact data sent (the query and up to 3 redacted passages), the recipient (`xAI Corp`), and cross-border transfer notice.
- Consent requires an unticked checkbox and explicit user submission.
- Consent is kept in volatile browser memory only and expires when the session ends.

### Configuring Vercel Environment Variables
The xAI API secret key must **NEVER** be committed to Git or embedded in client bundles. To enable the optional explain layer on Vercel:

1. Log into your [Vercel Dashboard](https://vercel.com).
2. Select your `trustlens` project (domain: `power.vercel.app`).
3. Navigate to **Settings** > **Environment Variables**.
4. Add the following variables:
   - `XAI_API_KEY`: Your private xAI API token (e.g., `xai-xxxxxxxxxxxx`).
   - `XAI_MODEL`: Desired model identifier (e.g., `grok-beta` or `grok-2`).
   - `EXPLAIN_ENABLED`: Set to `true` (or `false` to activate the immediate kill switch).
5. Redeploy your project.

The serverless proxy at `api/explain.ts` runs on Vercel's Edge runtime, enforces an 8 KB payload cap, limits queries to 500 characters and 3 passages, implements a fixed-window rate limit (10 requests/min/IP), and communicates with `https://api.x.ai/v1/chat/completions`.

---

## 7. Security Model & Explicit Limitations

### Security Protections
- **Strict Content-Security-Policy (CSP)**: `connect-src 'self'`, `frame-ancestors 'none'`, zero third-party script tags.
- **Header Hardening**: `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy: camera=(), microphone=(), geolocation=(), payment=()`, and HSTS preloading.
- **Zero InnerHTML**: All user input, document passages, and model responses are rendered strictly via React DOM text nodes, eliminating DOM-based Cross-Site Scripting (XSS).
- **Volatile Storage**: Zero session data, evidence logs, or chat queries are written to `localStorage` or `cookies`.

> **SECURITY DISCLAIMER:** Implementing these defensive headers, automated redaction rules, and serverless request guards significantly **reduces attack surface and operational risk**, but does **NOT** make the application unconditionally unhackable. Deterministic regex filters can be evaded by phonetic spelling or novel obfuscation techniques, and human review remains essential for safety-critical deployments.

---

## 8. Honest Failure Modes

Per our commitment to transparency, TrustLens highlights five genuine failure modes where deterministic rules fall short (with interactive live reproduction buttons in the **Limits** section):

1. **Lexical Gap / Synonym Paraphrase**: Queries with zero n-gram term overlap (e.g. "newborn infant" vs. "primary parental leave") trigger explicit abstention.
2. **Phonetic Obfuscation**: Sensitive fields spelled phonetically ("oh-oh-oh dash one-two") bypass syntactic regex masks.
3. **Cross-Document Arithmetic**: The system cannot perform mathematical reasoning across documents (e.g. summing vacation days across two policies).
4. **Multi-Hop Traversal**: Relational dependencies spanning disconnected passages cannot be resolved without a knowledge graph.
5. **Generative Extrapolation**: The external LLM layer may occasionally extrapolate beyond provided passages; our client-side grounding validator catches and discards these responses.

---

## 9. Future Roadmap

- **Dense-Sparse Hybrid Retrieval**: Integrating client-side ONNX / WebAssembly micro-embeddings (SPLADE or MiniLM) to bridge lexical gaps while preserving offline determinism.
- **Relational Entity Knowledge Graph**: Linking entities across document chunks to answer multi-hop organizational queries.
- **Local Named Entity Recognition (NER)**: Deploying a lightweight client-side transformer to detect phonetic and obfuscated personal identifiers.

---

## 10. License & Credits

- All code is licensed under the MIT License.
- Typography: Silkscreen (OFL), Space Mono (OFL), Inter (OFL).
- Components: React Bits (`DecryptedText`, `SplitText`) under MIT License.
- Complete licensing documentation is available in [CREDITS.md](CREDITS.md).
