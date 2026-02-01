# Apple Pricing API Reference

Complete guide to App Store Connect API endpoints for pricing management.

## Authentication

### JWT Token Generation

App Store Connect API uses JWT (JSON Web Token) authentication with ES256 algorithm.

```python
import jwt
import time

def generate_token(key_id, issuer_id, private_key):
    """Generate JWT for App Store Connect API."""
    headers = {
        "alg": "ES256",
        "kid": key_id,
        "typ": "JWT"
    }

    payload = {
        "iss": issuer_id,
        "iat": int(time.time()),
        "exp": int(time.time()) + 1200,  # 20 minutes max
        "aud": "appstoreconnect-v1"
    }

    return jwt.encode(payload, private_key, algorithm="ES256", headers=headers)
```

### Required Credentials

Obtain from App Store Connect → Users and Access → Keys:

| Credential | Location | Format |
|------------|----------|--------|
| Key ID | Key details | 10-character string |
| Issuer ID | API Keys tab header | UUID format |
| Private Key | Downloaded .p8 file | PEM format |

### Request Headers

```
Authorization: Bearer {jwt_token}
Content-Type: application/json
```

---

## Public API (No Auth Required)

### iTunes Lookup

Get app metadata including price for any public app.

```
GET https://itunes.apple.com/lookup?id={app_id}&country={territory_code}
```

**Parameters:**
| Parameter | Required | Description |
|-----------|----------|-------------|
| id | Yes | App Store ID (numeric) |
| country | No | Two-letter country code (default: us) |

**Response Fields (Price-Related):**
```json
{
  "results": [{
    "trackId": 123456789,
    "trackName": "App Name",
    "price": 9.99,
    "currency": "USD",
    "formattedPrice": "$9.99"
  }]
}
```

**Example - Fetch Spotify price in India:**
```
GET https://itunes.apple.com/lookup?id=324684580&country=in
```

**Rate Limit:** ~20 requests/minute

---

## Private API Endpoints

Base URL: `https://api.appstoreconnect.apple.com`

### List All Territories

```
GET /v1/territories
```

Returns all 175 App Store territories.

**Response:**
```json
{
  "data": [
    {
      "type": "territories",
      "id": "USA",
      "attributes": {
        "currency": "USD"
      }
    }
  ]
}
```

---

### App Price Points

#### List All Price Points for an App

```
GET /v1/apps/{app_id}/appPricePoints
```

**Query Parameters:**
| Parameter | Description |
|-----------|-------------|
| filter[territory] | Filter by territory code (e.g., USA) |
| include | Include related resources (territory) |
| limit | Max results per page (max 200) |

**Example:**
```
GET /v1/apps/123456789/appPricePoints?filter[territory]=USA&include=territory
```

#### Get Price Point Equalizations

Get equivalent prices across all territories for a given price point.

```
GET /v1/appPricePoints/{price_point_id}/equalizations
```

**Response:**
```json
{
  "data": [
    {
      "type": "appPricePoints",
      "id": "eyJpIjoiMTIzIiwidCI6IlVTQSJ9",
      "attributes": {
        "customerPrice": "9.99",
        "proceeds": "7.00"
      },
      "relationships": {
        "territory": {
          "data": { "type": "territories", "id": "USA" }
        }
      }
    }
  ]
}
```

---

### Subscription Price Points

#### List Price Points for a Subscription

```
GET /v1/subscriptions/{subscription_id}/pricePoints
```

**Required Filter (as of API 3.6):**
```
GET /v1/subscriptions/{id}/pricePoints?filter[territory]=USA&include=territory
```

**Response:**
```json
{
  "data": [
    {
      "type": "subscriptionPricePoints",
      "id": "eyJpIjoiMTIzIiwidCI6IlVTQSJ9",
      "attributes": {
        "customerPrice": "9.99",
        "proceeds": "7.00"
      }
    }
  ]
}
```

#### Get Subscription Price Point Equalizations

```
GET /v1/subscriptionPricePoints/{price_point_id}/equalizations
```

Returns equivalent price points across all territories.

---

### In-App Purchase Price Points

#### List Price Points for an IAP

```
GET /v2/inAppPurchases/{iap_id}/pricePoints
```

**Query Parameters:**
```
GET /v2/inAppPurchases/{id}/pricePoints?filter[territory]=USA&include=territory&limit=200
```

#### Get IAP Price Point Equalizations

```
GET /v2/inAppPurchasePricePoints/{price_point_id}/equalizations
```

---

### Setting Prices

#### Set Subscription Price

```
POST /v1/subscriptionPrices
```

**Request Body:**
```json
{
  "data": {
    "type": "subscriptionPrices",
    "attributes": {
      "startDate": "2024-03-01",
      "preserveCurrentPrice": false
    },
    "relationships": {
      "subscription": {
        "data": {
          "type": "subscriptions",
          "id": "{subscription_id}"
        }
      },
      "subscriptionPricePoint": {
        "data": {
          "type": "subscriptionPricePoints",
          "id": "{price_point_id}"
        }
      }
    }
  }
}
```

**Notes:**
- `startDate` is optional — omit for immediate effect
- `preserveCurrentPrice` — if true, existing subscribers keep old price
- Must call for EACH territory you want to set

#### Set IAP Price Schedule

