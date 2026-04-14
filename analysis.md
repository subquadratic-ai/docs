# SubQ Documentation: Competitive Analysis & Quality Report

## Methodology

### Research Process

This analysis was conducted through three parallel research tracks:

1. **Internal Audit (SubQ Docs)**
   - Read all 47 MDX content files and 16 `meta.json` navigation files in `content/docs/`
   - Evaluated each page for: topic coverage, code example completeness (languages, runnable snippets), placeholder/stub content, explanation quality, cross-linking, and consistency
   - Cataloged all features documented, all features referenced but not documented, and all pages with "Coming soon" stubs

2. **Competitor Research (Deepgram)**
   - Fetched and analyzed 20+ pages from `developers.deepgram.com`, covering: getting started, all STT feature pages (smart-format, diarization, language-detection, redaction, callback, multichannel, paragraphs, find-and-replace, filler-words, keywords, utterances, endpointing, interim-results, numerals, punctuation), API reference (overview, listen-file endpoint), and models/languages overview
   - Cataloged every documented feature, SDK coverage, developer tooling, and documentation patterns

3. **Competitor Research (ElevenLabs)**
   - Fetched and analyzed 10+ pages from `elevenlabs.io/docs`, covering: models overview, STT overview, transcription guides (batch, live, entity detection, keyterm prompting, speaker diarization, language detection), API reference, and quickstart
   - Cataloged features, pricing model, SDK coverage, and unique capabilities

### Evaluation Criteria

Each documentation set was evaluated against:

| Criterion | What We Looked For |
|---|---|
| **Completeness** | Are all product features documented? Are there stubs or gaps? |
| **Code Examples** | How many languages? Are examples runnable? Do they cover both SDK and raw HTTP? |
| **API Reference** | Full endpoint specs? Request/response schemas? Parameter tables? Error codes? |
| **Developer Experience** | Playground? Interactive docs? CLI? "Try it" buttons? |
| **Consistency** | Same patterns across pages? Matching terminology? Consistent auth approaches? |
| **Discoverability** | Cross-linking? Navigation structure? Decision guides? |
| **Production Readiness** | Rate limits? Error handling? Best practices? Troubleshooting? |

---

## SubQ Documentation Audit

### Content Inventory

**Total files:** 47 MDX files, 16 meta.json files across 8 top-level sections

| Section | Files | Status |
|---|---|---|
| Root (index, overview) | 2 | ✅ Complete |
| Getting Started | 4 | ✅ Complete |
| Concepts | 14 | ✅ Complete |
| Integration | 13 | ✅ Complete |
| API Reference | 6 | ❌ All placeholder ("Coming soon") |
| Guides | 4 | ❌ All placeholder ("Coming soon") |
| Reference | 2 | ❌ All placeholder ("Coming soon") |
| Community & Support | 2 | ❌ All placeholder ("Coming soon") |

**Overall:** 35 pages (74%) have real content. 14 pages (26%) are placeholders.

---

### Section-by-Section Analysis

#### Root Files

**`content/docs/index.mdx`** — Landing page with `<HeroBanner />` component and card grid linking to Quickstart, Overview, integration pages, and concept pages. Well-structured with "Build" and "Concepts" groupings. No issues.

**`content/docs/overview.mdx`** — Introduces SubQ as a Deepgram-compatible STT service. Covers pre-recorded + streaming modes and mobile SDKs. **Thin** (~40 lines). Missing: supported languages list, pricing/plans mention, architecture diagram, and broader "Next steps" links.

---

#### Getting Started (4 files)

**`get-api-key.mdx`** — Clear step-by-step guide with screenshot reference. Good security callout about never exposing keys. No issues.

**`choose-integration.mdx`** — Excellent decision guide covering Direct HTTP, Deepgram SDKs, Browser, and Mobile paths. Includes comparison table and decision flowchart. One of the strongest pages in the docs.

**`quickstart/pre-recorded.mdx`** — Complete with tabbed code examples (cURL, Python, Node.js). Shows response structure with field descriptions. Well-written. Minor note: `timestamps: true` passed as a header (unconventional — worth explaining why).

