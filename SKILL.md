---
name: app-store-submission-blockers-skill
description: Diagnose and clear App Store and Google Play submission and purchasability blockers using the provider's own records instead of agent summaries. Use when an app is rejected, stuck in review for days or weeks, claimed as submitted but unproven, blocked by metadata, IAP, privacy or reviewer-access gates, when repeated build swaps keep restarting the review clock, or when products are approved but nobody can actually buy them (unsubmitted IAPs, offering wiring, regional availability, test-versus-production purchase evidence).
---

# App Store Submission Blockers Skill

## Purpose

Find the real blocker, prove it from the provider's own records, fix it, and keep the review queue moving. This is a diagnosis-and-unblock skill: it is not store media generation, not ASO copywriting, and not deployment-stack selection.

Never treat an agent summary, a CI badge, or a TestFlight email as proof that a build is in review. Prove the current state first, then act.

## Evidence rules (apply before any conclusion)

- Upload is not submission. A processed build can be App Store eligible for TestFlight while no App Review submission exists.
- TestFlight is not App Review. "Ready to test" emails say nothing about review state.
- CI green is not submission. A swallowed API error can leave the pipeline green with nothing submitted.
- An API 404 is not "the app does not exist". It usually means wrong scope, wrong key, wrong app id, or a purged record.
- A submission item reading READY_FOR_REVIEW is not proof the parent submission is missing. Read the parent submission, the app version, and the product records.
- Verdicts and issue notices arrive by email, not through the API. Read the mailbox before concluding that the provider is slow.
- Timestamps decide the story. Resolve the current submission's submittedDate before describing how long something has "waited".
- Label every fact: [API-verified], [recorded in a source], [inferred]. Never present a summary as a provider read.
- Configuration is not a completed purchase. A verified product catalog, an active offering, and a rendering checkout page are not revenue; only a store transaction with a matching receipt is.
- A store test purchase sheet proves nothing about production charging: it comes from development, TestFlight or sandbox installs, and a local StoreKit test configuration in the run scheme produces the same sheet.
- Compare the installed build against the public store version before interpreting any on-device behavior; a developer build behaves differently from the store build.

## Step 1 — Read the provider's own records (read-only first)

Use an App Store Connect API key for state, not a browser session. Read-only calls cannot change the queue.

```bash
# Mint an ES256 JWT from the .p8 key. The signature must be the raw 64-byte R||S form;
# an ASN.1 DER signature returns 401 NOT_AUTHORIZED and looks like a revoked or wrong key.
# PyJWT's ES256 encoder already emits the raw form.
ASC_KEY_PATH="$ASC_KEY_PATH" ASC_KEY_ID="$ASC_KEY_ID" ASC_ISSUER_ID="$ASC_ISSUER_ID" python3 - <<'PY'
import os, time, jwt
key = open(os.environ["ASC_KEY_PATH"]).read()
now = int(time.time())
print(jwt.encode({"iss": os.environ["ASC_ISSUER_ID"], "iat": now, "exp": now + 900,
                  "aud": "appstoreconnect-v1"}, key, algorithm="ES256",
                 headers={"kid": os.environ["ASC_KEY_ID"]}))
PY

# Then, with TOKEN set to that JWT:
curl -s -H "Authorization: Bearer $TOKEN" \
  "https://api.appstoreconnect.apple.com/v1/apps?filter[bundleId]=$ASC_BUNDLE_ID" | jq .
curl -s -H "Authorization: Bearer $TOKEN" \
  "https://api.appstoreconnect.apple.com/v1/apps/$ASC_APP_ID/reviewSubmissions?limit=200" | jq .
curl -s -H "Authorization: Bearer $TOKEN" \
  "https://api.appstoreconnect.apple.com/v1/reviewSubmissions/$SUBMISSION_ID/items" | jq .
curl -s -H "Authorization: Bearer $TOKEN" \
  "https://api.appstoreconnect.apple.com/v1/apps/$ASC_APP_ID/appStoreVersions?limit=50&include=build" | jq .
```

Read, for each platform: the selected review build, the parent submission state and submittedDate, the submission items (app version, subscriptions, subscription group, in-app purchases), and each product's own state. A public store lookup returning resultCount 0 proves the app has never been live.

Google Play: verify the track and release state, and remember that uploaded changes stay pending until someone clicks Send changes for review in Play Console. Service-account scope errors return 404 for a package that exists.

