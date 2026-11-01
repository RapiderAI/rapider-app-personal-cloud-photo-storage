<!-- Rapider Official App SOW & Specification | App #179: Personal Cloud Photo Storage (personal-cloud-photo-storage) | Domain: PRS -->

# #179 Personal Cloud Photo Storage (`personal-cloud-photo-storage`) — Scope of Work (SOW)

- **App Number & Name:** `#179` — Personal Cloud Photo Storage
- **Proposed App Key:** `personal-cloud-photo-storage`
- **Suite / Domain Key & Model Prefix:** `PER` (`Prs*` — `PrsPhoto*` family)
- **Representative Product (`P1`):** Google Photos
- **Combined Feature Catalog:** [../combined-features/179-personal-cloud-photo-storage-features.md](../combined-features/179-personal-cloud-photo-storage-features.md)
- **Target Release Posture:** Owner-Isolated Personal Photo & Video Cloud Vault, Scrubbable Timeline, Non-Destructive Edits, Locked Folder, Memories, Opt-In Biometric Clustering & Family Partner Sharing Microapplication

---

## 1. Executive Outcome & Target Audience

### 1.1 Consumer & Privacy Problem Solved
Families, travelers, and everyday individuals accumulate tens of thousands of irreplaceable photos, videos, screenshots, and scanned documents across phones and camera cards—yet mainstream big-tech clouds monetize personal imagery or force fragile vendor lock-in, while corporate Digital Asset Management tools (`#058` `dam`) are built for public marketing SKUs and expose everything to workspace admins. **Rapider Personal Cloud Photo Storage (`personal-cloud-photo-storage`)** combines **Google Photos**-class speed, timeline scrubbing, Memories, and Classic + *Ask Photos* search with **Ente/Immich**-grade owner isolation (`ownerUserRef == authenticatedActor`) on top of encrypted Rapider Platform Files—ensuring workspace or employer administrators can NEVER view a user's personal photos.

### 1.2 Fast-Path Everyday Flow vs. Enthusiast & Privacy Power Controls
- **Fast-Path Everyday Flow (Families, Everyday Smartphone Users):** Drag-and-drop or auto-upload photos/videos with automatic SHA-256 duplicate skipping, scrub smoothly across Years/Months/Days, star Favorites, build shared family vacation albums with GPS stripped by default, and hide sensitive ID photos inside the **Locked Vault**.
- **Progressive Enthusiast & Privacy Depth (Photographers, Archivists, Privacy-First Households):** Batch-correct wrong camera-clock timezones on DSLR/scanned batches, curate wedding or travel shoots on a `<rapider-kanban>` **Curation Board**, apply non-destructive exposure/crop adjustments with 1-click *Revert to Original*, opt in (or out) of isolated **People & Pets** face clustering, configure filtered **Partner Sharing**, and merge perceptual near-duplicates to reclaim storage quota.

### 1.3 Measurable Outcomes
- **Zero Duplicate Byte Waste:** SHA-256 content-hash deduplication skips 100% of re-uploaded identical photos/videos before consuming storage quota.
- **100% Original Fidelity Preservation:** Every crop, rotate, or exposure adjustment writes a non-destructive recipe + rendition while keeping `originalFileRef` bit-for-bit intact.
- **Strict Biometric & Workplace Privacy:** `0` unauthorized reads by workspace/employer admins (`403 Forbidden`) and `0` face embeddings generated unless `biometricConsentGranted == true`.

### 1.4 Benchmark Summary (`P1`–`P6`)
Synthesizes the scrubbable timeline, Classic + *Ask Photos* search toggle, Memories suppression controls, Locked Folder, Partner Sharing, and storage cleanup of **`P1` Google Photos**, the burst/RAW+JPEG stacking, Duplicates merge, and shared family library UX of **`P2` Apple iCloud Photos**, the zero-surveillance privacy, GPS-stripped expiring share links, and Takeout portability of **`P3` Ente Photos**, the per-user isolated semantic/face search and EXIF world map of **`P4` Immich**, the batch EXIF date/timezone correction and Pick/Reject curation of **`P5` Mylio Photos**, and the step-up Personal Vault and OCR document recall of **`P6` Microsoft OneDrive Photos**.

---

## 2. In-Scope, Out-of-Scope & Suite Ownership Boundaries