**`quickstart/streaming.mdx`** — Complete with tabbed examples (Python, Node.js, Browser HTML). Shows WebSocket auth, message structure, key fields table. Good security callout for browser usage. Issues:
- Python example uses `Authorization` header, but Node.js example uses `Sec-WebSocket-Protocol` subprotocol — inconsistent auth approaches
- Word format (`[word, start_ms, end_ms]` array) differs from pre-recorded format (`{word, start, end}` object) — documented but never explicitly compared

---

#### Concepts (14 files across 5 subcategories)

**Transcription Modes:**
- `index.mdx` (214 lines) — Comprehensive. Covers pre-recorded vs streaming, code examples in cURL/Python/Node.js, alternative endpoints, supported audio formats. Issues: Node.js example has unused `createClient` import; typo "To transcript a file" → "To transcribe a file"
- `async-callbacks.mdx` — Solid webhook pattern explanation. Covers `callback`, `callback_method`, `metadata` parameters. Missing: Python/Node.js code examples (cURL only); no webhook payload response example

**Speech Intelligence:**
- `language-detection.mdx` — Documents `en`/`es` support, `detect_language=true`, `language=auto`. cURL examples only.
- `speaker-diarization.mdx` — Good response example. cURL only. Missing: streaming support clarity, max speaker count, accuracy expectations
- `keyword-boosting.mdx` — Explains `keywords` query parameter. Missing: response example showing effect, keyword limits, SDK code examples

**Transcript Formatting:**
- `smart-formatting.mdx` — One of the strongest pages. Excellent before/after examples covering `smart_format`, `numerals`, `measurements`. Typos: "it's formatted version" → "its"; spurious `+` in "transcription+"
- `paragraphs.mdx` — Good explanation of pause-based/topic-based detection. cURL only. Missing: response example
- `filler-words.mdx` — Excellent when-to-include/exclude analysis with before/after table. Notes English-only limitation

**Streaming Controls:**
- `interim-results.mdx` — Clear "watching someone type in chat" analogy. Documents `is_final`, `speech_final` fields
- `vad-events.mdx` — Well-structured. Covers `SpeechStarted` message format with comparison table. Minor: unclosed italic marker on line 17
- `utterance-detection.mdx` — Documents `utterance_end_ms` parameter and `UtteranceEnd` message. Good comparison table vs endpointing
- `endpointing.mdx` — Clear with practical value ranges table. Documents `endpointing=false` for manual control
- `websocket-protocol.mdx` — Most comprehensive streaming reference. Documents both endpoints, client/server messages, all query parameters. Effectively serves as the streaming API reference

**Privacy & Compliance:**
- `pii-redaction.mdx` — Documents all `redact` values and `redact_mode` options with examples. Missing: REST examples (WebSocket URL only), response example
- `compliance.mdx` — Covers HIPAA/BAA, data handling, TLS, self-hosting. Good enterprise CTA. Minor naming inconsistency: meta.json says "Data protection" vs content says "Privacy & Compliance"

---

#### Integration (13 files)

**`overview.mdx`** — Good decision map with endpoints/protocols table. Lists supported platforms.

**`migrate-from-deepgram.mdx`** (322 lines) — Excellent. Before/after code for Python/Node.js/Go. Verification examples. Documents WebSocket auth issue with Python/Go SDKs. Troubleshooting table. One of the best pages in the entire documentation.

**`setup.mdx`** (496 lines) — Very thorough. 4 language tabs (Python, Node.js, Go, Rust). Each includes dependencies, env vars (Linux/macOS/Windows), and verification script.

**`transcribe-file.mdx`** (317 lines) — Complete with 4 language tabs. Prerequisites, full code, "How it works" breakdown, run instructions.

**`transcribe-url.mdx`** — Same quality. 4 language tabs. **Inconsistency:** Downloads audio client-side then POSTs bytes, while quickstart/concepts show `{"url": "..."}` JSON body. Two different URL transcription methods documented without acknowledging each other.

**`streaming.mdx`** (655 lines) — Largest file. 4 language tabs with file-streaming and live-source examples.

