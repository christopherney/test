# IDCHECK.IO
## Good practices workflow
### Capture ID with SDKMobile API
### Analysis with CIS API

**v1 – Dec 2025**

---

## Server Architecture

```
IDCheck - SDKMobile
├── CIS API
├── CIS Folder
├── BackOffice CIS
├── CIS Document
└── Images Analysis
    └── report
```

### Authentication Flows

**Integrator Backend:**
- Username / password authentication
- OpenID Connect authentication (access_token)

**IDnow Gateway to CIS:**
- Token & appID / bundleID configuration

---

## IDCheck - SDKMobile - Capture ID

### Process Flow

1. **Mobile App Initialization**
   - `idcheckio.activate()`
   - `idcheckioview.startOnboarding(cisFileUid, colors)`

2. **ID Capture**
   - Capture front & back of ID document
   - Event: `onSuccess`
   - Retry capture if quality issue or mismatch between front and back

3. **Document Analysis**
   - Attach ID document in `cisFileUid`
   - Launch document analysis in asynchronous mode
   - **Average analysis time: 6 seconds**
   - **Notification:** `FILE_UPDATE_CHECK`
   - Response includes: control tree + extracted data + `cisImageUid`

4. **File Analysis**
   - `GET /file/{cisFileUid}`
   - **Average analysis time: 10 seconds**
   - Response includes: control tree + extracted data + `cisDocUid`

---

## IDCheck - SDKMobile - Authentication + CIS Configuration

### OpenID Connect Authentication

```
POST /openid-connect/token
{
  grant_type: client_credentials,
  client_id,
  client_secret
}
→ { access_token }
```

**Note:** `access_token` must be used on each request

### Create Notification Endpoint

```
POST /rest/v1/realm/{realm}/endpoint
{
  url: your-public-url,
  client_id: a-unique-id
}
```

### Subscribe to Notification Event

```
POST /rest/v1/realm/{realm}/endpoint/{client_id}/event
{
  eventKind: FILE,
  method: UPDATE,
  operation: CHECK
}
```

→ Triggered at end of folder analysis

### Notifications Retry Policy

- If no HTTP 200 received: resend notification up to 3 times
- If still unsuccessful: send email to registered `supportEmail`

---

## Sandbox Environment

### SDKMobile Test Account

**SDKMobile Key:**
```
4ISdBd6Ha1x/oyw6Cbss7KoUytkXT8br34BheZpHdewwrJa9gYFey2aDAlikjMS8BNfhd/Qr/erPRnzAqH0b0tCWnsvX3B7swCQ3Zp7rdxXjoiq56ZVnVkdRuKuf7XKaQtHt6bBFYn+WmTihhoLrZPuFq6K1HZo/h6MBapZ/XReGr5YExzuS8QUBJQDirpFK0kzK
```

**SDKMobile Git Access:** https://git-externe.rennes.ariadnext.com/users/sign_in

| Field | Value |
|-------|-------|
| Username | skiptax |
| Password | NyH6w \`} -=.k$}oE4T |

**Note:** Provide appID and bundleID of your applications for authorization

---

## IDCheck – SDKMobile – CIS API

### BackOffice CIS Test Account

| Field | Value |
|-------|-------|
| Login | skiptax@idnow.io |
| Password | 15hr6Eh_8C72RU63cftK |
| Realm | skiptax |
| CIS BackOffice URL | https://customer-identity.idcheck-sandbox.ariadnext.io/*** |

### CIS API Technical Test Account

| Field | Value |
|-------|-------|
| client_id | sa-skiptax-sandbox |
| client_secret | muGjxggPsQHE1F0n3a3lH3vZ4LamMROH |

---

## CIS Documentation & Base Sandbox URLs

### CIS Documentation

- **Developer guide CIS API:** https://cloud.rennes.ariadnext.com/index.php/s/KTso6QqHgpzCZBT
- **Reference guide CIS API** (all objects and requests): https://api.idcheck-sandbox.ariadnext.io/gw/cis/api/index.html

### Base URLs - Sandbox

| Service | URL |
|---------|-----|
| Authentication URL | https://api.idcheck-sandbox.ariadnext.io/auth/realms/customer-identity/protocol/openid-connect/token |
| CIS API URL | https://api.idcheck-sandbox.ariadnext.io/gw/cis |

### Postman Collection

https://cloud.rennes.ariadnext.com/index.php/s/3b5F2CGr97WSjXK

### Response Examples

| Resource | Link |
|----------|------|
| GET /file | https://cloud.rennes.ariadnext.com/index.php/s/be6t4Hse2NccR6x |
| GET /document (ID) | https://cloud.rennes.ariadnext.com/index.php/s/NBFRfBizbdn9nkJ |
| GET /document (biometric liveness) | https://cloud.rennes.ariadnext.com/index.php/s/SSnwTTcmAzd5Qq8 |
| FILE_UPDATE_CHECK Notification | https://cloud.rennes.ariadnext.com/index.php/s/Kt5pk8EpCXq67Et |

**File password:** IDNow_2023

---

## SDKMobile Documentation

### Code Examples from Github

| Platform | Repository |
|----------|------------|
| Android | https://github.com/ariadnext/IDCHECK.IO_SDK-example-Android |
| iOS | https://github.com/ariadnext/IDCHECK.IO_SDK-example-iOS |

### Hybrid Plugins

| Framework | Link |
|-----------|------|
| Flutter | https://pub.dev/packages/idcheckio |
| React | https://www.npmjs.com/package/@idnow/react-idcheckio |

### Developer Guides

| Platform | Link |
|----------|------|
| iOS | https://cloud.rennes.ariadnext.com/index.php/s/QB2YioFF8scxj35 |
| Android | https://cloud.rennes.ariadnext.com/index.php/s/mzJyAzzEoFSPaZk |

**File password:** IDnow_IDCh3ck