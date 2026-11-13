---
name: app-store-google-play-optimice-and-resubmit-autonomously-skill
description: Audit and improve an existing App Store or Google Play app, resolve every current rejection and submission or purchase blocker, optimize truthful localized metadata and device-specific media, build and submit the intended revision, and verify the exact provider submission. Use for rejected apps, unproven submission claims, release recovery, resubmission, store-listing optimization, review videos, or approved products that cannot be purchased.
---

# App Store and Google Play Optimization and Resubmission

Carry the requested app through its actual review submission. Recover the original requirements and current provider records, implement every missing in-scope change, align the listing and reviewer information, and verify the exact submitted binary. When the request covers several apps or a numbered proposal set, cover the complete population.

## Establish the target and current truth

- Bind the repository, branch, source revision, developer account, app identifier, bundle ID or Android package, platform, version, build number or versionCode, target track, countries, localizations, and requested outcome. Resolve the provider mapping from its current record before mutations. A matching display name is insufficient.
- Read local instructions, release tools, signing/export configuration and relevant history. Compare earlier completion claims against the actual code and current store records. Preserve unrelated dirty work and one submission owner per app.
- Recover current rejection messages in full, including follow-up replies and attachments, from App Store Connect App Review or Play Console Policy status and the connected mailbox. Read the provider conversation even when an API state already says rejected. Messages may be unavailable through a public API. Use native mail tools or IMAP for email.
- For each issue, retain the exact guideline or policy, reviewer observation, affected candidate, required evidence, owning source or metadata field, action and provider readback in the runtime task ledger. Keep this transient ledger out of the repository unless requested.
- Read the full discoverable requirement set before classifying it. Numbered proposals marked complete still require code inspection through their normal entry points, persistence, lifecycle, accessibility and localization paths. Recount against the complete set after fixes.
- Check current official documentation for the installed SDK, API and store policy before unfamiliar calls or a material route change. Reuse a working repository integration instead of inventing another service or wrapper.
- Distinguish uploaded, processed, selected, submitted, waiting, in review, approved, released, available and purchased. Report the provider state that was actually read. A CI badge, successful upload, agent summary, TestFlight readiness email or release draft does not establish review submission.

## Recover relevant prior work

When release history or earlier attempts change the action, use available conversation history and Chronicle to reconstruct recent cross-app and CLI work. Read the current Chronicle skill at `~/.codex/skills/chronicle/SKILL.md`, its instructions at `~/.codex/memories_extensions/chronicle/instructions.md` and relevant `~/.codex/memories_extensions/chronicle/resources/*.md` when present. Screenpipe can locate a decisive previous review message or release action through `~/.codex/screenpipe-memories.md` and its currently exposed search tools. Record actual source coverage in the runtime ledger. Historical artifacts are evidence. Verify the underlying current source or provider record before acting.

## Read App Store Connect and Google Play

Prefer the existing authenticated API or CLI for supported operations. Use the provider UI for review messages, privacy publishing, attachments and operations the supported API cannot express. Preserve successful reads when one route fails.

For Apple, read the current app version, its selected build, processing state, parent review submission, submittedDate and submission items. Include attached subscriptions, subscription groups and IAPs when present, then read each product's own state. Follow pagination. A submission item in READY_FOR_REVIEW does not determine the parent state. A COMPLETE parent with removed app-version items does not establish approval.

For Google, read package identity, application access, current track, release status, versionCodes, edit state, Publishing overview, Policy status, App content and any product or subscription records in scope. Verify that changes reached review. A committed edit or uploaded AAB can leave changes pending in Publishing overview. Managed publishing also separates review approval from public release.

Use existing global environment values, including `~/.env` when available, and the exact-account saved credentials. Relevant variable names include `ASC_KEY_ID`, `ASC_ISSUER_ID`, `ASC_KEY_PATH`, `ASC_APP_ID`, `ASC_BUNDLE_ID`, `PLAY_PACKAGE_NAME`, `PLAY_SERVICE_ACCOUNT_JSON`, `REVENUECAT_PROJECT_ID` and `REVENUECAT_SECRET_KEY`. Keep credential values in the tool layer.

For App Store Connect ES256 authentication, use the maintained client or PyJWT. The signature requires raw 64-byte R||S encoding. A DER signature can cause 401 NOT_AUTHORIZED despite a valid key. A 404 can indicate wrong account scope, key, app ID, package or endpoint. Diagnose that layer before inferring the app is absent. A public lookup with zero results establishes only absence from that lookup at that time and storefront.

Compute review age from the newest relevant non-cancelled submission's submittedDate. Check subsequent native events before describing it as waiting. Upload dates and first TestFlight emails cannot establish elapsed review time. Check the provider's current review-time guidance if escalation is relevant, and distinguish a published baseline from an SLA. Preserve an unchanged queued candidate. Replace it when the user requests a new release or verified fixes require a replacement, after preparing the new eligible candidate.

## Implement every actionable rejection

Fix root causes in the app, backend, companion app, listing and submission configuration as the rejection requires. Audit sibling instances of a proven defect. Keep scope tied to the user's app and current requirements.

