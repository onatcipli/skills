# App Store Connect Screenshot Upload API

Complete reference for uploading screenshots via the App Store Connect API.

---

## Authentication (JWT)

All ASC API requests require a JWT token signed with your API key.

### Prerequisites

- API Key (.p8 file) — download from App Store Connect → Users and Access → Keys
- Key ID (10-character string)
- Issuer ID (UUID format)

### Generate JWT Token

```javascript
const jwt = require('jsonwebtoken');
const fs = require('fs');

function generateToken(keyPath, keyId, issuerId) {
    const privateKey = fs.readFileSync(keyPath, 'utf8');
    return jwt.sign(
        {
            iss: issuerId,
            iat: Math.floor(Date.now() / 1000),
            exp: Math.floor(Date.now() / 1000) + 1200, // 20 min max
            aud: 'appstoreconnect-v1'
        },
        privateKey,
        {
            algorithm: 'ES256',
            header: { alg: 'ES256', kid: keyId, typ: 'JWT' }
        }
    );
}
```

### Use in Requests

```
Authorization: Bearer {token}
Content-Type: application/json
```

---

## API Base URL

```
https://api.appstoreconnect.apple.com
```

---

## Upload Flow (6 Steps)

### Step 1: Find App

```
GET /v1/apps?filter[bundleId]={bundleId}
```

Response: `data[0].id` → appId

### Step 2: Find Editable Version

```
GET /v1/apps/{appId}/appStoreVersions?filter[appStoreState]=PREPARE_FOR_SUBMISSION
```

**Valid filter values for `appStoreState`:**

| Value | Description |
|-------|-------------|
| `PREPARE_FOR_SUBMISSION` | App is being prepared |
| `DEVELOPER_REJECTED` | Developer rejected the submission |
| `REJECTED` | Rejected by App Review |
| `METADATA_REJECTED` | Metadata issues |
| `WAITING_FOR_REVIEW` | In the review queue |
| `IN_REVIEW` | Being reviewed |
| `PENDING_DEVELOPER_RELEASE` | Approved, awaiting manual release |
| `READY_FOR_SALE` | Live on the App Store |

```
⚠️ "READY_FOR_DISTRIBUTION" is NOT a valid value.
   Using it will return an API error.
```

Response: `data[0].id` → versionId

### Step 3: Get or Create Localizations

```
GET /v1/appStoreVersions/{versionId}/appStoreVersionLocalizations
```

To create a new localization:

```
POST /v1/appStoreVersionLocalizations
```

```json
{
    "data": {
        "type": "appStoreVersionLocalizations",
        "attributes": {
            "locale": "tr"
        },
        "relationships": {
            "appStoreVersion": {
                "data": { "type": "appStoreVersions", "id": "{versionId}" }
            }
        }
    }
}
```

Response: `data.id` → localizationId

### Step 4: Create Screenshot Set

```
POST /v1/appScreenshotSets
```

```json
{
    "data": {
        "type": "appScreenshotSets",
        "attributes": {
            "screenshotDisplayType": "APP_IPHONE_67"
        },
        "relationships": {
            "appStoreVersionLocalization": {
                "data": {
                    "type": "appStoreVersionLocalizations",
                    "id": "{localizationId}"
                }
            }
        }
    }
}
```

Response: `data.id` → setId

### Step 5: Delete Existing Screenshots (if replacing)

First, list existing screenshots:

```
GET /v1/appScreenshotSets/{setId}/appScreenshots
```

Then delete each one:

```
DELETE /v1/appScreenshots/{screenshotId}
```

### Step 6: Upload Each Screenshot (3-part process)

#### 6a. Reserve Upload

```
POST /v1/appScreenshots
```

```json
{
    "data": {
        "type": "appScreenshots",
        "attributes": {
            "fileName": "01_hook.png",
            "fileSize": 2456789
        },
        "relationships": {
            "appScreenshotSet": {
                "data": { "type": "appScreenshotSets", "id": "{setId}" }
            }
        }
    }
}
```

Response includes `uploadOperations` — an array of upload parts.

#### 6b. Upload Binary Parts

For each operation in `uploadOperations`:

```
PUT {operation.url}
Headers: {operation.requestHeaders}
Body: file bytes from offset to offset + length
```

#### 6c. Commit Upload

```
PATCH /v1/appScreenshots/{screenshotId}
```