### 2.1 In-Scope Capabilities
1. Owner-isolated personal photo/video catalog (`PrsPhotoAsset`) backed by encrypted, malware-scanned Rapider Platform Files (`originalFileRef`, `displayFileRef`, `thumbnailFileRef`) with SHA-256 exact upload deduplication.
2. Scrubbable chronological Year/Month/Day timeline, RAW+JPEG/burst stacking, Favorites, Archive, EXIF/XMP metadata inspector, batch camera-clock date/timezone shift, and non-destructive adjustment recipes (*Revert to Original*).
3. Custom pinned **Albums** (`PrsPhotoAlbum`, `PrsPhotoAlbumItem`), `<rapider-kanban>` **Photo Curation Board**, *On This Day* **Memories** with sensitive person/date suppression (`PrsPhotoMemoryRule`), and Places map clusters.
4. Step-up verified **Locked Vault** (`isLockedVault = true`, governed by `PrsPhotoVaultPolicy`) completely walled off from Timeline, Albums, Search, Memories, AI, and Partner Sharing.
5. Explicit opt-in **People & Pets** clustering (`PrsPhotoPersonCluster`, BIPA/GDPR Art. 9 compliant), OCR text-in-photo search, and toggleable Classic Keyword vs. owner-grounded *Ask Photos* search.
6. Scoped collaborative album sharing & filtered **Partner Sharing** (`PrsPhotoShareGrant`), perceptual duplicate merge & storage cleanup (`PrsPhotoCleanupBatch`), 30-day Trash recovery, Google Takeout import, and lossless archive export.

### 2.2 Out-of-Scope Capabilities (Delegated to Adjacent Rapider Apps)
- **Corporate brand guidelines, SKU product photography, talent release rights, and marketing CDN renditions:** Owned by `#058` Digital Asset Management (`dam`) and `ComProduct` / `MktCampaign`.
- **Multi-track timeline video production, color-grading LUT studios, and broadcast audio mixing:** Out of scope beyond lightweight trim and non-destructive photo adjustments.
- **Public social-network feeds, follower graphs, and ad-targeted creator monetization:** `#179` is a private personal & family vault; public social publishing belongs to `social` (`MktSocialPost`) or external platforms.

### 2.3 Suite Ownership & Integration Contract Matrix

| Domain / App | Owns | Integration Contract with `personal-cloud-photo-storage` |
| :--- | :--- | :--- |
| **`PER` (`personal-cloud-photo-storage`)** | `PrsPhotoAsset`, `PrsPhotoAlbum`, `PrsPhotoAlbumItem`, `PrsPhotoPersonCluster`, `PrsPhotoMemoryRule`, `PrsPhotoShareGrant`, `PrsPhotoCleanupBatch`, `PrsPhotoVaultPolicy` | Exclusive owner of personal photo/video metadata, non-destructive edit recipes, albums, opt-in face clusters, and sharing grants under `ownerUserRef`. |
| **Rapider Platform Files** | Encrypted object storage, malware scanning, short-lived signed URLs & thumbnail/video transcoding | Stores all binary assets (`originalFileRef`, `displayFileRef`, `thumbnailFileRef`); `#179` NEVER stores binary Base64 image blobs inside database documents. |
| **`PER` (`personal-budgeting-and-money-planning`, `#178`)** | `PrsFinTransaction` | Optional explicit single-asset owner handoff sending one receipt image `fileRef` from *Documents & Receipts* to `#178`. |
| **Platform Home `WorkItem`s** | Cross-app actionable user invites | Delivers collaborative album and Partner Sharing consent invitations (`PrsPhotoShareGrant`) to named recipients. |
| **Platform Channels & AI** | ACL-scoped `projectChannelMessage` & LLM/Vision gateway | Hosts shared-album family comment threads and executes owner-isolated OCR / *Ask Photos* / album-draft suggestions. |

---

## 3. Primary Web Pages & Sidebar Navigation Architecture (13 Pages)

Every visible sidebar section strictly enforces the **Minimum 3 Visible Items Rule (`>= 3`)** (`Gallery & Explore`: 3 items, `Collections & Curation`: 3 items, `Vault & Governance`: 3 items = 9 visible items; 4 detail/drawer views hidden from sidebar).

