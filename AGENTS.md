---
description: Apoorv global development instructions
alwaysApply: true
---

Before every update to these shared CLI instruction files (Codex AGENTS.md, Claude CLAUDE.md, or OpenCode AGENTS.md), create a dated, timestamped backup of each file being changed under ~/Documents/cli-settings/backups/. Preserve the originals in the backup, follow symlinks to back up their contents, and keep the active symlinks intact. Never overwrite an earlier backup.

For every code change, create a git commit and push it to the current branch before finishing, unless the user explicitly says not to or pushing is impossible.

Use the agent-browser MCP for web browsing by default (headless, logged out). If a site needs my login, relaunch it with `--profile Default` to reuse my Chrome logins.
Use the cua-driver MCP for computer use (controlling Mac apps) by default. Start each cua-driver session with `cursor_motion: {"timing": "fixed", "glide_duration_ms": 1, "dwell_after_click_ms": 0}` so the cursor jumps instead of gliding and clicks without a pause.


## Marketing

Apply these rules only when I explicitly ask you to market or promote a product.

- Prioritize X, YouTube, Product Hunt, and Hacker News.
- Never suggest Reddit or use Reddit for marketing or promotion.
- When using agent-browser for marketing, if a bot check, CAPTCHA, or human-verification check (including Cloudflare) cannot be passed through normal browser interaction, silently skip that marketing destination and move on to another idea. Do not repeatedly retry, switch to a headed browser to get around it, ask me to complete verification, or notify me about the skipped check. Record the destination internally as blocked and not submitted.
- Also pursue relevant GitHub PRs, directories, newsletters, showcases, provider partnerships, creator outreach, and submission forms.
- Check previous posts, submissions, and outreach for duplicates before acting.
- Tailor each pitch to the product, audience, and channel; follow their submission guidelines.
- Send marketing emails only from apoorv@fud-ai.app using the Fud Mail MCP.
- Use valid promo codes to provide complimentary access for relevant outreach.
- Record submissions, contacts, links, outcomes, and promo codes issued so future campaigns can avoid duplicates and follow up appropriately.

# Device install (mobile app development only)

Apply this **only** to work on **installable mobile apps** (iOS/Android, e.g. Fud AI / calorietracker) when the change affects what runs on device.

This policy does **not** apply to anything else. Never rebuild, install, relaunch, or open a non-mobile target because of this policy — that includes macOS/desktop apps (e.g. Browser Cookie Bridge), Electron/CLI tools, browser extensions, backend services, web apps, and libraries. For those, build/test only what the task needs and finish; do not touch system installs unless I explicitly ask.

Skip entirely for non-app repos, non-mobile apps, docs-only, backend-only, or when I say not to install / no mobile device is connected.

Personal test installs for Apoorv only — not App Store / Play Store release shipping.

## Default: Release (preserve data)

After every code change that affects the **mobile** apps, rebuild, install, and open on my connected phones before finishing (unless I explicitly say not to or a device is disconnected):

- **iOS**: Release build only. Install over the existing App Store / prior install. Never uninstall first.
- **iOS DerivedData (critical, Fud AI / calorietracker)**: Always reuse a stable path — prefer `~/Library/Developer/Xcode/DerivedData/calorietracker-device-release` with `-derivedDataPath`. Never `rm -rf` DerivedData before a normal install build, and never invent a fresh `/tmp/...` tree each attempt. Cold Release rebuilds of heavy SPM deps (e.g. RevenueCat) take 15–30+ min; wiping forces that every time. Incremental reuse is required.
- **Android**: Release build only. Use `adb install -r` over the existing release / prior install. Never uninstall first or clear app data. Do not expect to overwrite the Play Store package if signatures differ.
- **Fud AI package IDs (release)**: iOS/Android `com.apoorvdarshan.calorietracker` — launch after install.
- **Android launch (critical)**: Never use `adb shell monkey` to open the app. On this phone, monkey flips system auto-rotate on (`accelerometer_rotation=1`) even when portrait is locked. Launch with `am start` instead, e.g. `adb shell am start -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -p com.apoorvdarshan.calorietracker` (use the debug package id for debug installs). Do not change rotation / auto-rotate settings unless I ask.
- Preserve existing on-device **release** data unless I explicitly ask to wipe or reinstall clean.
- Prefer the currently connected device (Apoorv’s iPhone / USB Android) when installing.
- After promoting app assets (e.g. workout images), rebuild so they are in the binary before install; spot-check a sample asset in the build artifact when relevant.

## Explicit Debug installs (only when I ask for debug)

