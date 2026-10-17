<p align="center">
  <img src="https://raw.githubusercontent.com/GlycemicGPT/.github/main/assets/logo.png" alt="GlycemicGPT" width="160" height="160"/>
</p>

<h1 align="center">GlycemicGPT</h1>

<p align="center">
  <strong>Open source AI-powered diabetes management platform.</strong><br/>
  <em>Because no one should manage diabetes alone.</em>
</p>

<p align="center">
  <a href="https://glycemicgpt.org">Website</a> &middot;
  <a href="https://github.com/GlycemicGPT/GlycemicGPT">Platform</a> &middot;
  <a href="https://github.com/GlycemicGPT/GlycemicGPT/blob/develop/CONTRIBUTING.md">Contribute</a> &middot;
  <a href="https://github.com/GlycemicGPT/GlycemicGPT/discussions">Discussions</a> &middot;
  <a href="https://github.com/lumose-health/.github/blob/main/SECURITY.md">Security</a> &middot;
  <a href="https://discord.gg/TJKzjsts">Discord</a>
</p>

---

> **IMPORTANT SAFETY WARNING**
>
> This software is **NOT** designed to replace your endocrinologist or healthcare provider. GlycemicGPT provides AI-generated suggestions only and should be used as a supplementary tool alongside professional medical care.

---

> **ALPHA SOFTWARE** -- This project is under active development. It is functional and in daily use by the developer, but has not been broadly tested. Use at your own risk and always consult your healthcare provider.

---

## What is GlycemicGPT?

GlycemicGPT is a complete diabetes management platform that connects your CGM and insulin pump directly to an AI-powered analysis engine. It bridges the gap between raw device data and the kind of pattern analysis you'd get from an endocrinologist -- except it's always on, always watching, and runs entirely on your own infrastructure.

Think of it as what Nightscout, Loop, and an endocrinologist's pattern analysis would look like if they were designed together from scratch with modern AI. Self-hosted, privacy-first, no subscriptions, no vendor lock-in. Your data stays yours.

The platform is built around three pillars:

1. **Data Aggregation & Monitoring** -- real-time glucose and insulin data from CGMs and pumps via BLE and cloud APIs, displayed through a web dashboard, Android app, and Wear OS watch face.

2. **AI-Powered Analysis** -- daily briefs, meal analysis, pattern recognition, and a conversational AI chat backed by RAG-powered clinical diabetes knowledge. Bring your own AI provider (Claude, OpenAI, Ollama, or any OpenAI-compatible endpoint).

3. **Alerting & Caregiver Ecosystem** -- configurable glucose thresholds with predictive alerts, tiered delivery (in-app, push, Telegram), caregiver escalation, and multi-patient caregiver dashboards.

## Key Features

**Data & Monitoring**
- Real-time glucose monitoring with trend charts, Time in Range, and ambulatory glucose profile
- BLE connectivity to Tandem insulin pumps (basal rates, bolus history, insulin on board, reservoir, battery)
- Dexcom G7 CGM integration via cloud API
- CGM summary statistics, insulin summary, and bolus review
- Historical data with configurable retention periods

**AI Analysis**
- AI-powered daily briefs analyzing overnight patterns, meal responses, and trends
- Multi-turn AI chat with conversation memory -- ask questions about your data like you would an endocrinologist
- RAG-powered research pipeline with clinical diabetes knowledge base
- Knowledge base viewer with document management
- BYOAI: bring your own AI provider -- no forced subscriptions or data lock-in

**Alerting & Caregivers**
- Configurable glucose thresholds with predictive alerts
- Tiered delivery: in-app, push notification, Telegram, caregiver escalation
- Caregiver dashboard with read-only access and multi-patient support
- Emergency contact configuration with automatic escalation

**Mobile & Wearables**
- Android phone app (Kotlin, Jetpack Compose)
- Wear OS companion with watch face complications (glucose, IoB, trend)
- Watch-to-phone AI chat and alert dismiss from wrist
- Text-to-speech for AI responses
- Self-update from GitHub Releases (sideloaded builds)

**Platform & Extensibility**
- Self-hosted Docker stack with web dashboard and REST API
- Capability-based plugin architecture for device support
- Tandem t:slim X2 and Mobi plugin built-in
- SDK for community-developed pump and CGM drivers
- Safety layer enforced at the platform level, not the plugin level
- Monitoring-only in all pre-built releases (no pump control)

## Supported Devices

| Device | Type | Connection | Status |
|--------|------|------------|--------|
| Dexcom G7 | CGM | Cloud API | Verified |
| Tandem t:slim X2 | Insulin Pump | BLE (direct) + Cloud API | Verified |
| Tandem Mobi | Insulin Pump | BLE (direct) + Cloud API | Protocol-compatible (untested on hardware) |