| # | Route | Page Title | Sidebar Section | Menu Visible | Key Components & UX |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `photos-timeline` | **Photos Timeline** | `Gallery & Explore` | `true` (Order 1) | Scrubbable Year/Month/Day gallery + table toggle (`findSWR`), drag-and-drop resumable uploader, *On This Day* Memories carousel (with quick *Hide Date/Person*), Favorite heart toggle, and `#activeFilterTagsBar` with `[Clear all]`. |
| 2 | `search-explore` | **Search, OCR & Places** | `Gallery & Explore` | `true` (Order 2) | Instant **Classic Search / Ask Photos** mode switcher, media-type pills (*Videos*, *Screenshots*, *Documents & Receipts*, *RAW*), OCR text match highlights, Places city/country map clusters, and `#activeFilterTagsBar`. |
| 3 | `people-and-pets` | **People, Pets & Memories** | `Gallery & Explore` | `true` (Order 3) | Explicit opt-in biometric consent banner (when disabled), private `PrsPhotoPersonCluster` grid (label, merge, split, hide), and `PrsPhotoMemoryRule` sensitive date/person mute list. |
| 4 | `albums-collections` | **Albums & Collections** | `Collections & Curation` | `true` (Order 1) | Pinned personal & collaborative `PrsPhotoAlbum` grid/table (`findSWR`), `#activeFilterTagsBar`, clickable Album Title -> `album-detail`, icon+text `Edit`, and icon-only `Delete` (removes album container without trashing underlying photos). |
| 5 | `curation-board` | **Photo Curation Board** | `Collections & Curation` | `true` (Order 2) | Interactive `<rapider-kanban>` shoot/event workflow (`UNREVIEWED` -> `SHORTLIST` -> `NEEDS_EDIT` -> `FINAL_ALBUM` -> `REJECT_CANDIDATE`) with batch promote-to-album and `#activeFilterTagsBar`. |
| 6 | `shared-with-family` | **Shared & Partner Library** | `Collections & Curation` | `true` (Order 3) | Active shared albums, expiring password-protected share links, and **Partner Sharing** rules (`shareFromDate`, selected `PrsPhotoPersonCluster` filter, GPS-strip status) with instant 1-click revoke. |
| 7 | `locked-vault` | **Locked Vault** | `Vault & Governance` | `true` (Order 1) | Step-up session verification gate unlocking `isLockedVault == true` sensitive photos/IDs, *Move Out of Vault*, and automatic session re-lock timer. |
| 8 | `storage-cleanup` | **Storage & Deduplication** | `Vault & Governance` | `true` (Order 2) | `Decimal128` GiB quota bar (Photos / Videos / RAW / Trash), side-by-side perceptual duplicate merge (`PrsPhotoCleanupBatch`), blurry/large-video/old-screenshot cleanup cards, and **30-Day Trash** restore/purge table. |
| 9 | `privacy-portability` | **Import, Export & Privacy** | `Vault & Governance` | `true` (Order 3) | Biometric consent & *Wipe All Face Embeddings* switch (`PrsPhotoVaultPolicy`), Google Takeout importer, lossless full-library export archive, and cryptographic cascade vault erasure. |
| 10 | `photo-detail` | **Photo Lightbox & Non-Destructive Editor** | *(Hidden)* | `false` | Full-res viewer (`#/photo-detail?id=...`) with EXIF/XMP/OCR inspector, batch/single capture-date & timezone editor, non-destructive Light/Color/Crop sliders + **Revert to Original**, single-receipt push to `#178`, and `Back to list` state restoration. |
| 11 | `album-detail` | **Album Story & Gallery** | *(Hidden)* | `false` | Single album workspace (`#/album-detail?id=...`) with cover banner, narrative story blocks, photo grid/list, `projectChannelMessage` family comments, *Share Album* trigger, and `#activeFilterTagsBar`. |
| 12 | `upload-import-drawer` | **Upload & Batch EXIF Drawer** | *(Hidden)* | `false` | Sliding drawer for multi-file upload, SHA-256 duplicate skip summary, optional target album assignment, and camera-clock timezone override; resets button in `finally` and calls `closePageDrawer()`. |
| 13 | `share-album-drawer` | **Share Album / Partner Rule Drawer** | *(Hidden)* | `false` | Sliding drawer configuring named collaborator grants (with `<rapider-phone-input>` E.164 SMS/contact lookup or email), password/expiry link settings, and `stripExifGps = true` default; calls `closePageDrawer()`. |

---

## 4. Canonical Data Models (`PrsPhoto*` — 8 Models)

All models omit `projectId`, enforce server-resolved `ownerUserRef` isolation, use platform UUIDs (`00000000-xxxx-xxxx-xxxx-xxxxxxxxxxxx` in deterministic seeds), store binary files strictly in Rapider Platform Files, and track optimistic `version`.