```json
{
    "data": {
        "type": "appScreenshots",
        "id": "{screenshotId}",
        "attributes": {
            "uploaded": true,
            "sourceFileChecksum": "{md5-base64}"
        }
    }
}
```

The `sourceFileChecksum` is the MD5 hash of the complete file, base64-encoded:

```javascript
const crypto = require('crypto');
const md5 = crypto.createHash('md5').update(fileBuffer).digest('base64');
```

---

## Display Type Mapping

```
⚠️ APP_IPHONE_69 does NOT exist in the API.
   Use APP_IPHONE_67 for both 6.7" and 6.9" screenshots.
```

### iPhone Display Types

| Device | API Display Type | Accepted Dimensions (Portrait) |
|--------|-----------------|-------------------------------|
| iPhone 6.9" | `APP_IPHONE_67` | 1260 x 2736 |
| iPhone 6.7" | `APP_IPHONE_67` | 1290 x 2796 |
| iPhone 6.5" | `APP_IPHONE_65` | 1284 x 2778, 1242 x 2688 |
| iPhone 6.1" | `APP_IPHONE_61` | 1179 x 2556, 1170 x 2532 |
| iPhone 5.8" | `APP_IPHONE_58` | 1125 x 2436 |
| iPhone 5.5" | `APP_IPHONE_55` | 1242 x 2208 |
| iPhone 4.7" | `APP_IPHONE_47` | 750 x 1334 |

### iPad Display Types

| Device | API Display Type | Accepted Dimensions (Portrait) |
|--------|-----------------|-------------------------------|
| iPad 13" / 12.9" | `APP_IPAD_PRO_3GEN_129` | 2064 x 2752, 2048 x 2732 |
| iPad 11" | `APP_IPAD_PRO_3GEN_11` | 1668 x 2388 |
| iPad 10.9" | `APP_IPAD_109` | 1640 x 2360 |
| iPad 10.5" | `APP_IPAD_PRO_129` | 1668 x 2224 |
| iPad 9.7" | `APP_IPAD_97` | 1536 x 2048 |

---

## Locale Codes

Common locale codes for ASC API:

| Code | Language |
|------|----------|
| `en-US` | English (US) |
| `en-GB` | English (UK) |
| `de-DE` | German |
| `fr-FR` | French |
| `es-ES` | Spanish (Spain) |
| `it` | Italian |
| `pt-BR` | Portuguese (Brazil) |
| `ja` | Japanese |
| `ko` | Korean |
| `zh-Hans` | Chinese (Simplified) |
| `zh-Hant` | Chinese (Traditional) |
| `tr` | Turkish |
| `nl-NL` | Dutch |
| `ru` | Russian |
| `ar-SA` | Arabic |

**Note:** Some locales use short codes (`tr`, `ja`, `ko`) while others require region (`en-US`, `de-DE`, `pt-BR`). The ASC API will reject invalid locale formats.

---

## Common Gotchas

| Issue | Solution |
|-------|----------|
| `READY_FOR_DISTRIBUTION` filter error | Use `READY_FOR_SALE` instead |
| `APP_IPHONE_69` display type error | Use `APP_IPHONE_67` |
| Screenshots not appearing after upload | Must call PATCH with `uploaded: true` |
| MD5 checksum mismatch | Use `crypto.createHash('md5').digest('base64')` not hex |
| Can't replace screenshots | DELETE existing screenshots first, then upload new ones |
| Locale not found | Create localization first via POST |
| Upload timeout | Large files may need retry with backoff |

---

## Complete Upload Script

See the full working implementation in SKILL.md Workflow 9 or use the reference
implementation from Appendix F of the field test feedback document.

```javascript
// Quick usage example
const config = {
    keyPath: './AuthKey_XXXXXXXXXX.p8',
    keyId: 'XXXXXXXXXX',
    issuerId: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
    bundleId: 'com.example.myapp',
    locale: 'en-US',
    screenshots: [
        { filePath: 'exports/en/01_hook.png', fileName: '01_hook.png' },
        { filePath: 'exports/en/02_feature.png', fileName: '02_feature.png' },
    ]
};

uploadScreenshots(config);
```

---

## Rate Limits

The ASC API has rate limits (not publicly documented). Recommendations:

- Add 1-2 second delay between screenshot uploads
- Retry on 429 (Too Many Requests) with exponential backoff
- JWT tokens expire after 20 minutes — regenerate if needed
