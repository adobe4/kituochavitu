# Kuni Box — downloads

Built APKs only. The source lives in a private repository; nothing here is code.

This exists because app downloads were being served from Supabase Storage, whose
free plan allows 5 GB of cached egress a month. At roughly 25 MB an APK and
around 200 installs, a single release costs about 5 GB — twenty-one releases in
one week ran the organisation to 18.5 GB and put every project, including
unrelated ones, three days away from returning 402 to its users. GitHub serves
these files without billing bandwidth, so downloads no longer cost anything.

## What is here

| File | App | Version |
| --- | --- | --- |
| `apk/kuni-box-1.3.9.apk` | Kuni Box (phones) | 1.3.9 |
| `apk/kuni-box-tv-1.0.19.apk` | Kuni Box TV | 1.0.19 |
| `apk/kuni-box-admin-1.1.9.apk` | Kuni Box Admin | 1.1.9 |

Download URLs take the form:

```
https://raw.githubusercontent.com/adobe4/kituochavitu/main/apk/<file>.apk
```

Those are the URLs stored in `app_versions.apk_url`, which is what the in-app
updater reads.

## Publishing a release

1. Build the APK and give it a filename carrying its version — never overwrite
   an existing one. A phone part-way through a download would otherwise receive
   half of one build and half of another.
2. Replace the previous APK for that app rather than adding to it, then commit
   with `--amend` onto the single existing commit and force-push.

   The history is deliberately one commit deep. Git keeps every binary it has
   ever seen, so a new 30 MB APK per release would add 30 MB to the repository
   permanently and reach GitHub's size warnings within a dozen releases. Only
   the current builds need to exist.
3. Point `app_versions` at the new URL and mark the row active. The apps read
   that table, not this repository.

## A note on the signing key

Every build here is signed with the same key, so they install over each other
as updates. That key lives in the private repository and must stay there: a
public repository holding it would let anyone publish an app that Android
accepts as an update to this one.