> **Tandem Mobi note:** The Mobi uses the same BLE protocol, authentication, and data formats as the t:slim X2. Our Tandem plugin is protocol-compatible with both models, but **Mobi support has not been verified against physical hardware**. Protocol compatibility does not guarantee correct operation on untested devices. Use with Mobi hardware is entirely at your own risk. See the full [Medical Disclaimer](#medical-disclaimer) below.

Support for additional pumps and CGMs is planned. The plugin architecture is designed for community contributions -- if you use a device we don't support yet, you can help add it.

## Architecture

| Component | Technology |
|-----------|------------|
| Backend API | FastAPI, Python 3.12, PostgreSQL 16, Redis 7 |
| Web Dashboard | Next.js 15, React 19, Tailwind CSS, shadcn/ui |
| AI Sidecar | TypeScript, Express, multi-provider proxy |
| Android App | Kotlin, Jetpack Compose, BLE |
| Wear OS | Kotlin, Wear Compose, Watch Face Push API |
| Plugin SDK | Kotlin interfaces, capability-based, sandboxed |

## Repositories

| Repo | Description | Status |
|------|-------------|--------|
| [**GlycemicGPT**](https://github.com/GlycemicGPT/GlycemicGPT) | Core platform: API, web dashboard, AI sidecar, mobile app, Wear OS, plugins | Active |

*Coming soon: `android-unofficial` (sideloaded builds), `android-playstore` (Play Store), `ios-unofficial`, `ios-appstore`, and `website` (glycemicgpt.org).*

## Getting Started

```bash
git clone https://github.com/GlycemicGPT/GlycemicGPT.git
cd GlycemicGPT && cp .env.example .env
docker compose up --build -d
```

See the [main repository README](https://github.com/GlycemicGPT/GlycemicGPT#quick-start) for full setup instructions, configuration, and development guide.

## Key Principles

- **Suggestions only** -- GlycemicGPT does not control medical devices
- **Self-hosted** -- your data stays on your infrastructure
- **BYOAI** -- bring your own AI provider, no forced subscriptions
- **Safety-first** -- pre-validation layer, emergency escalation, medical disclaimers baked into the UI
- **Open source** -- GPL-3.0, always free, community-driven

## Get Involved

We welcome contributions from developers, designers, and people living with diabetes.

- Read the [Contributing Guide](https://github.com/GlycemicGPT/GlycemicGPT/blob/develop/CONTRIBUTING.md) for the main platform
- Check out [Good First Issues](https://github.com/GlycemicGPT/GlycemicGPT/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
- Join the [Discussions](https://github.com/GlycemicGPT/GlycemicGPT/discussions)
- Read our [Governance](https://github.com/GlycemicGPT/GlycemicGPT/blob/develop/GOVERNANCE.md) to understand project roles

### Project Roles

| Role | Description |
|------|-------------|
| **Contributor** | Anyone who opens issues, submits PRs, or joins discussions |
| **Committer** | Trusted contributors with write access to specific components |
| **Maintainer** | Project stewards responsible for releases, security, and direction |

See [GOVERNANCE.md](https://github.com/GlycemicGPT/GlycemicGPT/blob/develop/GOVERNANCE.md) for the path to growing your involvement.

---

<!-- SYNC: The Medical Disclaimer section below must match the canonical MEDICAL-DISCLAIMER.md in the main GlycemicGPT repository. In case of any discrepancy, the main repository version governs. Last synced: 2026-03-30 -->

## Medical Disclaimer

### Regulatory Status

This software has **not** been cleared, approved, or certified by any regulatory authority worldwide, including but not limited to:

- The U.S. Food and Drug Administration (FDA)
- EU Notified Bodies (no CE marking under MDR 2017/745)
- Health Canada
- Australia's Therapeutic Goods Administration (TGA)
- Any equivalent national regulatory authority

### Not a Medical Device

**This software is NOT a medical device.** It is experimental open-source software provided for educational and informational purposes only. No individual, organization, or entity associated with this project is the "manufacturer" of a medical device under any regulatory framework.

GlycemicGPT does not control any medical devices. It reads data from insulin pumps and continuous glucose monitors (CGMs), displays that data, and provides AI-generated text suggestions. These suggestions are not medical advice and must not be treated as such.

### Health Data Processing

This software processes health data including:

- Continuous glucose monitor (CGM) readings
- Insulin pump telemetry (basal rates, bolus history, insulin on board)
- Pump hardware status (battery, reservoir levels)
- User-configured therapy parameters (target glucose ranges, insulin ratios)

Users are responsible for understanding the privacy and security implications of their deployment. The self-hosted architecture means users control where their data resides.

When using the BYOAI (Bring Your Own AI) feature, glucose data context is sent to the configured AI provider (Anthropic, OpenAI, Ollama, or a self-hosted endpoint). Users should review their AI provider's data handling policies.

### AI Limitations

AI-generated suggestions in this software are produced by large language models (LLMs) that are known to:

- **Hallucinate** -- generate plausible-sounding but incorrect information
- **Misinterpret data** -- draw incorrect conclusions from glucose readings
- **Provide outdated information** -- not reflect the latest medical guidelines
- **Lack context** -- not understand your complete medical history, comorbidities, or current medications

All AI-generated content in this software is labeled as suggestions, not medical advice. Never act on AI suggestions without consulting your healthcare team.

### Critical Warnings

1. **Do not replace professional medical care.** Always consult with your endocrinologist, diabetes educator, or healthcare provider before making any changes to your diabetes management.

2. **Verify all suggestions.** Any insulin dosing, carb ratio, or correction factor suggestions must be verified with your healthcare team before use.

3. **Use extreme caution.** Incorrect diabetes management can result in severe hypoglycemia, diabetic ketoacidosis (DKA), or other life-threatening conditions.

4. **If you experience a diabetes emergency, contact your healthcare provider or emergency services immediately.** Do not rely on this software for emergency medical guidance.

### Future Pump Control Features

Pump control capabilities (insulin delivery commands) are not included in pre-built releases of this software. If such features are implemented in the future:

- They will be available only by building from source code
- They will require explicit user opt-in and acknowledgment of risks
- They will be subject to additional safety validation pipelines
- Users who build from source assume full responsibility as the "manufacturer" of their personal build

### Untested Device Compatibility

This software may declare protocol compatibility with devices that have **not been tested against physical hardware**. Protocol compatibility (shared BLE protocol, authentication mechanism, and data formats) does not guarantee correct operation. Specifically:

- Data displayed from untested devices may be inaccurate, delayed, or missing
- BLE connection behavior, reconnection stability, and pairing flows may differ
- Safety-critical values (insulin on board, glucose readings, basal rates) must always be verified against the device manufacturer's official companion app
- Users who choose to use this software with untested device hardware accept all associated risk

No contributor, maintainer, or entity associated with this project is liable for any adverse outcome resulting from use with untested or partially-tested device hardware. If in doubt, use only the device manufacturer's official software.

### LIMITATION OF LIABILITY

THE AUTHORS, CONTRIBUTORS, AND MAINTAINERS OF THIS SOFTWARE PROVIDE IT "AS IS" WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NONINFRINGEMENT.

IN NO EVENT SHALL THE AUTHORS, CONTRIBUTORS, OR MAINTAINERS BE LIABLE FOR ANY CLAIM, DAMAGES, OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT, OR OTHERWISE, ARISING FROM, OUT OF, OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

THIS INCLUDES BUT IS NOT LIMITED TO ANY DAMAGES, INJURIES, OR ADVERSE HEALTH OUTCOMES RESULTING FROM THE USE OF THIS SOFTWARE. BY USING GLYCEMICGPT, YOU ACKNOWLEDGE THAT:

- You are using this software entirely at your own risk
- You will not rely solely on AI-generated suggestions for medical decisions
- You understand that AI can and will make errors
- You will maintain regular care with qualified healthcare professionals
- You accept full responsibility for any decisions made based on this software's output
- No individual or entity associated with this project is liable for medical outcomes

### License Warranty Disclaimer

This software is licensed under the GNU General Public License v3.0 (GPL-3.0). Per Sections 15-17 of the GPL-3.0:

- **Section 15:** THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY APPLICABLE LAW. THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU.
- **Section 16:** IN NO EVENT UNLESS REQUIRED BY APPLICABLE LAW OR AGREED TO IN WRITING WILL ANY COPYRIGHT HOLDER, OR ANY OTHER PARTY WHO MODIFIES AND/OR CONVEYS THE PROGRAM AS PERMITTED ABOVE, BE LIABLE TO YOU FOR DAMAGES.
- **Section 17:** If the disclaimer of warranty and limitation of liability provided above cannot be given local legal effect according to their terms, reviewing courts shall apply local law that most closely approximates an absolute waiver of all civil liability in connection with the Program.

See the [LICENSE](https://github.com/GlycemicGPT/GlycemicGPT/blob/develop/LICENSE) file for the complete GPL-3.0 text.

**Jurisdictional note:** Limitation of liability clauses for personal injury may be unenforceable in some jurisdictions, including under EU consumer protection law, UK consumer rights legislation, and Australian consumer law. The build-from-source distribution model, where the individual user is the "manufacturer" of their personal build, is the primary risk mitigation strategy. This disclaimer does not constitute legal advice.

> The canonical version of this disclaimer is maintained in the main repository at [`GlycemicGPT/MEDICAL-DISCLAIMER.md`](https://github.com/GlycemicGPT/GlycemicGPT/blob/develop/MEDICAL-DISCLAIMER.md). In case of any discrepancy, the main repository version governs.

---

<p align="center">
  <strong>Licensed under <a href="https://github.com/GlycemicGPT/GlycemicGPT/blob/develop/LICENSE">GPL-3.0</a></strong><br/>
  <sub>Built with care for the diabetes community. Stay safe. 💙</sub>
</p>