When I **explicitly** ask for a debug build/install (e.g. to test onboarding once):

- Use the dedicated **debug** app identity — do **not** install debug over the release/Play/App Store app.
- **Launcher name must be `Fud AI Debug`** (not plain `Fud AI`) so it is visually distinct next to release. Keep that name on both platforms.
- **Same debug app every time** — overwrite the existing **Fud AI Debug** install (same icon / same debug package). Do **not** create a second debug copy (no extra `.debug2` / duplicate icons unless I ask).
- **Fresh data** — always wipe the **debug** app’s data before or as part of install so onboarding and first-run flows can be tested cleanly. Do **not** wipe release app data.
  - **Android**: clear/wipe `com.apoorvdarshan.calorietracker.debug` (e.g. `adb shell pm clear …` and/or uninstall that debug package only), then `adb install -r` the debug APK; launch the debug package.
  - **iOS**: uninstall only `com.apoorvdarshan.calorietracker.debug` (or otherwise reset that debug app’s container), then install Debug over that same bundle id; launch the debug app. Never uninstall the release `com.apoorvdarshan.calorietracker` app for this.
- **iOS Debug DerivedData**: reuse a stable debug path such as `~/Library/Developer/Xcode/DerivedData/calorietracker-device-debug` — still don’t invent a new `/tmp/...` tree each attempt.
- Package IDs: debug `com.apoorvdarshan.calorietracker.debug` (iOS + Android).

## OpenSSF badges and security practices

Apply this workflow when I explicitly request OpenSSF badges, passing
certification, or Scorecard setup for a repository.

- Complete the work end to end: inspect current official requirements,
  implement applicable missing practices, run checks, complete the
  Best Practices Passing assessment, and verify the published result.
  Do not stop at a checklist when the remaining work is authorized.
- Also configure OpenSSF Scorecard, publish its results, and improve
  actionable findings with appropriate dependency updates, security
  analysis, meaningful fuzz testing, CI, and branch protections.
- Add both live badges near the top of the README, linked to their
  public assessments. Verify that the badges actually render.
- I am a sole maintainer. Never require another person's approval:
  required approving reviews must be zero; code-owner, last-push,
  and other extra approval requirements must be disabled.
  PRs and passing automated checks may be required, but I must be
  able to merge my own PRs without another reviewer.
- Follow repository protections: use branches and PRs when necessary,
  resolve conflicts, wait for checks, and merge authorized changes.
  Do not bypass checks or fabricate reviews to increase a score.
- Use truthful evidence. Distinguish the Best Practices Passing badge
  from the numeric Scorecard rating. Do not invent attestations,
  claim unperformed tests, or promise a perfect score.
- Ask for genuinely missing maintainer attestations. Get explicit
  authorization before making private code public or choosing/changing
  its license, unless already authorized in the conversation.
- Report verified badge links, the actual score, completed checks,
  and remaining limitations. Badge work does not authorize an
  App Store/Play Store release.

## Mobile release switches

Apply only when I explicitly request creating or changing iOS/Android
release switches or store automation.

- Give each platform its own master switch and separate controls for
  individual store operations. New switches default to off; missing
  settings mean disabled; reject invalid values.
- Document exactly what each switch controls. Store listing updates
  need their own gate, even when binary upload or rollout is disabled.
- Creating or testing this automation does not itself authorize
  submitting or publishing an app.

### iOS

- Separate controls for listing metadata, screenshots, What's New,
  review submission, and public release.
- Before review submission, wait for the Xcode Cloud run matching the
  exact release tag and commit to complete successfully.
- Poll every 30 seconds for up to 2 hours total, including upload and
  App Store Connect processing. Make both settings configurable and
  give the CI job enough time for the remaining release steps.
- Require a valid, unexpired build for the intended app and version.
  Explicitly select it and recheck that selection before submission.
- Refresh authentication during long waits. On timeout, build failure,
  ambiguity, or mismatch, stop submission; never substitute an older build.
- This wait covers build readiness, not Apple review or public release.
- Document Xcode Cloud's independent trigger; a GitHub workflow switch
  alone does not disable Xcode Cloud builds.

### Android

- Separate controls for signed artifact builds, listing metadata,
  screenshots, What's New, binary upload, and production rollout.
- Default the Play destination to none. Support explicitly configured
  testing tracks; production must require explicit opt-in.
- Validate the app identity, version, signing, and intended artifact
  before upload. Never silently fall back to production.
- Verify the upload and track update before reporting success.
  Distinguish uploaded, submitted, and publicly available states.

Validate disabled paths, failures, and timeouts without publishing.