| Model Name | Key Fields & Types | Relationships & External Refs | Invariants & Lifecycle |
| :--- | :--- | :--- | :--- |
| **`PrsPhotoAsset`** | `id` (UUID), `ownerUserRef` (UUID), `originalFileRef` (Platform File UUID), `displayFileRef` (Platform File UUID), `thumbnailFileRef` (Platform File UUID), `sha256Hash` (String), `perceptualHash` (String?), `mediaType` (`PHOTO` \| `VIDEO` \| `LIVE_PHOTO` \| `RAW_PHOTO`), `contentCategory` (`STANDARD` \| `SCREENSHOT` \| `DOCUMENT_RECEIPT`), `mimeType` (String), `sizeBytes` (Long), `widthPx` (Int), `heightPx` (Int), `durationSeconds` (`Decimal128`?), `capturedAt` (DateTime), `timezoneId` (IANA String), `cameraMakeModel` (String?), `gpsLat` (`Decimal128`?), `gpsLng` (`Decimal128`?), `placeLabel` (String?), `ocrText` (String?), `description` (String?), `editRecipeJson` (Object?), `curationStage` (`UNREVIEWED` \| `SHORTLIST` \| `NEEDS_EDIT` \| `FINAL_ALBUM` \| `REJECT_CANDIDATE`), `isFavorite` (Boolean), `isArchived` (Boolean), `isLockedVault` (Boolean), `trashedAt` (DateTime?), `purgeAfterAt` (DateTime?), `version` (Int) | Stored in Platform Files; linked via `PrsPhotoAlbumItem` and `PrsPhotoPersonCluster` | Unique `(ownerUserRef, sha256Hash)` prevents duplicate uploads; `originalFileRef` is immutable so *Revert to Original* always succeeds; if `isLockedVault == true` or `trashedAt != null`, excluded from Timeline, Albums, Search, Memories, AI, and Partner Sharing. |
| **`PrsPhotoAlbum`** | `id` (UUID), `ownerUserRef` (UUID), `title` (String), `storyDescription` (String?), `coverAssetId` (UUID?), `isPinned` (Boolean), `itemCount` (Int), `earliestCapturedAt` (DateTime?), `latestCapturedAt` (DateTime?), `channelThreadRef` (UUID?), `version` (Int) | Has many `PrsPhotoAlbumItem`, `PrsPhotoShareGrant`; `channelThreadRef` -> `projectChannelMessage` | Deleting a `PrsPhotoAlbum` deletes container & `PrsPhotoAlbumItem` join rows ONLY—never moves underlying `PrsPhotoAsset` photos to Trash. |
| **`PrsPhotoAlbumItem`** | `id` (UUID), `ownerUserRef` (UUID), `albumId` (UUID), `assetId` (UUID), `addedByUserRef` (UUID), `captionNote` (String?), `sortOrder` (Int), `version` (Int) | Joins `PrsPhotoAlbum` and `PrsPhotoAsset` | Unique `(albumId, assetId)`; moving an asset into Locked Vault or Trash automatically detaches its active album visibility. |
| **`PrsPhotoPersonCluster`** | `id` (UUID), `ownerUserRef` (UUID), `clusterType` (`PERSON` \| `PET`), `displayName` (String?), `coverAssetId` (UUID?), `assetCount` (Int), `isHiddenFromMemories` (Boolean), `version` (Int) | Populated ONLY when `PrsPhotoVaultPolicy.biometricConsentGranted == true` | Strictly scoped to `ownerUserRef`; revoking biometric consent immediately purges all `PrsPhotoPersonCluster` records and vector embeddings. |
| **`PrsPhotoMemoryRule`** | `id` (UUID), `ownerUserRef` (UUID), `ruleType` (`MUTE_DATE_RANGE` \| `MUTE_PERSON_CLUSTER`), `startDate` (LocalDate?), `endDate` (LocalDate?), `personClusterId` (UUID?), `reasonNote` (String?), `version` (Int) | Governs *On This Day* & *Memories* generation | Deterministically suppresses matching assets from appearing in any nostalgic Memory carousel. |
| **`PrsPhotoShareGrant`** | `id` (UUID), `ownerUserRef` (UUID), `grantMode` (`ALBUM_COLLABORATOR` \| `ALBUM_EXPIRING_LINK` \| `PARTNER_AUTO_SHARE`), `albumId` (UUID?), `granteeUserRef` (UUID?), `granteePhoneE164` (String?), `accessRole` (`VIEWER` \| `CONTRIBUTOR`), `shareFromDate` (LocalDate?), `filterPersonClusterIds` (UUID[]), `stripExifGps` (Boolean, default `true`), `passwordHash` (String?), `expiresAt` (DateTime?), `grantStatus` (`PENDING` \| `ACTIVE` \| `REVOKED` \| `EXPIRED`), `workItemRef` (UUID?), `version` (Int) | Scopes `PrsPhotoAlbum` or filtered `PARTNER_AUTO_SHARE` stream; invites via Home `WorkItem` | Never exposes `isLockedVault == true` or `trashedAt != null`; `stripExifGps` defaults to `true` so viewers cannot extract raw home GPS coordinates unless explicitly overridden; revocation takes effect immediately (`403`). |
| **`PrsPhotoCleanupBatch`** | `id` (UUID), `ownerUserRef` (UUID), `cleanupCategory` (`EXACT_OR_NEAR_DUPLICATE` \| `BLURRY_SHOT` \| `LARGE_VIDEO` \| `OLD_SCREENSHOT`), `candidateAssetIds` (UUID[]), `recommendedKeepAssetId` (UUID?), `reclaimableBytes` (Long), `resolutionStatus` (`OPEN` \| `MERGED` \| `DISMISSED`), `version` (Int) | References `PrsPhotoAsset`s | Merging near-duplicates transfers all `PrsPhotoAlbumItem` memberships and `isFavorite` to `recommendedKeepAssetId` and moves redundant copies to 30-day Trash (never silent permanent deletion). |
| **`PrsPhotoVaultPolicy`** | `id` (UUID), `ownerUserRef` (UUID), `quotaLimitGiB` (`Decimal128`), `usedBytes` (Long), `biometricConsentGranted` (Boolean, default `false`), `biometricConsentedAt` (DateTime?), `askPhotosAiEnabled` (Boolean, default `true`), `lockedVaultTimeoutMinutes` (Int, default `5`), `defaultStripGpsOnShare` (Boolean, default `true`), `version` (Int) | 1:1 per `ownerUserRef` | Flipping `biometricConsentGranted` from `true` to `false` triggers synchronous cluster lockout + async embedding wipe workflow. |

