# Payments setup

What is built, and what still needs doing in the Play and RevenueCat consoles.

## Done in code

- **Upload keystore** — `android/app/upload.keystore`, 4096-bit RSA, valid to
  2054. Credentials in `keystore.properties`. Both gitignored.
- **Release signing** — `plugins/withReleaseSigning.js` reapplies it on every
  `expo prebuild`, since `android/` is generated. Falls back to debug signing
  if the keystore is absent, so a checkout without the key still builds.
- **`versionCode`** — set in `app.json`. **Increment for every upload**; Play
  rejects a duplicate.
- **AAB** — `npm run android:release` builds an APK for the device;
  `cd android && ./gradlew bundleRelease` builds the AAB Play needs.
- **Subscription state** — `premium_until` (migration 0030). Expiry is
  self-executing; a missed webhook cannot leave someone premium forever.
- **Webhook** — `supabase/functions/revenuecat-webhook/`.
- **Client shim** — `lib/purchases.ts`. Restore is wired into the Premium
  screen; the SDK itself is not installed yet.

## THE KEYSTORE IS IRREPLACEABLE

Play identifies the app by this key.

- **Lose it** → you can never ship an update. Not recoverable by support.
- **Leak it** → someone else can publish updates as you.

Back up **both** `android/app/upload.keystore` and `keystore.properties`
somewhere durable and private — a password manager, or encrypted offline
storage. Not in git; not in the repo folder alone.

## Remaining steps

### 1. Play Console

1. Create the app. Package `com.panayiotisph.tripper` — **permanent**.
2. Upload the AAB to **internal testing**.
3. Complete the Data safety form. Location is collected; be accurate.
4. Add a **privacy policy URL** — mandatory for location permissions.
5. Create the subscription product, e.g. `premium_monthly` at $4/month.
   Consider an annual tier alongside it.
6. Add **license testers** so purchases can be tested without being charged.

### 2. Google Cloud

7. Create a service account with Play Developer API access, download the JSON.
8. Grant it access in Play Console under Users and permissions.

### 3. RevenueCat

9. Add the Play app, upload the service-account JSON.
10. Map the Play product to an entitlement named `premium`.
11. Point the webhook at the deployed function URL, and set an
    `Authorization` header to a secret of your choosing.

### 4. Back in code

12. `npx expo install react-native-purchases`, then prebuild.
13. Fill the `TODO(payments)` blocks in `lib/purchases.ts` and flip
    `purchasesAvailable()` to true.
14. Call `identifyForPurchases(userId)` on sign-in — RevenueCat keys
    entitlements by `app_user_id`, and the webhook uses it to find the row.
    Wrong here means a purchase grants premium to the wrong account.
15. Deploy the function and set its secrets:

```
supabase secrets set REVENUECAT_WEBHOOK_SECRET=<the same value as the header>
supabase functions deploy revenuecat-webhook
```

## Testing

Play Billing only works for builds **installed through Play**. The usual
`adb install` loop cannot test purchases — use the internal testing track.
