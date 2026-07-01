# scanner-config

Canonical DataWedge configuration for the Eastern Warehouse Distributors Zebra scanner fleet.

This repository is the single source of truth for scanner behavior. The file committed here
is deployed to the handhelds and wearables so every device shares one configuration.

## Files

| File | What it is |
|---|---|
| `datawedge.db` | Full DataWedge backup. Contains every profile at once. Restoring it replaces the entire DataWedge configuration on a device. |

Individual single-profile exports (`dwprofile_<name>.db`) are not stored here. The full
backup already contains every profile, so a standalone profile file is redundant.

## Current configuration

- DataWedge version: 13.0.325
- Last exported from: Zebra TC53E (Android 13)
- Profiles: 17 total. Sixteen are stock Zebra system profiles. One is custom:

### KCS (the warehouse profile)

`KCS` is the only EWD-authored profile. It is bound to the Kerridge / Autopart apps and is
what governs scanning inside the warehouse management system:

- `com.kerridgecs.epick`
- `com.kerridgecs.ereceipts`
- `com.kerridgecs.k8.estockcheck`
- `com.kerridgecs.k8.ewarehouse`

## Important behavior

Importing `datawedge.db` restores the **whole** DataWedge configuration on the target
device, not just the KCS profile. Any local per-device change is overwritten. This is
intentional for keeping the fleet standardized.

Device-level settings such as the physical Notification volume the beep rides on are **not**
stored in this file. They live on the device and must be set separately.

## How to update this file

1. Make the change on one "golden" scanner and confirm it behaves correctly.
2. On that scanner, open DataWedge, use the menu to back up / export the configuration.
   This produces a fresh `datawedge.db`.
3. Replace `datawedge.db` in a clone of this repo with the fresh export.
4. Commit with a message describing what changed and who requested it, then push.

Because `.db` is binary, `git diff` will not show readable changes. The commit message is
the audit trail, so keep it specific.

## Owners

- Data: James Cataldi (config author)
- IT: Anthony Garrett (repo, deployment)
