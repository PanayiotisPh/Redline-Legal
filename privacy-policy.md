# Privacy Policy

**Last updated: 18 August 2026**

This policy explains what Redline collects, why, and what control you have
over it. It is written to describe what the app actually does — if anything
here does not match the app's behaviour, treat that as a bug and tell us.

## The short version

- Location is recorded **only between the moment you tap "Start trip" and the
  moment you tap "Stop trip"**. It is never collected in the background at
  other times, and the app does not detect driving on its own.
- There are **no adverts and no advertising SDKs**.
- There are **no analytics or tracking SDKs**. We do not measure how you use
  the app.
- **We do not sell your data, and we do not share it with data brokers or
  advertisers.**
- Maps are served by OpenFreeMap, not Google or Mapbox, so your route is not
  handed to an advertising company in order to draw a map.
- You can export your trips as GPX files and delete your account, with all
  its data, from inside the app.

## What we collect

### Account information

When you create an account we store your **email address** (through our
authentication provider) and a **display name**, which is derived from the
part of your email before the `@` unless you change it. You may optionally
select an avatar from a set of built-in images.

### Trip data

While a trip is running, the app records:

- **Location**: GPS coordinates along your route, with timestamps
- **Speed**: derived from GPS
- **Motion**: accelerometer and gyroscope readings, used to calculate
  G-force, cornering and braking
- **Air pressure**: from the barometer, used to calculate elevation change
- **Derived statistics**: distance, duration, average and maximum speed, peak
  and sustained G-force, elevation gain, and per-kilometre splits
- **A label** for the trip, which you may edit

Sensor readings are thinned before upload — we store roughly one point per
second rather than every raw reading, because that is all the charts use.

### Vehicles

If you choose to add a vehicle to estimate power output, we store the name
and weight you enter, and optionally its drag figures. This is entirely
optional and the app works without it.

### Device information

We store a **randomly generated identifier for each installation** of the
app. This is not a hardware identifier: it is a random value created by the
app, it cannot be linked to you across other apps, it tells us nothing about
your device, and it is regenerated if you reinstall. We use it solely to
limit how many installations one account may be signed in on at once.

### What we do not collect

We do not collect your contacts, photos, calendar, microphone, camera,
advertising identifier, or your location outside of an active trip.

## Why we collect it, and our lawful basis

| What | Why | Lawful basis (GDPR Art. 6) |
|---|---|---|
| Email and display name | To create and secure your account | Contract |
| Trip and sensor data | To provide the app's core function — recording and analysing your drives | Contract |
| Group membership | To show leaderboards to people you have chosen to share a group with | Contract |
| Installation identifier | To limit concurrent installations per account | Legitimate interests — preventing account sharing |
| Subscription status | To provide paid features you have purchased | Contract |

## Who your data is shared with

Your data is stored on servers operated by **Supabase**, our database and
authentication provider, acting as our data processor. Data may be stored on
servers within the EU or elsewhere depending on the region of our project.

Map imagery is requested from **OpenFreeMap**, a free and open map tile
service. Requesting tiles necessarily reveals the approximate area you are
viewing to that service, in the same way that loading any map does. We chose
it specifically because it requires no API key or account and is not operated
by an advertising business.

When payments are enabled, subscription processing is handled by **Google
Play** and **RevenueCat**. We never see or store your payment card details.

Other people see your data only in these cases:

- **Group members** can see your display name, avatar, and your aggregate
  statistics — trip count, distance, top speed and peak G — on the
  leaderboard of a group you have both joined.
- **Nobody else.** There is no global leaderboard and no public profile.
  People who do not share a group with you cannot see your profile at all.
- **Stat cards you share** contain whatever the card shows, including a map
  of your route. Street and place names are deliberately removed from these
  maps, but the shape of the route is visible. You choose whether to share
  one and with whom.

## How long we keep it

Trips are kept until you delete them or delete your account. Free accounts
see their most recent 10 trips and paid accounts their most recent 100 —
this is a limit on what is *displayed*, not a deletion. Older trips are
retained, and become visible again if you subscribe.

Deleting your account removes your profile, trips, group memberships and
vehicles.

## Who we are

Redline is operated by **Panayiotis Photiadis**, a sole trader based in
Cyprus, acting as the data controller for the purposes of the EU General
Data Protection Regulation (GDPR).

Contact for any privacy question or request: **privacy@redlinegforce.com**

## Your rights

Under GDPR you have the right to access your data, correct it, delete it,
export it, restrict or object to processing, and to withdraw consent.

In practice:

- **Export**: any trip can be exported as a GPX file from its detail screen,
  and a full JSON export is available from your profile.
- **Delete**: individual trips can be deleted from the app, and your entire
  account can be deleted from Profile → Delete profile. This is immediate
  and cannot be undone.
- **Anything else**: email **privacy@redlinegforce.com** and we will respond within
  30 days.

If you believe we have handled your data improperly, you may complain to the
Office of the Commissioner for Personal Data Protection in Cyprus
(www.dataprotection.gov.cy), or to your local supervisory authority.

## Security

Access to your data is enforced at the database level by row-level security
policies, so one account cannot read another's trips even if the app were
modified. Connections are encrypted in transit.

No system is perfectly secure. Your session token is stored in the app's
private storage on your device; anyone with access to an unlocked phone has
access to the app.

## Children

Redline is not directed at children under 16 and we do not knowingly collect
their data.

## Changes

If this policy changes materially we will update the date above and note the
change in the app. Continuing to use Redline after a change means you accept
the updated policy.