**Browser (3 files):**
- `browser/setup.mdx` — CDN-based Deepgram SDK setup, HTTPS cert generation, API key auth pattern
- `browser/live-recording.mdx` — Full working HTML source with MediaRecorder + WebSocket flow
- `browser/pre-recorded-upload.mdx` — Simple and clear HTML source

**Mobile (4 files):**

- `mobile/setup.mdx` (575 lines) — Full SDK client source code for both iOS (Swift) and Android (Kotlin). iOS uses native `URLSession` + `URLSessionWebSocketTask` with zero external dependencies. Android uses OkHttp as the sole dependency. Each platform includes a complete ~220-line client class (`SubQSTTClient` + `StreamSession`) with inline doc comments and a verify-your-setup snippet. Excellent quality.

- `mobile/pre-recorded.mdx` (~180 lines) — Covers URL transcription and file/data transcription for both platforms. Error handling section with HTTP status code matching (401, 413). "How it works" breakdown (4 steps). Shows how to add query parameters (`smart_format=true`, `language=es`). Good quality.

- `mobile/streaming.mdx` (561 lines) — Full microphone capture + WebSocket streaming for both platforms. iOS: `AVAudioEngine` with format conversion to 16-bit linear PCM at 16 kHz. Android: `AudioRecord` with coroutine-based capture loop on `Dispatchers.IO`. Both include complete `startRecording()` and `stopRecording()` functions, control messages, and error handling. The most detailed streaming guide in the entire docs. Minor gap: Android `StreamSession` doesn't expose `keepAlive()` or `finalizeStream()` convenience methods — iOS has these built in.

- `mobile/sdk-reference.mdx` (~200 lines) — Complete class/method reference for both platforms. Constructor, method signatures, parameter tables, return types, throw/error behavior. Authentication section explaining REST (`Authorization: Token`) vs WebSocket (`Sec-WebSocket-Protocol: token, <key>`). Audio format defaults table. Good formal reference.

---

#### API Reference (6 files) — ALL PLACEHOLDER

Every file contains only `## Coming soon`:
- `api-overview.mdx`
- `authentication.mdx`
- `pre-recorded.mdx`
- `streaming.mdx`
- `parameters.mdx`
- `error-codes.mdx`

**This is the single largest content gap.** Both competitors have full endpoint specifications, request/response schemas, interactive testing, and comprehensive parameter documentation.

---

#### Guides (4 files) — ALL PLACEHOLDER

- `best-practices.mdx` — "Coming soon"
- `audio-quality-tips.mdx` — "Coming soon"
- `latency-optimization.mdx` — "Coming soon"
- `troubleshooting.mdx` — "Coming soon"

---

#### Reference (2 files) — ALL PLACEHOLDER

- `rate-limits.mdx` — "Coming soon"
- `security.mdx` — "Coming soon"

---

#### Community & Support (2 files) — ALL PLACEHOLDER

- `community.mdx` — "Coming soon"
- `support.mdx` — "Coming soon"

---

### Inconsistencies & Bugs

| # | Issue | Location | Severity |
|---|---|---|---|
| 1 | **URL transcription: two different approaches** — Quickstart/concepts use `{"url": "..."}` JSON body; integration guide downloads client-side and POSTs bytes | `quickstart/pre-recorded.mdx` vs `integration/transcribe-url.mdx` | High |
| 2 | **Python streaming auth inconsistency** — Quickstart uses `Authorization` header; integration/protocol docs use `Sec-WebSocket-Protocol` | `quickstart/streaming.mdx` vs `integration/streaming.mdx` | High |
| 3 | **Word timestamp format difference** — Pre-recorded: `{word, start, end}` (seconds, object). Streaming: `[word, start_ms, end_ms]` (milliseconds, array). Never compared in one place | Across quickstart and concept pages | Medium |
| 4 | **Node.js SDK `key` vs `accessToken`** — Migration page notes version-dependent property name, not addressed elsewhere | `migrate-from-deepgram.mdx` | Medium |
| 5 | **Android `StreamSession` missing control methods** — iOS exposes `keepAlive()`, `finalizeStream()`, `requestClose()`. Android only has `close()`. | `integration/mobile/streaming.mdx` | Medium |
| 6 | **Privacy section naming** — `meta.json` title: "Data protection"; content language: "Privacy & Compliance" | `concepts/privacy-compliance/meta.json` | Low |
| 7 | **Typo: "To transcript a file"** → "To transcribe a file" | `concepts/transcription-modes/index.mdx` line 28 | Low |
| 8 | **Typo: "it's formatted version"** → "its formatted version" | `concepts/transcript-formatting/smart-formatting.mdx` line 22 | Low |
| 9 | **Spurious `+` in "transcription+"** | `concepts/transcript-formatting/smart-formatting.mdx` line 79 | Low |
| 10 | **Unused `createClient` import** in Node.js example | `concepts/transcription-modes/index.mdx` line 75 | Low |
| 11 | **Unclosed italic marker** on line 17 | `concepts/streaming-controls/vad-events.mdx` | Low |