```
POST /v1/inAppPurchasePriceSchedules
```

**Request Body:**
```json
{
  "data": {
    "type": "inAppPurchasePriceSchedules",
    "relationships": {
      "inAppPurchase": {
        "data": {
          "type": "inAppPurchases",
          "id": "{iap_id}"
        }
      },
      "baseTerritory": {
        "data": {
          "type": "territories",
          "id": "USA"
        }
      },
      "manualPrices": {
        "data": [
          { "type": "inAppPurchasePrices", "id": "${placeholder-0}" },
          { "type": "inAppPurchasePrices", "id": "${placeholder-1}" }
        ]
      }
    }
  },
  "included": [
    {
      "type": "inAppPurchasePrices",
      "id": "${placeholder-0}",
      "attributes": {
        "startDate": null
      },
      "relationships": {
        "inAppPurchasePricePoint": {
          "data": {
            "type": "inAppPurchasePricePoints",
            "id": "{usa_price_point_id}"
          }
        },
        "inAppPurchaseV2": {
          "data": {
            "type": "inAppPurchases",
            "id": "{iap_id}"
          }
        }
      }
    },
    {
      "type": "inAppPurchasePrices",
      "id": "${placeholder-1}",
      "attributes": {
        "startDate": null
      },
      "relationships": {
        "inAppPurchasePricePoint": {
          "data": {
            "type": "inAppPurchasePricePoints",
            "id": "{gbr_price_point_id}"
          }
        },
        "inAppPurchaseV2": {
          "data": {
            "type": "inAppPurchases",
            "id": "{iap_id}"
          }
        }
      }
    }
  ]
}
```

---

## Price Point ID Format

Price point IDs are base64-encoded JSON containing:
- `i`: Price tier index
- `t`: Territory code

**Example:**
```
eyJpIjoiMTIzIiwidCI6IlVTQSJ9
// Decodes to: {"i":"123","t":"USA"}
```

---

## Common Workflows

### Get All Available Price Tiers for USA

```python
def get_usa_price_tiers(subscription_id, token):
    url = f"https://api.appstoreconnect.apple.com/v1/subscriptions/{subscription_id}/pricePoints"
    params = {
        "filter[territory]": "USA",
        "include": "territory",
        "limit": 200
    }
    headers = {"Authorization": f"Bearer {token}"}

    response = requests.get(url, params=params, headers=headers)
    return response.json()["data"]
```

### Get Equalizations for a Price Point

```python
def get_equalizations(price_point_id, token):
    url = f"https://api.appstoreconnect.apple.com/v1/subscriptionPricePoints/{price_point_id}/equalizations"
    headers = {"Authorization": f"Bearer {token}"}

    all_equalizations = []
    while url:
        response = requests.get(url, headers=headers)
        data = response.json()
        all_equalizations.extend(data["data"])
        url = data.get("links", {}).get("next")

    return all_equalizations
```

### Set Price for Multiple Territories

```python
def set_subscription_prices(subscription_id, price_points, token):
    """
    price_points: list of {"territory": "USA", "price_point_id": "..."}
    """
    url = "https://api.appstoreconnect.apple.com/v1/subscriptionPrices"
    headers = {
        "Authorization": f"Bearer {token}",
        "Content-Type": "application/json"
    }

    for pp in price_points:
        payload = {
            "data": {
                "type": "subscriptionPrices",
                "relationships": {
                    "subscription": {
                        "data": {"type": "subscriptions", "id": subscription_id}
                    },
                    "subscriptionPricePoint": {
                        "data": {"type": "subscriptionPricePoints", "id": pp["price_point_id"]}
                    }
                }
            }
        }
        response = requests.post(url, json=payload, headers=headers)
        print(f"{pp['territory']}: {response.status_code}")
```

---

## Rate Limits

| Endpoint Type | Limit | Backoff |
|---------------|-------|---------|
| App Store Connect API | Undocumented | Exponential on 429/503 |
| iTunes Lookup | ~20 req/min | Wait 3s between calls |

### Backoff Strategy

```python
import time

def api_call_with_retry(func, max_retries=4):
    for attempt in range(max_retries):
        try:
            response = func()
            if response.status_code == 429:
                wait = 2 ** attempt  # 1, 2, 4, 8 seconds
                time.sleep(wait)
                continue
            return response
        except Exception as e:
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)
```

---

## Error Handling

| Status | Meaning | Action |
|--------|---------|--------|
| 401 | Invalid/expired token | Regenerate JWT |
| 403 | Insufficient permissions | Check API key scope |
| 404 | Resource not found | Verify IDs |
| 409 | Conflict (e.g., pending change) | Wait or cancel existing |
| 429 | Rate limited | Backoff and retry |
| 500/503 | Server error | Retry with backoff |

---

## Useful Resources

- [App Store Connect API Documentation](https://developer.apple.com/documentation/appstoreconnectapi)
- [Subscription Price Points](https://developer.apple.com/documentation/appstoreconnectapi/subscription-price-points-and-subscription-prices)
- [Price Point Equalizations](https://developer.apple.com/documentation/appstoreconnectapi/list_all_subscription_price_point_equalizations)
- [Managing Prices via API (WWDC)](https://developer.apple.com/videos/play/wwdc2023/10014/)
