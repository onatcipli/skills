# Apple Endpoints Reference

## Table of Contents

- [§1 Public Endpoints](#1-public-endpoints)
  - [§1.1 iTunes Search](#11-itunes-search)
  - [§1.2 iTunes Lookup](#12-itunes-lookup)
  - [§1.3 RSS Customer Reviews](#13-rss-customer-reviews)
- [§2 Private Endpoints (App Store Connect API)](#2-private-endpoints-app-store-connect-api)
  - [§2.1 JWT Authentication](#21-jwt-authentication)
  - [§2.2 Customer Reviews](#22-customer-reviews)
  - [§2.3 Customer Review Responses](#23-customer-review-responses)
  - [§2.4 Review Summarizations](#24-review-summarizations)
  - [§2.5 App Store Versions](#25-app-store-versions)

---

## §1 Public Endpoints

No authentication required. Works for any app.

### §1.1 iTunes Search

```
GET https://itunes.apple.com/search
```

| Param     | Type   | Description                                  |
|-----------|--------|----------------------------------------------|
| `term`    | string | Search query (URL-encode spaces as `+`)      |
| `country` | string | ISO 3166-1 alpha-2 code (default: `us`)     |
| `media`   | string | `software` for apps                          |
| `entity`  | string | `software`, `iPadSoftware`, `macSoftware`    |
| `limit`   | int    | 1–200, default 50                            |
| `lang`    | string | e.g., `en_us`, `ja_jp`                       |

**Rate limit:** ~20 calls/min

**Response:** JSON with `resultCount` and `results[]`. Key fields per result:
`trackId`, `trackName`, `bundleId`, `averageUserRating`, `userRatingCount`,
`version`, `description`, `sellerName`, `primaryGenreName`, `price`

### §1.2 iTunes Lookup

```
GET https://itunes.apple.com/lookup
```

| Param      | Type   | Description                              |
|------------|--------|------------------------------------------|
| `id`       | int    | App Store ID (`trackId`)                 |
| `bundleId` | string | Bundle identifier                        |
| `country`  | string | Required for non-US apps                 |

Same response format as Search API.

### §1.3 RSS Customer Reviews

```
GET https://itunes.apple.com/{country}/rss/customerreviews/page={page}/id={appId}/sortby={sort}/{format}
```

| Param       | Values                        |
|-------------|-------------------------------|
| `{country}` | `us`, `gb`, `de`, `fr`, `jp`, `au`, `ca`, `br`, `kr`, etc. |
| `{page}`    | 1–10                          |
| `{appId}`   | App Store ID                  |
| `{sort}`    | `mostrecent`, `mosthelpful`   |
| `{format}`  | `json`, `xml`                 |

**Limits:** 50 reviews/page, 10 pages max = 500 reviews/country.

**JSON response path:** `feed.entry[]`

| Field                   | Description          |
|-------------------------|----------------------|
| `im:rating.label`      | Star rating (1–5)    |
| `title.label`          | Review title         |
| `content.label`        | Review body          |
| `author.name.label`    | Reviewer name        |
| `im:version.label`     | App version          |
| `updated.label`        | Review date (ISO)    |
| `id.label`             | Review ID            |

---

## §2 Private Endpoints (App Store Connect API)

Base URL: `https://api.appstoreconnect.apple.com`

All requests require: `Authorization: Bearer <JWT>`

### §2.1 JWT Authentication

**Prerequisites:** Generate an API key in App Store Connect → Users and Access → Integrations → App Store Connect API. You receive: Issuer ID, Key ID, Private Key (.p8 file).

**JWT payload:**

| Field | Value |
|-------|-------|
| `alg` | `ES256` |
| `kid` | Key ID |
| `iss` | Issuer ID |
| `iat` | Current UNIX timestamp |
| `exp` | Max `iat` + 1200 (20 minutes) |
| `aud` | `appstoreconnect-v1` |

**Node.js:**

```javascript
const jwt = require("jsonwebtoken");
const fs = require("fs");
const privateKey = fs.readFileSync("AuthKey_XXXXXX.p8");
const token = jwt.sign({}, privateKey, {
  algorithm: "ES256",
  expiresIn: "15m",
  issuer: ISSUER_ID,
  audience: "appstoreconnect-v1",
  header: { alg: "ES256", kid: KEY_ID, typ: "JWT" }
});
```

**Python:**

```python
import jwt, time
with open("AuthKey_XXXXXX.p8") as f:
    key = f.read()
token = jwt.encode(
    {"iss": ISSUER_ID, "iat": int(time.time()), "exp": int(time.time()) + 900, "aud": "appstoreconnect-v1"},
    key, algorithm="ES256", headers={"kid": KEY_ID}
)
```

### §2.2 Customer Reviews

#### List all reviews for an app

```
GET /v1/apps/{appId}/customerReviews
```

| Param | Description |
|-------|-------------|
| `filter[rating]` | `1`,`2`,`3`,`4`,`5` (comma-separated) |
| `filter[territory]` | ISO 3166-1 alpha-3 (`USA`, `GBR`, `DEU`) |
| `sort` | `createdDate`, `-createdDate`, `rating`, `-rating` |
| `fields[customerReviews]` | `rating,title,body,reviewerNickname,createdDate,territory` |
| `include` | `response` |
| `limit` | 1–200 (default ~50) |

**Response:** `CustomerReviewsResponse`

```json
{
  "data": [{
    "type": "customerReviews",
    "id": "...",
    "attributes": {
      "rating": 5,
      "title": "...",
      "body": "...",
      "reviewerNickname": "...",
      "createdDate": "2025-03-15T10:30:00Z",
      "territory": "USA"
    },
    "relationships": { "response": { "links": { "related": "..." } } }
  }],
  "links": { "self": "...", "next": "...?cursor=OPAQUE_TOKEN&limit=50" },
  "meta": { "paging": { "total": 1234, "limit": 50 } }
}
```

Pagination: cursor-based. Follow `links.next` until absent.

#### List reviews for a specific version

```
GET /v1/appStoreVersions/{versionId}/customerReviews
```

Same parameters and response as above. Get `versionId` from §2.5.

#### Read a single review

```
GET /v1/customerReviews/{reviewId}
```

Params: `fields[customerReviews]`, `include`

### §2.3 Customer Review Responses

#### Get response for a review

```
GET /v1/customerReviews/{reviewId}/response
```

#### Read response by its ID

```
GET /v1/customerReviewResponses/{responseId}
```

#### Create or update a response

```
POST /v1/customerReviewResponses
```

**Body:**

```json
{
  "data": {
    "type": "customerReviewResponses",
    "attributes": {
      "responseBody": "Your reply text here (max 5970 characters)"
    },
    "relationships": {
      "review": {
        "data": { "id": "REVIEW_ID", "type": "customerReviews" }
      }
    }
  }
}
```

The `data` wrapper is mandatory — omitting it returns `422 ENTITY_UNPROCESSABLE`.

#### Delete a response

```
DELETE /v1/customerReviewResponses/{responseId}
```

### §2.4 Review Summarizations

```
GET /v1/apps/{appId}/customerReviewSummarizations
```

Returns AI-generated summarization of customer reviews. Params: `fields[customerReviewSummarizations]`, `limit`.

### §2.5 App Store Versions

```
GET /v1/apps/{appId}/appStoreVersions
```

| Param | Description |
|-------|-------------|
| `filter[appStoreState]` | `READY_FOR_SALE`, `IN_REVIEW`, etc. |
| `filter[platform]` | `IOS`, `MAC_OS`, `TV_OS` |
| `fields[appStoreVersions]` | `versionString,appStoreState,platform,createdDate` |

Use this to get `versionId` for version-specific review queries.