---

## Competitor Analysis: Deepgram

### Overview

Deepgram is SubQ's primary competitor and the API that SubQ is compatible with. Their documentation at `developers.deepgram.com` is mature, extensive, and well-structured.

### Feature Coverage

Deepgram documents **30+ individual STT features**, each with its own dedicated page:

| Category | Features |
|---|---|
| **Core Transcription** | Pre-recorded (batch), Live streaming (WebSocket), Turn-based (Flux model) |
| **Formatting** | Smart Formatting, Punctuation, Paragraphs, Numerals, Filler Words, Dictation, Measurements |
| **Speech Intelligence** | Speaker Diarization, Language Detection, Multilingual Codeswitching, Keyword Boosting (Nova-2), Keyterm Prompting (Nova-3), Find and Replace, Search |
| **Privacy** | Redaction (PCI, PII, PHI, numbers, 50+ entity types), Profanity Filter |
| **Streaming Controls** | Endpointing, Interim Results, Utterance End, VAD Events, KeepAlive, Speech Started |
| **Audio/Channel** | Multichannel (up to 20 channels), Encoding, Sample Rate, Audio Format Detection |
| **Results Processing** | Utterances, Utterance Split, Callback (async webhooks — HTTP + WebSocket), Tagging, Confidence scores |
| **Audio Intelligence** | Summarization, Sentiment Analysis, Topic Detection, Intent Detection, Entity Detection |
| **Model Management** | MIP Opt-out, Custom models, Extra metadata |

### SDK Coverage

4 official SDKs with tabbed code examples on every page:

| Language | Package |
|---|---|
| Python | `deepgram-sdk` |
| JavaScript/TypeScript | `@deepgram/sdk` |
| Go | `deepgram-go-sdk` |
| C# / .NET | `deepgram-dotnet-sdk` |

No mobile-specific SDKs (iOS/Android).

### API Reference Quality

- Full endpoint specifications for `POST /v1/listen` (pre-recorded and streaming)
- **~40+ query parameters** documented with types, defaults, enum values, and required/optional flags
- Full request/response JSON schemas with expandable properties
- Interactive "Try it" buttons linking to the API Playground
- Auto-generated code snippets in Python, cURL, JavaScript, Go, C#
- Error codes documented per endpoint
- Management APIs: Projects, Members, Invitations, Scopes, Billing, Usage, API Keys, Models, Requests

### Developer Tooling

| Tool | Details |
|---|---|
| **API Playground** | Interactive web tool at `playground.deepgram.com` — linked from almost every feature page |
| **CLI** | Deepgram CLI with built-in MCP server (28 API commands) — prominently marketed |
| **MCP Server** | Built into CLI for AI agent integration |
| **Slack Bot** | AI-powered support bot for Slack workspaces |
| **Console** | Web dashboard for API key management, usage, billing |
| **Code Samples Repo** | GitHub repository with non-SDK examples |

### Documentation Patterns (What They Do Well)

