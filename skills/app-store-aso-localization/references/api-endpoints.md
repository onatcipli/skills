# API Endpoints Reference

App Store Connect API endpoints for metadata and localization management.

## Table of Contents

- [Authentication](#authentication)
- [App Info Localizations](#app-info-localizations)
- [App Store Version Localizations](#app-store-version-localizations)
- [Public APIs](#public-apis)
- [Rate Limits](#rate-limits)
- [Error Handling](#error-handling)

---

## Authentication

### JWT Token Generation

App Store Connect API uses JWT (JSON Web Token) authentication with ES256 algorithm.

#### Required Credentials

1. **Key ID** — From App Store Connect (Users and Access > Keys)
2. **Issuer ID** — Your team's issuer ID
3. **Private Key** — Downloaded .p8 file

#### Token Generation

```python
import jwt
import time

def generate_token(key_id, issuer_id, private_key):
    """
    Generate JWT token for App Store Connect API.
    Token valid for 20 minutes.
    """
    now = int(time.time())

    payload = {
        "iss": issuer_id,
        "iat": now,
        "exp": now + 1200,  # 20 minutes
        "aud": "appstoreconnect-v1"
    }

    headers = {
        "alg": "ES256",
        "kid": key_id,
        "typ": "JWT"
    }

    token = jwt.encode(
        payload,
        private_key,
        algorithm="ES256",
        headers=headers
    )

    return token
```

#### Request Headers

```
Authorization: Bearer {token}
Content-Type: application/json
```

### Base URL

```
https://api.appstoreconnect.apple.com
```

---

## App Info Localizations

Manage app-level metadata: **App Name** and **Subtitle**.

### List App Infos

Get app info resources for an app.

```
GET /v1/apps/{appId}/appInfos
```

**Response:**
```json
{
  "data": [
    {
      "type": "appInfos",
      "id": "abc123",
      "attributes": {
        "appStoreState": "READY_FOR_SALE",
        "appStoreAgeRating": "FOUR_PLUS"
      },
      "relationships": {
        "appInfoLocalizations": {
          "links": {
            "related": "/v1/appInfos/abc123/appInfoLocalizations"
          }
        }
      }
    }
  ]
}
```

### List App Info Localizations

Get all localizations for an app info.

```
GET /v1/appInfos/{appInfoId}/appInfoLocalizations
```

**Response:**
```json
{
  "data": [
    {
      "type": "appInfoLocalizations",
      "id": "loc123",
      "attributes": {
        "locale": "en-US",
        "name": "My App Name",
        "subtitle": "My App Subtitle",
        "privacyPolicyUrl": "https://example.com/privacy",
        "privacyPolicyText": null,
        "privacyChoicesUrl": null
      }
    },
    {
      "type": "appInfoLocalizations",
      "id": "loc456",
      "attributes": {
        "locale": "de-DE",
        "name": "Mein App Name",
        "subtitle": "Mein App Untertitel",
        "privacyPolicyUrl": "https://example.com/privacy-de"
      }
    }
  ]
}
```

### Create App Info Localization

Add a new locale for app info (name/subtitle).

```
POST /v1/appInfoLocalizations
```

**Request Body:**
```json
{
  "data": {
    "type": "appInfoLocalizations",
    "attributes": {
      "locale": "fr-FR",
      "name": "Mon Application",
      "subtitle": "Le meilleur éditeur de photos"
    },
    "relationships": {
      "appInfo": {
        "data": {
          "type": "appInfos",
          "id": "{appInfoId}"
        }
      }
    }
  }
}
```

### Update App Info Localization

Update name/subtitle for existing locale.

```
PATCH /v1/appInfoLocalizations/{localizationId}
```

**Request Body:**
```json
{
  "data": {
    "type": "appInfoLocalizations",
    "id": "{localizationId}",
    "attributes": {
      "name": "Updated App Name",
      "subtitle": "Updated Subtitle"
    }
  }
}
```

### Delete App Info Localization

Remove a locale.

```
DELETE /v1/appInfoLocalizations/{localizationId}
```

---

## App Store Version Localizations

Manage version-level metadata: **Keywords**, **Description**, **Promotional Text**, **What's New**.

### List App Store Versions

Get versions for an app.

```
GET /v1/apps/{appId}/appStoreVersions
```

**Query Parameters:**
- `filter[appStoreState]` — Filter by state (e.g., `READY_FOR_SALE`, `PREPARE_FOR_SUBMISSION`)

**Example:**
```
GET /v1/apps/{appId}/appStoreVersions?filter[appStoreState]=PREPARE_FOR_SUBMISSION
```

### List Version Localizations

Get all localizations for a version.

```
GET /v1/appStoreVersions/{versionId}/appStoreVersionLocalizations
```

**Response:**
```json
{
  "data": [
    {
      "type": "appStoreVersionLocalizations",
      "id": "verloc123",
      "attributes": {
        "locale": "en-US",
        "description": "Full app description here...",
        "keywords": "photo,editor,camera,filter,enhance",
        "promotionalText": "Now with AI features!",
        "whatsNew": "Bug fixes and improvements",
        "marketingUrl": "https://example.com",
        "supportUrl": "https://example.com/support"
      }
    }
  ]
}
```

### Create Version Localization

Add a new locale for version metadata.

```
POST /v1/appStoreVersionLocalizations
```

**Request Body:**
```json
{
  "data": {
    "type": "appStoreVersionLocalizations",
    "attributes": {
      "locale": "de-DE",
      "description": "German description...",
      "keywords": "foto,bearbeitung,kamera,filter",
      "promotionalText": "Jetzt mit KI-Funktionen!",
      "whatsNew": "Fehlerbehebungen und Verbesserungen"
    },
    "relationships": {
      "appStoreVersion": {
        "data": {
          "type": "appStoreVersions",
          "id": "{versionId}"
        }
      }
    }
  }
}
```

### Update Version Localization

Update metadata for existing locale.

```
PATCH /v1/appStoreVersionLocalizations/{localizationId}
```

**Request Body:**
```json
{
  "data": {
    "type": "appStoreVersionLocalizations",
    "id": "{localizationId}",
    "attributes": {
      "keywords": "updated,keywords,here",
      "description": "Updated description...",
      "promotionalText": "Updated promo text",
      "whatsNew": "Updated release notes"
    }
  }
}
```

### Delete Version Localization

Remove a locale from a version.

```
DELETE /v1/appStoreVersionLocalizations/{localizationId}
```

---

## Public APIs

No authentication required.

### iTunes Search API

Search for apps by keyword.

```
GET https://itunes.apple.com/search
```

**Parameters:**
| Parameter | Description | Example |
|-----------|-------------|---------|
| `term` | Search query | `photo editor` |
| `country` | 2-letter country code | `us`, `de`, `jp` |
| `media` | Media type | `software` |
| `entity` | Entity type | `software`, `iPadSoftware` |
| `limit` | Results limit (max 200) | `50` |

**Example:**
```
GET https://itunes.apple.com/search?term=photo+editor&country=us&media=software&limit=25
```

**Response Fields:**
```json
{
  "resultCount": 25,
  "results": [
    {
      "trackId": 123456789,
      "trackName": "App Name",
      "bundleId": "com.example.app",
      "description": "Full description...",
      "sellerName": "Company Name",
      "averageUserRating": 4.5,
      "userRatingCount": 10000,
      "price": 0,
      "formattedPrice": "Free",
      "primaryGenreName": "Photo & Video",
      "genres": ["Photo & Video", "Entertainment"]
    }
  ]
}
```

### iTunes Lookup API

Get details for a specific app.

```
GET https://itunes.apple.com/lookup
```

**Parameters:**
| Parameter | Description | Example |
|-----------|-------------|---------|
| `id` | App Store ID | `123456789` |
| `bundleId` | Bundle identifier | `com.example.app` |
| `country` | 2-letter country code | `us` |

**Examples:**
```
GET https://itunes.apple.com/lookup?id=123456789&country=us
GET https://itunes.apple.com/lookup?bundleId=com.example.app&country=de
```

---

## Rate Limits

### App Store Connect API

| Limit | Value | Notes |
|-------|-------|-------|
| Requests per hour | ~3,600 | Undocumented, estimate |
| Requests per minute | ~60 | Conservative estimate |
| Concurrent requests | ~10 | Recommended limit |

**Best Practices:**
- Implement exponential backoff on 429 errors
- Cache responses where possible
- Batch operations when available

### iTunes Search/Lookup API

| Limit | Value | Notes |
|-------|-------|-------|
| Requests per minute | ~20 | Conservative estimate |
| Between requests | 3 seconds | Recommended delay |

**Best Practices:**
- Add 3-second delay between requests
- Cache responses for 24 hours
- Implement retry with backoff on 403/429

---

## Error Handling

### Common Error Responses

#### 401 Unauthorized

```json
{
  "errors": [
    {
      "status": "401",
      "code": "NOT_AUTHORIZED",
      "title": "Authentication credentials are missing or invalid."
    }
  ]
}
```

**Solution:** Regenerate JWT token (may have expired).

#### 403 Forbidden

```json
{
  "errors": [
    {
      "status": "403",
      "code": "FORBIDDEN_ERROR",
      "title": "This request is forbidden for the current user."
    }
  ]
}
```

**Solution:** Check API key permissions.

#### 409 Conflict

```json
{
  "errors": [
    {
      "status": "409",
      "code": "STATE_ERROR",
      "title": "The request cannot be fulfilled because of the state of another resource."
    }
  ]
}
```

**Solution:** App may be in review or another process is running.

#### 422 Unprocessable Entity

```json
{
  "errors": [
    {
      "status": "422",
      "code": "ENTITY_ERROR.ATTRIBUTE.INVALID",
      "title": "The provided entity includes an attribute with an invalid value.",
      "detail": "'keywords' may not exceed 100 characters."
    }
  ]
}
```

**Solution:** Fix the validation error described in detail.

#### 429 Too Many Requests

```json
{
  "errors": [
    {
      "status": "429",
      "code": "RATE_LIMIT_EXCEEDED",
      "title": "Rate limit exceeded."
    }
  ]
}
```

**Solution:** Implement exponential backoff.

### Retry Strategy

```python
import time

def api_request_with_retry(url, method, data=None, max_retries=4):
    """
    Make API request with exponential backoff retry.
    """
    for attempt in range(max_retries):
        response = make_request(url, method, data)

        if response.status_code == 429:
            wait_time = 2 ** attempt  # 1, 2, 4, 8 seconds
            time.sleep(wait_time)
            continue

        if response.status_code >= 500:
            wait_time = 2 ** attempt
            time.sleep(wait_time)
            continue

        return response

    raise Exception(f"Max retries exceeded for {url}")
```

---

## Quick Reference

### Endpoints Summary

| Action | Method | Endpoint |
|--------|--------|----------|
| List app infos | GET | `/v1/apps/{id}/appInfos` |
| List app info localizations | GET | `/v1/appInfos/{id}/appInfoLocalizations` |
| Create app info localization | POST | `/v1/appInfoLocalizations` |
| Update app info localization | PATCH | `/v1/appInfoLocalizations/{id}` |
| Delete app info localization | DELETE | `/v1/appInfoLocalizations/{id}` |
| List app versions | GET | `/v1/apps/{id}/appStoreVersions` |
| List version localizations | GET | `/v1/appStoreVersions/{id}/appStoreVersionLocalizations` |
| Create version localization | POST | `/v1/appStoreVersionLocalizations` |
| Update version localization | PATCH | `/v1/appStoreVersionLocalizations/{id}` |
| Delete version localization | DELETE | `/v1/appStoreVersionLocalizations/{id}` |
| iTunes search | GET | `itunes.apple.com/search?term={q}&country={cc}` |
| iTunes lookup | GET | `itunes.apple.com/lookup?id={id}&country={cc}` |

### Field Limits

| Field | Limit | API Attribute |
|-------|-------|---------------|
| App Name | 30 chars | `name` |
| Subtitle | 30 chars | `subtitle` |
| Keywords | 100 chars | `keywords` |
| Promotional Text | 170 chars | `promotionalText` |
| Description | 4,000 chars | `description` |
| What's New | 4,000 chars | `whatsNew` |