## Step 2 — Compute the real wait before calling anything "stuck"

- Take the newest non-cancelled submission's submittedDate and compute elapsed time from it, not from the first upload or the first TestFlight email.
- Count the history. COMPLETE submissions whose app-version items were removed are not approvals; ten of those plus two waiting submissions means churn, not a slow queue.
- Removal and resubmission restart the review process. Every cancellation, build replacement, or version bump resets the clock.
- The provider's published baseline is that 90% of submissions are reviewed in under 24 hours. That is not a per-app SLA, and it makes a multi-week "wait" a symptom to investigate rather than a cause.

## Step 3 — Blocker catalogue (symptom, cause, fix)

1. Missing Terms of Use / EULA link on auto-renewable subscriptions. Cause: the subscription metadata has no functional Terms of Use link, which the guidelines require. Fix: add a working EULA link (the provider's standard EULA or your own) to every localization, then resubmit.
2. A price-confirmation or reply request. Cause: the provider asks the developer to confirm a price or answer a question in App Store Connect. Fix: reply the same day in the web UI; this state is not visible through the API.
3. App Privacy answers selected but never published. Cause: the answers were saved, but the publish step, which exists only in the web UI, was never completed. Fix: finish and publish it; it silently blocks submission.
4. In-app purchases or subscriptions not attached to the submission. Cause: products exist but are not part of the same submission, or the first product attach returned 409 Conflict. Fix: attach every product to the submission, and finish the first product in the App Store Connect web UI before retrying the API call.
5. Reviewer demo account missing, disabled, or not actually working. Cause: credentials in the review notes point at an account that does not exist in production. Fix: create it, prove a real production login, and align the account state with what the review notes promise.
6. Dangling draft platform listings. Cause: a draft for another platform, for example a macOS 1.0 draft on an iOS-only release, stays in PREPARE_FOR_SUBMISSION. Fix: remove or complete it; drafts can block the reviewer pipeline.
7. Wrong app record. Cause: automation targets a stale or wrong public store id or bundle id. Fix: assert the bundle id to app id mapping in the release config before every submit, and fail loudly on mismatch.
8. Automation reporting success for a failed submission. Cause: the submission error handler logs errors such as "submission already in progress" or "build could not be added" and then treats the upload as successful. Fix: make the handler hard-fail, and require a returned submission id in the release log. Otherwise a stale submission can hold an old build while new builds only reach TestFlight.
9. Submission churn. Cause: repeated cancellations, build swaps and version bumps, each restarting review. Fix: freeze one candidate, name one submission owner, and stop resetting the queue.
10. Unjustified capabilities. Cause: a declared background mode or entitlement with no reviewer-visible evidence. Fix: remove the unused capability, or provide reviewer-visible evidence and a justification in the review notes.
11. Google Play specific gates. Uploaded changes stay unsent until Send changes for review is clicked in Publishing overview; App Links need the Play app-signing SHA-256, not the upload key; new personal accounts face closed-testing requirements before production.
12. Auth and tooling traps. DER-encoded JWT signatures, wrong key id or issuer, service-account scope 404s, and verdicts that arrive only by email.

## Step 4 — Hold, fix, or escalate

- Genuinely queued and unchanged: hold. Do not swap the binary to "refresh" review; a replacement restarts the clock.
- Replace the candidate build only for a verified release-blocking defect.
- Suspect the human-only gates first (price replies, privacy publish, console clicks). They are invisible to the API and are the most common reason something looks stuck.
- Escalate through App Review Support after a genuinely unchanged wait of about 48 hours. That checkpoint is an operational recommendation, not a provider SLA.
- Never disable, hide, or gate a capability because one attempt failed. Require 3 distinct approaches and 2 independent source layers before any temporary gate, and leave a comment such as: // TEMP-GATE: YYYY-MM-DD - tried: ... - not tried: ... - removal condition: ...

## Step 5 — Report and verify

Report per platform: candidate version and build, submission id, state, submitted timestamp in local time, elapsed wait, open human-only items, next action, and an evidence label for each line. State explicitly what was not read (mailbox reconnection, unreadable documents, unavailable sources) instead of implying full coverage.

Verify before closing: the submission id you quote exists and matches the app version you name; the products you list are the ones attached to that submission; the public store lookup matches the claim about whether the app has ever been live; and no submission, cancellation, or release setting was changed unless the task asked for it.

## Step 6 - Purchase and product blockers (IAP, subscriptions, availability)

Approved products that nobody can buy look exactly like a demand problem. These are the blockers seen in practice, with the record that proves each one.

```bash
# Purchase SDK catalog (RevenueCat v2 REST shape): which offering can the app request, and what is attached to it
curl -s -H "Authorization: Bearer $REVENUECAT_SECRET_KEY" \
  "https://api.revenuecat.com/v2/projects/$REVENUECAT_PROJECT_ID/offerings" | jq .
curl -s -H "Authorization: Bearer $REVENUECAT_SECRET_KEY" \
  "https://api.revenuecat.com/v2/projects/$REVENUECAT_PROJECT_ID/products" | jq .
# Confirm the current endpoint shape against the provider docs; a healthy catalog is not a completed purchase.
```

1. First in-app purchase cannot be submitted on its own. The first product of each type must go out with a new app version; a standalone attempt fails with an error such as "InAppPurchase has no pending version for submission". Fix: attach the products to the next app version and submit that version.
2. Products that were never submitted are not purchasable. A product sitting in READY_TO_SUBMIT, or the Play equivalent, cannot be bought regardless of app review state. Check each product own state, not just the app state.
3. Incomplete commercial agreements. An unsigned Paid Applications Agreement, or unfinished banking and tax details, blocks every in-app purchase even when the app is already live.
4. Products attached to a draft that was never sent. A submission in READY_FOR_REVIEW with no submission date means review has not started; the remaining action is to send it, or to attach the newer build first.
5. Offering and entitlement wiring. A current offering may contain only part of the catalog, and a non-current offering is not shown by a default paywall. The app may also already request a non-current offering by identifier, so the current flag alone is not the diagnosis: check the offering the app actually requests, its packages, and the products attached to each package.
6. Catalog drift is not store truth. Null durations or stale price labels in the purchase SDK do not prove the store product lacks a period or a price. Read the store own product records.
7. Regional availability. Products can be live but enabled in one country only, which reads as "purchases are broken" everywhere else. Verify territories per product and per base plan, including future-region availability.
8. Store API migrations. A legacy product endpoint can fail with a migration notice such as "Please migrate to the new publishing API" while the catalog is healthy; use the current endpoint before concluding anything is broken. Converted prices can be rejected, in which case use the store stated maximum for that region.
9. Delivery confirmation. Grant the entitlement against the matching store transaction, and never show success before the balance or entitlement actually changes.
10. Web billing routing. A purchase SDK web billing is not a working web checkout. Confirm what the live checkout really uses, remove silent fallbacks to a different rail, and never describe automatic renewal on a page whose rail only takes one-time payments.
11. Test-versus-production evidence. Sandbox sheets, local StoreKit configurations, and a paywall showing one currency while the store sheet shows another are all test-environment behavior. Production proof requires the public store build installed from the store.
12. QA hygiene. Cancel unpaid test orders and verify balances are unchanged; they are neither revenue nor purchase evidence.
13. A claimed device or console blocker is often recoverable. Re-check device pairing and console access before declaring verification impossible.

## Hard rules

- No claim of "submitted" without a submission id and timestamp.
- No queue "refresh" by swapping builds.
- Never print secrets, cookies, or tokens; show variable names and proof that a value exists.
- Never overwrite a working release configuration to fix a symptom; fix the cause.
- One submission owner per app; background agents must not race each other into the queue.
- Distinguish what was verified this session from what is only recorded in a document.
- Configuration is not revenue: a healthy catalog, an active offering, and a rendering checkout page are not a completed purchase.
- Never claim a purchase flow works without a store transaction receipt that matches the granted entitlement.

## Required environment variable names

Use the existing global environment first, including ~/.env, instead of hardcoding values. Never commit or print secret values.

- ASC_KEY_ID, ASC_ISSUER_ID, ASC_KEY_PATH, ASC_APP_ID, ASC_BUNDLE_ID for App Store Connect API reads.
- PLAY_PACKAGE_NAME, PLAY_SERVICE_ACCOUNT_JSON for Google Play reads.
- REVENUECAT_PROJECT_ID, REVENUECAT_SECRET_KEY for purchase-SDK catalog reads (offerings, packages, products).
- Mailbox access for verdict and issue emails, through the agent's connected mail surface rather than browser automation.