1. **Consistent page structure**: Every feature page follows: description → parameter syntax → Enable Feature (cURL) → Analyze Response (annotated JSON) → before/after comparison
2. **Before/after tables**: Side-by-side "without feature" vs "with feature" on Smart Format, Numerals, Filler Words, Find and Replace, Keywords
3. **Full JSON response examples** on every feature page with inline field annotations
4. **Playground deep-links** pre-configured with feature parameters
5. **Cross-referencing**: Features heavily link to related features
6. **Operational limits clearly stated**: 2 GB file size, 100 concurrent requests (Nova), 10-min processing cap

### Models

| Model | Use Case |
|---|---|
| **Flux** | Voice-agent-native STT with built-in turn detection; English only |
| **Nova-3** | Flagship ASR; 54% WER reduction claimed; 10+ languages; medical variant |
| **Nova-2** | Legacy; 12+ domain variants (meeting, phonecall, finance, voicemail, video, medical, drivethru, automotive, ATC) |
| **Whisper Cloud** | Managed OpenAI Whisper (tiny→large); limited concurrency (5-15) |

### Self-Hosted Documentation

- Distribution credentials management API documented
- Feature availability differences (e.g., redaction: self-hosted = English-only; hosted = all languages for pre-recorded)
- Enterprise-tier offering

---

## Competitor Analysis: ElevenLabs

### Overview

ElevenLabs is a broader audio AI platform (TTS, STT, music, dubbing, voice cloning). Their STT product (Scribe v2) is newer but feature-rich, documented at `elevenlabs.io/docs`.

### STT Feature Coverage

| Feature | Details |
|---|---|
| **Batch transcription** | File upload (up to 3 GB / 10 hours) or URL (YouTube, TikTok) |
| **Real-time streaming** | WebSocket, ~150ms latency |
| **Speaker diarization** | Up to 32 speakers, configurable threshold (0.1–0.4) |
| **Language detection** | Automatic across 90+ languages |
| **Word-level timestamps** | Default; also character-level timestamps per word |
| **Entity detection** | 56 entity types across PII, PHI, PCI, offensive language |
| **Entity redaction** | `{REDACTED}`, `{ENTITY_TYPE}`, or `{ENTITY_TYPE_N}` modes |
| **Keyterm prompting** | Up to 1,000 context-aware terms (max 50 chars / 5 words each) |
| **Audio event tagging** | `(laughter)`, `(footsteps)`, etc. |
| **Multi-channel** | Up to 5 independent channels |
| **No-verbatim mode** | Single flag to strip all fillers, false starts, non-speech sounds |
| **VAD** | Configurable silence/speech thresholds for streaming |
| **Webhook delivery** | Async results with `webhook_metadata` correlation |
| **Temperature/seed control** | Randomness (0–2) and deterministic output control |
| **Zero retention** | Enterprise-only — no logs or transcripts stored |
| **Source URL transcription** | Direct YouTube/TikTok/hosted video URL input |

### SDK Coverage

| SDK | Status |
|---|---|
| Python | Official `elevenlabs` package |
| TypeScript/Node.js | Official SDK |
| Mobile (iOS/Android) | **Not available** |
| AI Skills | `npx skills add elevenlabs/skills` for AI coding assistants |

### Pricing Model

| Model | Price (Business Tier) | Notes |
|---|---|---|
| Scribe v2 (batch) | $0.22/hour | Entity detection +$0.066/hr, keyterm prompting +$0.044/hr |
| Scribe v2 Realtime | $0.39/hour | No add-on pricing |
| Free tier | 2.5 hours STT | Concurrency: 4 batch / 8 realtime |

Plans: Free → Starter ($5) → Creator ($22) → Pro ($99) → Scale ($330) → Business ($1,320) → Enterprise

### Documentation Quality

**Strengths:**
- Clean structure with models → capabilities → API reference → guides hierarchy
- API reference is excellent with types, defaults, constraints, allowed enum values
- Interactive "Try it" buttons on API reference pages
- Full WebSocket protocol documentation (send/receive schemas, 12+ error types)
- WER benchmarks per language broken into accuracy tiers (Excellent ≤5%, High ≤10%, Good ≤20%, Moderate ≤50%)