- Reviewer access must work against the intended live backend. Align demo credentials, account state, sample content, subscription state, companion-host availability and instructions with the actual submitted build. Exercise accessible real account operations within the user's constraints.
- Explain required hardware and dependencies accurately. A hardware-dependent app needs clear connection instructions, supported devices, companion software links, permissions and a useful ordinary-device or disconnected state where the product supports one. Fix broken discovery, pairing, permission recovery and reconnect behavior rather than hiding the capability after one failed attempt.
- Remove an unused background mode, entitlement, permission or capability when source inspection establishes it is unnecessary. Preserve a required capability and supply the reviewer-visible implementation and evidence it needs. An unrelated controller failure does not justify disabling a feature. Temporary restrictions need a concrete cause, scope and removal condition, without arbitrary retry quotas.
- Resolve crashes, empty or placeholder flows, broken links, incomplete content, invalid imports, inaccessible controls and state loss through normal entry points. Include older persisted data and interruptions when relevant. Keep UI permission explanations specific to implemented behavior.
- Align privacy policy, published App Privacy or Data safety answers, SDK behavior, permission usage, account deletion and data handling with the actual app and backend. Saved privacy answers require their provider publishing step where applicable.
- Complete price confirmations and reviewer questions in the current native thread. Resolve terms or EULA links, support and privacy URLs, age/content ratings, export compliance, content rights and required declarations with factual information from the app.
- Answer every numbered or nested reviewer information request individually. Supply the requested evidence for each section, retaining exact scope, even when a demonstration video addresses another section. For originality or template-related concerns, inspect source ancestry, licenses, reused frameworks, backend ownership, customization and the actual distinct user workflows. Describe provenance and development responsibility truthfully, remove incomplete beta or placeholder presentation where the app is intended for release, and demonstrate the implemented value. Avoid unsupported claims that the app was built entirely from scratch or generic replies that leave the reviewer questions unanswered.
- Inspect draft platform versions when an error identifies a conflicting record. Complete or remove only the obsolete record whose condition is proved, preserving another platform's active release.
- For Google, resolve the exact current App content, app-access, target SDK, signing, testing eligibility and policy requirements shown for that account. Verify App Links against the Play app-signing certificate fingerprint when that is the actual failing condition.
- Fix release automation that swallows API errors, assumes app mappings, submits the wrong version, or reports upload as review success. Require a provider submission result and a separate readback bound to the requested candidate.

## Optimize metadata and authentic device media

Inspect the existing app and public listing before rewriting copy. Lead with its actual primary benefit, then explain supported workflows and hardware requirements. Match the user-requested voice. Use current store field limits, meaningful keywords, appropriate categories and truthful localized release notes. Validate every locale and preserve existing good translations. Keep internal prompts, review status, source hashes and operational language out of customer-facing copy.

- Verify the actual supported device families, orientations, size classes, safe areas, accessibility text sizes, contrast, reduced-motion behavior, touch targets and window or fold transitions in source. Device adaptation should follow current SDK capabilities and geometry instead of an unverified marketing device name.
- Build screenshot coverage from the app's supported devices and the store's current accepted display sets. Keep required phone, tablet and other platform sets distinct. Reuse provider scaling only where allowed. Check dimensions, cropping, readable text, order, localization and uploaded asset-processing state.
- Use authentic captures from the relevant app build or accurately reusable existing captures. Marketing frames and captions may complement genuine app UI. Avoid fabricated app screens, stretched phone captures presented as tablet layouts, or a hardware photo represented as a native UI capture. Replacing one scaled source can affect other display sizes, so read back inherited sets.
- When the user prohibits device or simulator tests, obey that restriction throughout delegated work and scripts. Use source inspection, existing meaningful code checks, static layout reasoning, compilation and archive/export checks as permitted. Do not launch devices or simulators to obtain new captures. Use existing genuine suitable assets and disclose any exact capture gap.
- Keep public app previews and private reviewer evidence separate. Apple's public app preview specifications differ from a review demonstration of required hardware. Read the relevant current contract for each asset type.

Current primary references are the starting points. Check their latest content for the exact operation.