---

## 5. Core End-to-End User Journeys

### 5.1 Deduplicated Upload, Batch Camera-Clock Fix & Non-Destructive Edit (`photos-timeline` -> `photo-detail` -> `photos-timeline`)
1. Using `upload-import-drawer` on `#/photos-timeline`, the user drops 25 vacation photos (3 of which were already uploaded last week). SHA-256 checks skip the 3 duplicates idempotently (`0` extra bytes billed), store the 22 new originals in Platform Files, and auto-close the drawer via `closePageDrawer()`.
2. Filtering by `Camera: "Fuji X100V"` shows `#activeFilterTagsBar` (`Camera: "Fuji X100V" ✕ [Clear all]`). Noticing the camera clock was 6 hours behind in Tokyo, the user applies a `+6h Asia/Tokyo` timezone shift—reordering the timeline non-destructively.
3. Clicking a sunset photo opens `#/photo-detail?id=...` (verified free of `<base href>` 404 diversion). The user crops `16:9` and boosts warmth (`editRecipeJson` saved; `originalFileRef` untouched), then clicks **Revert to Original** to verify lossless restoration, and clicks `Back to list` to return to `#/photos-timeline` with the Fuji filter preserved.

### 5.2 Shoot Curation via `<rapider-kanban>` & GPS-Stripped Family Album Sharing (`curation-board` -> `albums-collections` -> `share-album-drawer`)
1. On `#/curation-board`, the user drags 18 birthday photos from `UNREVIEWED` into `SHORTLIST` and `FINAL_ALBUM` on `<rapider-kanban>`, then clicks **Publish Shortlist to Album** (*Maya's 5th Birthday*).
2. Opening `#/album-detail?id=...`, the user clicks **Share Album**, enters grandparents' contacts in `share-album-drawer` (with `<rapider-phone-input>` E.164 lookup), leaves `stripExifGps = true` enabled, and saves—dispatching Rapider Home `WorkItem` invites and stripping home GPS coordinates from shared metadata responses.
3. Deleting the album container later on `#/albums-collections` (confirmed via `#rapiderConfirmOkBtn`) removes *Maya's 5th Birthday* from albums while keeping all 18 photos safely in `photos-timeline`.

### 5.3 Locked Vault Isolation & Filtered Partner Sharing (`locked-vault` & `shared-with-family`)
1. The user enables **Partner Sharing** with their spouse (`shareFromDate = 2025-01-01`, filtered to consented *Kids* `PrsPhotoPersonCluster`).
2. The user then photographs a passport renewal document and clicks **Move to Locked Vault**. Step-up verification unlocks `#/locked-vault` for `5 minutes`; simultaneously, the passport image disappears from `photos-timeline`, `search-explore`, OCR/Ask Photos results, and the spouse's Partner Sharing feed (`403 Forbidden`).

### 5.4 Consent-First Biometrics & Perceptual Duplicate Merge (`people-and-pets` & `storage-cleanup`)
1. Visiting `#/people-and-pets` for the first time shows `biometricConsentGranted = false` (`0` face clusters). Opting in indexes faces strictly within the user's private namespace; toggling consent back off immediately wipes all `PrsPhotoPersonCluster` records and embeddings.
2. On `#/storage-cleanup`, the user reviews a `PrsPhotoCleanupBatch` of 4 near-identical burst shots (`28.4 MB` reclaimable), clicks **Merge & Keep Sharpest**, retaining the `48 MP` original + album links and moving the 3 extras to 30-day Trash.

### 5.5 Classic vs. *Ask Photos* Search & Single-Receipt Handoff (`search-explore` -> `photo-detail` -> `#178`)
1. On `#/search-explore`, the user toggles **Ask Photos** and enters *"Hardware store receipt from October"*, surfacing cited `DOCUMENT_RECEIPT` thumbnails via private OCR + vision grounding.
2. Opening `#/photo-detail?id=...`, the user clicks **Send Single Receipt to Personal Budgeting (`#178`)**, passing just that one `originalFileRef` to draft a personal transaction in `#178`.

---

## 6. Consolidated Feature Priorities & Work Packages (`P0`, `P1`, `P2`)

### `P0` Foundation Work Packages
- **WP-P0-1 — Owner-Isolated Platform File Vault, SHA-256 Dedupe & Timeline (`PCP-001`–`PCP-005`, `PCP-019`):** Implement `ownerUserRef` server guard (`403` to workspace/`#058` admins), `PrsPhotoAsset` + Platform Files integration, SHA-256 upload dedupe, EXIF extraction + batch timezone correction, 30-day Trash (`trashedAt`), Classic faceted search with `#activeFilterTagsBar`, and `photos-timeline` / `photo-detail` / `upload-import-drawer`.
- **WP-P0-2 — Albums, `<rapider-kanban>` Curation & Non-Destructive Editing (`PCP-007`–`PCP-009`):** Deliver `PrsPhotoAlbum`, `PrsPhotoAlbumItem` (container-delete safety), `<rapider-kanban>` `curation-board`, non-destructive `editRecipeJson` + *Revert to Original*, and `albums-collections` / `album-detail`.
- **WP-P0-3 — Step-Up Locked Vault & Lossless Quota/Portability (`PCP-011`, `PCP-014`):** Deliver step-up gated `locked-vault` (`PrsPhotoVaultPolicy`), walling off `isLockedVault` from all other surfaces, `Decimal128` GiB quota bar, lossless archive export, and cascade erasure (`privacy-portability`).

### `P1` Family Sharing, Memories & Hygiene Work Packages
- **WP-P1-1 — Scoped Album Sharing, Partner Sharing & Places GPS Privacy (`PCP-010`, `PCP-015`, `PCP-016`):** Build `PrsPhotoShareGrant` (collaborator Home `WorkItem`s, expiring password links, date/person-filtered Partner Sharing, `stripExifGps = true` default), `shared-with-family`, `share-album-drawer`, and Places map clusters.
- **WP-P1-2 — Opt-In People/Pets, Memories Suppression, OCR & Storage Cleanup (`PCP-006`, `PCP-012`, `PCP-013`, `PCP-017`, `PCP-020`):** Build BIPA-compliant opt-in `PrsPhotoPersonCluster` + instant embedding wipe (`people-and-pets`), `PrsPhotoMemoryRule`, private OCR indexing, Google Takeout importer, and `PrsPhotoCleanupBatch` duplicate merge (`storage-cleanup`).

### `P2` Adjacent & Grounded AI Work Packages
- **WP-P2-1 — Toggleable *Ask Photos*, Trip Drafts, Alt-Text & `#178` Receipt Link (`PCP-018`, `PCP-021`–`PCP-024`):** Deliver owner-grounded *Ask Photos* search toggle (`search-explore`), review-before-create AI trip album proposals, accessibility alt-text captioning, screenshot archive nudges, and single-receipt `#178` handoff.

---

## 7. Roles, Permissions & Field-Level Governance

| Role | Permitted Actions | Restricted / Field-Level Boundaries |
| :--- | :--- | :--- |
| **Personal Vault Owner** | Full CRUD on own `PrsPhoto*` assets, Locked Vault (with step-up auth), albums, non-destructive edits, biometric consent toggle, `PrsPhotoShareGrant` issuance/revocation, Takeout imports, lossless exports, and vault erasure. | Cannot bypass storage quota ceiling (`quotaLimitGiB`) without cleaning Trash/duplicates or upgrading personal plan. |
| **Partner / Shared Album Contributor (`CONTRIBUTOR`)** | View shared album/partner stream items, comment in `projectChannelMessage` thread, and contribute own photos into a collaborative `PrsPhotoAlbum` while `grantStatus == ACTIVE`. | **Zero access (`403`)** to the owner's `Locked Vault`, unshared albums, `Archived` or `Trashed` photos, or raw GPS coordinates when `stripExifGps == true`; cannot permanently delete the owner's `PrsPhotoAsset`s. |
| **Shared Album / Link Viewer (`VIEWER`)** | Read-only view/download (if allowed) of assets inside the single shared `PrsPhotoAlbum` before `expiresAt` while `grantStatus == ACTIVE`. | Zero upload, edit, comment, or library browse access; raw EXIF GPS stripped when `stripExifGps == true`. |
| **Workspace / Employer / `#058` DAM Admin** | Configure workspace app catalog availability and monitor aggregate anonymous storage infrastructure health. | **STRICTLY BLOCKED (`403 Forbidden`)** from viewing, listing, searching, running ML across, or exporting any user's `PrsPhoto*` records, thumbnails, or Platform File URLs (`PCP-001`). |

---

## 8. Technical APIs, Guarded Commands, Events & `requiredWorkflows`

### 8.1 Guarded Domain Commands
Every command resolves `ownerUserRef` from authenticated JWT claims and requires `idempotencyKey`, `correlationId`, and `expectedVersion`:
- `per.photo.completeAssetIngest` — Verifies Platform File malware scan, checks `(ownerUserRef, sha256Hash)` idempotent deduplication, extracts EXIF/dimensions, increments `usedBytes`, and creates `PrsPhotoAsset`.
- `per.photo.applyOrRevertNonDestructiveEdit` — Persists `editRecipeJson` and rendered `displayFileRef` or restores `displayFileRef = originalFileRef` without touching the original binary.
- `per.photo.toggleLockedVault` — Requires step-up verification token; flips `isLockedVault`, detaches active shared album memberships, and purges search/AI embeddings for locked items.
- `per.photo.setBiometricClusteringConsent` — Records explicit opt-in consent or immediately locks `PrsPhotoPersonCluster` reads and triggers complete face-embedding erasure.
- `per.photo.mergeDuplicateBatch` — Transfers album links + favorite status to `recommendedKeepAssetId` and moves redundant duplicates to 30-day Trash (`trashedAt`).
- `per.photo.grantOrRevokeShare` — Issues `PrsPhotoShareGrant` (and recipient Home `WorkItem`) or immediately revokes collaborator/link/partner access (`403`).

### 8.2 Domain Events Published & Consumed (Kafka)
- **Published:** `per.photo.asset.ingested.v1`, `per.photo.asset.trashed.v1`, `per.photo.biometric.consentChanged.v1`, `per.photo.share.changed.v1`, `per.photo.vault.erased.v1` (opaque IDs and byte deltas only; zero filenames, GPS, or thumbnails).
- **Consumed:** `platform.file.malware_scan_completed.v1` (gates thumbnail/EXIF/OCR pipeline), `platform.auth.step_up_verified.v1`.

### 8.3 `requiredWorkflows` (Declared in `app.manifest.json`)
1. `personalPhotoIngestThumbnailAndOcrWorkflow` — Generates responsive WebP thumbnails, video poster frames, EXIF/XMP extraction, and private OCR indexing (skipping `isLockedVault`).
2. `personalPhotoOptInBiometricClusteringWorkflow` — Runs strictly when `biometricConsentGranted == true` within the owner's isolated vector partition; wipes all embeddings within seconds upon consent revocation.
3. `personalPhotoTrashExpirationAndTakeoutWorkflow` — Purges `trashedAt` assets past 30 days to reclaim Platform File bytes, imports Takeout ZIPs, and builds lossless export archives.
4. `personalPhotoVaultErasureWorkflow` — Revokes all share grants, wipes private indexes/embeddings, deletes Platform File blobs, and purges `PrsPhoto*` metadata.

### 8.4 Grounded Rapider AI Rules
- *Ask Photos* (`PCP-021`), Trip Album Drafts (`PCP-022`), and Alt-Text (`PCP-023`) operate exclusively over non-locked, non-trashed `PrsPhotoAsset`s owned by `authenticatedActor`, citing exact asset thumbnails in responses.
- User photos, faces, and OCR text are **never** pooled across tenants, never used for model training, and AI never deletes photos, merges duplicates, or shares albums without explicit human confirmation.

---

## 9. Security, Privacy, Compliance & Non-Functional Requirements

- **Biometric & Location Privacy (Illinois BIPA, GDPR Art. 9 & Art. 17, CCPA):** Facial geometry clustering is strictly opt-in (`biometricConsentGranted = false` default) with instant zero-residue erasure on opt-out. Shared albums strip EXIF `gpsLat`/`gpsLng` (`defaultStripGpsOnShare = true`) unless explicitly disabled by the owner.
- **Short-Lived Signed Media URLs & Zero Workplace Surveillance:** Thumbnails and originals are served via short-lived, owner/grant-authorized Platform File URLs—never public unauthenticated CDN buckets. Whether a consumer uses a `\$0.00` starter quota or a `\$9.99`/month `2 TB` personal storage tier, workspace/employer admins receive `403 Forbidden` on all `PrsPhoto*` endpoints.
- **Accessibility & Responsive Gallery UX:** Full WCAG 2.2 AA compliance (keyboard Lightbox arrow navigation, alt-text support, ARIA labels on icon-only Delete buttons, `#ffffff !important` contrast on primary/danger buttons in light and dark modes) across `390px` mobile, `768px` tablet, and `1440px` desktop viewports.

---

## 10. Deterministic Acceptance Criteria

1. **Owner-Only & DAM Isolation (`403 Forbidden`):** Authenticated Workspace Administrators and `#058` Corporate DAM Managers querying `PrsPhotoAsset`, `PrsPhotoAlbum`, or signed Platform File URLs belonging to another user deterministically receive `403 Forbidden` / `0` records.
2. **Sidebar 3-Item Section Rule:** `app.manifest.json` defines 3 visible sidebar sections (`Gallery & Explore`, `Collections & Curation`, `Vault & Governance`), each containing 3 visible menu items (`displayOnMenu: true`), while all 4 detail/drawer pages set `displayOnMenu: false`.
3. **SWR & Clean Query Compliance:** Initial reads across all pages use `findSWR` / `countSWR` without redundant `isDeleted` API query filters and defensively filter `item.isDeleted !== true` in memory.
4. **SHA-256 Upload Deduplication:** Uploading a byte-identical file twice returns the existing `PrsPhotoAsset` idempotently and adds `0` duplicate bytes to `PrsPhotoVaultPolicy.usedBytes`.
5. **Non-Destructive Edit & Lossless Revert:** Saving crop/exposure changes updates `editRecipeJson` and `displayFileRef` while leaving `originalFileRef` unchanged; clicking **Revert to Original** restores exact original dimensions and SHA-256 hash.
6. **Album Container Delete vs. 30-Day Photo Trash:** Deleting a `PrsPhotoAlbum` removes the album container while leaving its `PrsPhotoAsset` photos intact in `photos-timeline`; moving a photo to Trash sets `trashedAt` and hides it from Timeline/Albums/Shares while allowing 1-click Restore within 30 days.
7. **Locked Vault Wall-Off:** Moving an asset to `locked-vault` (`isLockedVault = true`) immediately excludes it from `photos-timeline`, `albums-collections`, `search-explore` (*Classic* & *Ask Photos*), `Memories`, and `Partner Sharing`, and requires step-up verification to view.
8. **BIPA Opt-In Biometric Guard:** With `biometricConsentGranted == false` (default), `PrsPhotoPersonCluster` count remains `0`; revoking consent after opting in immediately purges all clusters and face embeddings.
9. **Default EXIF GPS Stripping & Instant Share Revocation:** Creating an album share via `share-album-drawer` defaults `stripExifGps = true` (omitting `gpsLat`/`gpsLng` for viewers) and auto-closes via `closePageDrawer()`; revoking `PrsPhotoShareGrant` immediately blocks viewer access (`403`).
10. **Active Filter Tags, Table Standard & Custom Elements:** Filtering `photos-timeline`, `search-explore`, `albums-collections`, or `curation-board` renders HTML-escaped chips in `#activeFilterTagsBar` with `[Clear all]`; tables have zero standalone "View" buttons, icon+text `Edit`, icon-only `Delete` (`#rapiderConfirmOkBtn`, zero native `alert`/`confirm`/`prompt`), `curation-board` uses `<rapider-kanban>`, and `share-album-drawer` uses `<rapider-phone-input>`.
11. **Click-Through Navigation & Base-URL Guard:** Clicking a photo or album opens `#/photo-detail?id=...` or `#/album-detail?id=...` without `<base href>` 404 diversion, and `Back to list` restores prior `#activeFilterTagsBar` chips.
12. **No `projectId` Leakage & Theme Contrast:** Zero `PrsPhoto*` schemas or API payloads include `projectId`, and all 13 pages pass `validate_theme` across light and dark modes with `#ffffff !important` primary/danger button text.