**Weaknesses:**
- Guide pages rely heavily on client-side rendering (accessibility/SEO issues)
- SDKs and quickstart are TTS-focused; no dedicated STT quickstart
- Only 2 SDK languages (Python, TypeScript)

### Unique Features Not Found Elsewhere

1. **Audio event tagging** — Automatic non-speech event labeling (`(laughter)`, `(applause)`)
2. **Source URL transcription** — Direct YouTube/TikTok URL input, no download step
3. **Temperature/seed control** — Tunable randomness and request determinism
4. **56 entity types** for detection/redaction — most granular in the market
5. **No-verbatim mode** — Single boolean to strip all fillers and false starts
6. **Full audio platform convergence** — STT alongside TTS, music, dubbing, voice cloning, voice isolation, sound effects

---

## Feature Gap Matrix

Features documented by competitors but **not documented or not available in SubQ**:

| Feature | Deepgram | ElevenLabs | SubQ Status |
|---|---|---|---|
| Find and Replace | ✅ | ❌ | Not documented |
| Summarization | ✅ | ❌ (separate product) | Not documented |
| Sentiment Analysis | ✅ | ❌ | Not documented |
| Topic Detection | ✅ | ❌ | Not documented |
| Intent Detection | ✅ | ❌ | Not documented |
| Entity Detection (granular) | Basic (via redaction) | ✅ 56 types | Not documented |
| Profanity Filter | ✅ | ❌ | Not documented |
| Dictation Mode | ✅ | ❌ | Not documented |
| Multichannel | ✅ up to 20 ch | ✅ up to 5 ch | Mentioned once ("auto-converted to mono") — no feature page |
| Audio Event Tagging | ❌ | ✅ | Not documented |
| Source URL (YouTube/TikTok) | ❌ | ✅ | Not documented |
| Model selection guide | ✅ | ✅ | Not documented |
| WER benchmarks | ✅ | ✅ per-language tiers | Not documented |
| Interactive playground | ✅ | ✅ "Try it" buttons | Not available |
| CLI tool | ✅ with MCP server | ❌ | Not available |
| Domain-specific models | ✅ 12+ variants | ❌ | Not documented |
| Rate limits / operational bounds | ✅ (2GB, 100 concurrent, 10-min) | ✅ (3GB, tiered concurrency) | Not documented |
| Error code catalog | ✅ | ✅ | Not documented |
| Self-hosted docs | ✅ | ❌ | Mentioned in compliance page but no dedicated docs |
| Mobile SDKs (iOS/Android) | ❌ | ❌ | ✅ **Fully documented** |

---

## Documentation Pattern Comparison

| Pattern | Deepgram | ElevenLabs | SubQ |
|---|---|---|---|
| Tabbed multi-language code on **every feature page** | ✅ 4 languages | ✅ 2 languages | ⚠️ Integration + mobile pages: yes. Concept pages: cURL only |
| Full JSON response examples | ✅ Every page | ✅ Every page | ⚠️ Some pages, but missing on paragraphs, keyword-boosting, PII redaction, async-callbacks |
| Before/after comparison tables | ✅ Extensively | ⚠️ Some pages | ⚠️ Only smart-formatting and filler-words |
| Cross-feature linking | ✅ Heavy | ⚠️ Moderate | ⚠️ Light — some "Next steps" sections |
| Interactive "Try it" / Playground | ✅ Deep-linked | ✅ Inline buttons | ❌ Not available |
| Consistent page structure | ✅ Same template everywhere | ✅ Consistent | ⚠️ Integration + mobile are consistent; concept pages vary |
| Runnable code examples | ✅ | ✅ | ✅ Integration + mobile pages are copy-pasteable |
| Decision/selection guides | ⚠️ Not prominent | ⚠️ Model comparison matrix | ✅ `choose-integration.mdx` is excellent |
| Mobile-native documentation | ❌ | ❌ | ✅ Full iOS + Android guides with native audio capture |

---

## End-User Scenarios Not Covered

These are common questions/tasks end users look for that SubQ docs don't currently answer:

| # | Scenario | What Users Ask | Current Status |
|---|---|---|---|
| 1 | Error handling & retries | "What does error code 429 mean? How do I implement retries?" | No error code catalog, no retry guidance |
| 2 | Model selection | "Which model should I use for my use case? What's the accuracy?" | No model page, no WER benchmarks |
| 3 | Multichannel audio | "How do I transcribe a phone call with separate agent/customer channels?" | Not documented (mentioned as "auto-converted to mono") |
| 4 | Language support | "What languages are supported? How accurate is Spanish?" | Mentioned in passing (`en`/`es`) but no dedicated page |
| 5 | Production readiness | "What are rate limits? How do I optimize latency? Best practices?" | All placeholder pages |
| 6 | Interactive testing | "Can I test the API without writing code?" | No playground or interactive tool |
| 7 | Debugging | "My transcription is blank / WebSocket won't connect / audio not recognized" | Troubleshooting page is placeholder |
| 8 | Webhook payload | "What does the async callback response look like?" | Callback setup documented but no payload example |
| 9 | Operational limits | "What's the max file size? How many concurrent connections can I have?" | Completely undocumented |
| 10 | Mobile reconnection | "What happens when the user loses connectivity mid-stream?" | Not covered in mobile streaming docs |

---

## SubQ Strengths

These are areas where SubQ's documentation is **equal to or better than** both competitors:

### 1. First-Party Mobile SDKs (unique — no competitor has this)

**Files:** `integration/mobile/setup.mdx`, `pre-recorded.mdx`, `streaming.mdx`, `sdk-reference.mdx`

Neither Deepgram nor ElevenLabs offers first-party iOS or Android SDKs. SubQ has **complete, production-ready documentation** for both platforms across 4 pages (~1,500+ lines total):

- **iOS (Swift):** Zero external dependencies — uses native `URLSession` + `URLSessionWebSocketTask`. Full `SubQSTTClient` + `StreamSession` classes. Microphone capture via `AVAudioEngine` with format conversion to 16-bit linear PCM at 16 kHz. Control messages: `keepAlive()`, `finalizeStream()`, `requestClose()`, `close()`.
- **Android (Kotlin):** Single dependency (OkHttp 4.12). Full `SubQSTTClient` + `StreamSession` classes. Microphone capture via `AudioRecord` with coroutine-based IO loop. Runtime permission handling via `ActivityResultContracts`.
- Both platforms include complete `startRecording()` / `stopRecording()` functions, error handling with HTTP status code matching, and a formal SDK class/method reference.

This is a **shipping differentiator**, not a roadmap item.

### 2. Deepgram Migration Guide (unique — no competitor equivalent)

**File:** `integration/migrate-from-deepgram.mdx` (322 lines)

The most comprehensive migration guide in the STT documentation space. Before/after code in 3 languages, WebSocket auth workaround documentation, verification examples, and troubleshooting table. Neither competitor needs one, but this is a powerful acquisition tool for Deepgram users.

### 3. Choose-Your-Integration Decision Guide (better than competitors)

**File:** `getting-started/choose-integration.mdx`

Comparison table + decision flowchart covering Direct HTTP, Deepgram SDKs, Browser, and Mobile paths. Deepgram doesn't have an equivalent decision guide. ElevenLabs has "Choose your path" but it's less detailed.

### 4. Multi-Language Integration Guides (matches or exceeds Deepgram)

**Files:** `setup.mdx`, `transcribe-file.mdx`, `transcribe-url.mdx`, `streaming.mdx`

Python, Node.js, Go, and Rust tabs with OS-specific env setup (Linux/macOS/Windows PowerShell/Windows cmd). Each page includes consistent "How it works" breakdowns. Deepgram covers Python/JS/Go/C#; SubQ covers Python/JS/Go/Rust.

### 5. Deepgram API Compatibility (unique positioning)

"Change 2 lines to migrate" — well-articulated across overview, migration, and choose-integration pages. This is a powerful value proposition that no competitor can replicate.

### 6. WebSocket Protocol Documentation (on par with best)

**File:** `concepts/streaming-controls/websocket-protocol.mdx`

Comprehensive reference covering both endpoints, all client/server messages, and query parameters. Effectively serves as the streaming API reference. Comparable to ElevenLabs' WebSocket protocol docs.