- [Apple screenshot requirements](https://developer.apple.com/help/app-store-connect/reference/app-information/screenshot-specifications)
- [Apple screenshots showing the app in use](https://developer.apple.com/help/app-review/guideline-reference/2-3-3-screenshots/)
- [Apple review information and hardware video](https://developer.apple.com/help/app-review/before-submitting-for-review/complete-review/)
- [Google review and publishing controls](https://support.google.com/googleplay/android-developer/answer/9859654)

## Prepare and submit the intended build

1. Finish feasible app, listing, reviewer-access and compliance corrections first. Confirm source status and the exact final revision. Check available builds before choosing a unique new build number or versionCode. Keep the marketing version consistent with the provider's editable version.
2. Run the smallest meaningful checks permitted by the user and repository. A code-only request can permit compilation and archiving while excluding device and simulator execution. Disclose this proof boundary and avoid claiming runtime behavior was exercised.
3. Archive, sign, export and upload through the existing release route. Bind the artifact to source revision, bundle/package, version, build number, signing identity and upload result. Preserve working signing configuration. Wait for processing and resolve actual processing failures.
4. Verify the new candidate is valid and eligible before replacing a selected candidate or cancelling a previous submission. Keep one writer. Read current submission state immediately before mutation to preserve concurrent review progress. If a state transition prevents replacement, use the supported operation for that state and report its result.
5. Apply the reviewed localized listing, device media, reviewer notes and required product attachments. Read back critical fields. Finish console-only gates directly through the authenticated UI when the API lacks that operation.
6. Submit through the supported store operation, including Google's Send changes for review when required. Treat an API rejection as a failed operation. Resolve the actual error and retain previously completed steps.
7. Read back the parent submission or release review state, selected binary, submission identifier or native equivalent and timestamp. Confirm all intended products and changes are included. A version item alone is insufficient. If the provider offers no submission ID, retain the exact package, track, versionCodes, native state, timestamp and confirmation reference.
8. Update the original authorized project or conversation with the actual source revision, submitted candidate, provider state, timestamp and genuine remaining dependency. Verify that the update persisted. Avoid directing another executor to race the active submission owner.

## Required real-device video

Inspect the actual reviewer request and Apple's current hardware guidance. When hardware evidence is required, use a genuine recording showing the submitted app on a physical Apple device connecting to and interacting with that hardware. A simulator recording or app-only screen capture cannot demonstrate the physical setup. Never fabricate device evidence or describe a video as supplied before the attachment or accessible link exists.

Complete all feasible code, build, metadata and submission work first. If a physical holder must record evidence under the user's constraint, provide a compact, paste-ready flow containing all of the following.

1. Exact app version and build, install route, required device/hardware and companion app version, and initial pairing or permission setup.
2. A continuous camera view showing the real device, hardware and relevant companion display, with the build identifier visible at the start.
3. Exact taps and visible results for every reviewer observation, including connection, the core interaction, both sides or displays where relevant, and any recovery or permission flow the rejection identifies.
4. Expected result at each step, brief narration only where needed, and a readable ending showing successful completion. Ask the holder to return the original video without substituting promotional edits.
5. The destination for the returned recording. After it is available, verify playback, provide an accessible review attachment or link, reply to the current native review conversation, and read back the submission state.

Do not insert invented UI labels or steps. Derive the flow from the final code and current reviewer request. If missing footage is the only remaining dependency, identify the exact request and current submission state once, keeping uploaded and submitted outcomes distinct.

## Purchases, subscriptions and commercial availability

Apply this section only where purchase or subscription functionality is in scope. Keep product state, SDK catalog, paywall rendering, store transaction, entitlement delivery and revenue as separate facts.

- Read every relevant store product's own state, attached submission and regional availability, including base plans, offers and future-region settings. A READY_TO_SUBMIT product or an unsent draft is a submission problem even when the app itself is approved.
- The first Apple IAP or subscription needs the app-version submission route required for its product type. Resolve native errors such as a missing pending app version by attaching it to an eligible version. A standalone product attempt can fail while the product configuration is valid.
- Verify commercial agreements, banking and tax completion when they block sales. Complete the supported administrative steps within the task's authority and distinguish provider acceptance from saved configuration.
- Inspect the offering identifier the app actually requests, its packages, product mappings and entitlements. A non-current RevenueCat offering may be intentionally requested by identifier. The current flag alone cannot diagnose a broken paywall.
- SDK catalog drift, null durations or stale labels do not establish invalid store prices or periods. Read the store product. Verify currency, recurring period, trial, renewal disclosures and EULA or Terms links against the actual purchase rail.
- When a product API reports migration, use the currently supported endpoint. Correct a rejected converted regional price against the store's current limit. Preserve successful reads and other healthy products.
- Grant access or balance against the matching verified transaction, handle retries and restoration consistently, and show purchase success only after entitlement delivery. Inspect web checkout's actual billing rail and renewal behavior instead of assuming SDK web billing is active.
- A StoreKit local test configuration, sandbox sheet, development build or TestFlight purchase cannot establish production charging. Bind observed behavior to the installed build and environment. Production purchase proof requires the public store build, a real store transaction and matching entitlement. Record only the actual evidence available within the user's testing constraints.
- Unpaid test orders do not establish revenue. Clean them up when authorized and verify that balances and entitlements remain consistent. Recheck exact console/account/device access before treating an earlier tooling failure as a permanent blocker.

## Completion

Reconcile the full requirement and rejection population against source and provider readback. Report changed source revision, app/platform, exact version and build or versionCodes, submission identifier or native equivalent, review state and submitted timestamp in local time. Include fixed rejection reasons, published metadata and media coverage, permitted checks actually performed, and any precise unresolved physical-evidence or provider dependency. Distinguish current provider reads from historical records and inference.

Stop after the requested outcome is verified and no authorized feasible step remains. Do not create new scheduled loops, reset review merely to refresh the queue, repeat proven successful operations, or turn an unavailable preferred tool into a task-ending gate.