### 7. Smart Formatting Documentation (best-in-class)

**File:** `concepts/transcript-formatting/smart-formatting.mdx`

Before/after examples showing individual feature effects and combined output. Shows `smart_format`, `numerals`, and `measurements` independently and together. More detailed than Deepgram's equivalent page.

### 8. Browser Integration Guides (complete working examples)

**Files:** `browser/setup.mdx`, `browser/live-recording.mdx`, `browser/pre-recorded-upload.mdx`

Full working HTML source code that users can copy and run. Includes HTTPS cert generation and security callouts.

---

## Prioritized Recommendations

### Tier 1 — Critical (Fill Before Public Launch)

| # | Action | Rationale |
|---|---|---|
| 1 | **Write API Reference** (6 pages) | Table stakes. Both competitors have full endpoint specs with schemas, parameters, and error codes. The `websocket-protocol.mdx` page partially covers streaming, but pre-recorded has no formal reference. |
| 2 | **Document rate limits & error codes** | Users cannot build production systems without knowing operational boundaries. Deepgram: 2GB files, 100 concurrent, 10-min cap. ElevenLabs: 3GB, tiered concurrency. SubQ: unknown. |
| 3 | **Create a models & languages page** | Document available models, supported languages (`en`/`es` at minimum), and accuracy expectations. Both competitors have this. |
| 4 | **Add Android `keepAlive()`/`finalizeStream()` to `StreamSession`** | Parity gap with iOS. The Android `StreamSession` only exposes `close()` while iOS has 4 control methods. |

### Tier 2 — Important (Close Competitive Gaps)

| # | Action | Rationale |
|---|---|---|
| 5 | **Add multi-language code tabs to concept pages** | All 14 concept pages show cURL only. Add Python/Node.js tabs to match Deepgram's 4-language coverage. |
| 6 | **Add JSON response examples** to every feature page | Missing on: paragraphs, keyword-boosting, PII redaction, async-callbacks. Both competitors show full responses on every page. |
| 7 | **Write best practices & troubleshooting guides** | Common user need. Cover: audio quality tips, latency optimization, debugging WebSocket issues, common error scenarios. |
| 8 | **Document multichannel audio** | Deepgram supports 20 channels. ElevenLabs supports 5. SubQ says "auto-converted to mono" — document whether multichannel is supported or explicitly state it isn't. |
| 9 | **Add before/after comparison tables** to more features | Currently only on smart-formatting and filler-words. Add to: keyword-boosting, diarization, PII redaction, paragraphs. |
| 10 | **Resolve URL transcription inconsistency** | Document both approaches (JSON body vs client-side download) on the same page, explaining when to use each. |
| 11 | **Fix auth inconsistency** | Standardize Python/Node.js WebSocket auth approach across quickstart, integration, and protocol pages. |

### Tier 3 — Strategic (Competitive Differentiation)

| # | Action | Rationale |
|---|---|---|
| 12 | **Build interactive API playground** | Both competitors have one. Significant DX advantage. |
| 13 | **Publish WER benchmarks** | Both competitors publish accuracy metrics. Builds trust and helps users evaluate SubQ. |
| 14 | **Add a mobile sample app page** (SwiftUI + Jetpack Compose) | The old `sample-app.mdx` was removed. A complete working app accelerates mobile adoption. |
| 15 | **Add mobile reconnection/offline guidance** | Neither platform discusses connectivity resilience — important for real-world mobile usage. |
| 16 | **Document audio intelligence features** (if available) | Summarization, sentiment, topics, intents, entity detection. If SubQ offers any, document them. If not, consider the roadmap. |
| 17 | **Evaluate CLI / MCP server** | Deepgram is heavily marketing their CLI + MCP. Consider whether SubQ should enter this space. |
| 18 | **Add community/support channels** | Fill placeholder pages. Both competitors have Discord, forums, and/or Slack bots. |
| 19 | **Document self-hosted deployment** | Mentioned in compliance page. If available, create dedicated self-hosted docs with feature availability matrix. |

---
