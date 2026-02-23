# iTunes API Reference

Endpoints for fetching app data and screenshot URLs.

---

## iTunes Lookup API

Get detailed app information including screenshot URLs.

### Endpoint

```
GET https://itunes.apple.com/lookup?id={appId}&country={countryCode}
```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `id` | Yes | iTunes App ID (numeric) |
| `country` | No | Two-letter country code (default: us) |

### Example Request

```bash
curl "https://itunes.apple.com/lookup?id=324684580&country=us"
```

### Example Response

```json
{
  "resultCount": 1,
  "results": [
    {
      "trackId": 324684580,
      "trackName": "Spotify - Music and Podcasts",
      "bundleId": "com.spotify.client",
      "sellerName": "Spotify AB",
      "primaryGenreName": "Music",
      "primaryGenreId": 6011,
      "averageUserRating": 4.8,
      "userRatingCount": 24500000,
      "price": 0,
      "formattedPrice": "Free",
      "currentVersionReleaseDate": "2024-01-15T08:00:00Z",
      "version": "8.9.0",
      "description": "With Spotify, you can listen to music...",

      "artworkUrl60": "https://is1-ssl.mzstatic.com/.../60x60bb.jpg",
      "artworkUrl100": "https://is1-ssl.mzstatic.com/.../100x100bb.jpg",
      "artworkUrl512": "https://is1-ssl.mzstatic.com/.../512x512bb.jpg",

      "screenshotUrls": [
        "https://is1-ssl.mzstatic.com/.../1290x2796bb.png",
        "https://is1-ssl.mzstatic.com/.../1290x2796bb.png",
        "https://is1-ssl.mzstatic.com/.../1290x2796bb.png"
      ],
      "ipadScreenshotUrls": [
        "https://is1-ssl.mzstatic.com/.../2048x2732bb.png",
        "https://is1-ssl.mzstatic.com/.../2048x2732bb.png"
      ],
      "appletvScreenshotUrls": []
    }
  ]
}
```

### Key Fields for Screenshots

| Field | Description |
|-------|-------------|
| `screenshotUrls` | Array of iPhone screenshot URLs |
| `ipadScreenshotUrls` | Array of iPad screenshot URLs |
| `artworkUrl512` | App icon (512px) |
| `artworkUrl100` | App icon (100px) |

---

## IMPORTANT: Empty Screenshot Arrays

Many apps return `"screenshotUrls": []` from the iTunes Lookup API. This is **common
and expected behavior** — it is not a rate limit or error. The API simply doesn't
include screenshots for many apps (especially newer or smaller ones).

### Fallback Strategy (try in order)

#### 1. Try Different Country Codes

Some countries return screenshots when others don't:

```bash
# Try these in order: us, gb, de, au, cy, tr, jp
curl "https://itunes.apple.com/lookup?id={appId}&country=gb"
curl "https://itunes.apple.com/lookup?id={appId}&country=de"
curl "https://itunes.apple.com/lookup?id={appId}&country=au"
```

#### 2. Scrape the App Store Web Page

App Store web pages render via JavaScript, but the raw HTML source often contains
screenshot URLs as data attributes or in embedded JSON:

```bash
# Fetch the App Store web page
curl "https://apps.apple.com/us/app/id{appId}" -o page.html

# Search for mzstatic.com URLs with screenshot dimensions
grep -oP 'https://is\d+-ssl\.mzstatic\.com/[^"]+\d{3,4}x\d{3,4}[^"]*\.png' page.html
```

Look for URLs matching patterns like:
- `is1-ssl.mzstatic.com/image/thumb/.../{width}x{height}bb.png`
- Dimensions like `1290x2796`, `1242x2208`, `1260x2736`

#### 3. Manual Capture

As a last resort, instruct the user to:
- Visit the App Store listing in a browser
- Take screenshots of each competitor screenshot
- Save to the competitor's folder

### Download Script with Fallback

```python
import requests
import json
from pathlib import Path

def download_screenshots(app_id, app_name, output_dir, country="us"):
    """Download screenshots with country code fallback."""
    countries = [country, "us", "gb", "de", "au", "cy", "tr"]
    data = None

    for cc in countries:
        url = f"https://itunes.apple.com/lookup?id={app_id}&country={cc}"
        response = requests.get(url, timeout=10)
        result = response.json()

        if result["resultCount"] > 0:
            app = result["results"][0]
            if app.get("screenshotUrls"):
                data = app
                print(f"Found screenshots via country={cc}")
                break
            elif data is None:
                data = app  # Keep metadata even without screenshots

    if not data:
        print(f"App {app_id} not found in any country")
        return None

    if not data.get("screenshotUrls"):
        print(f"WARNING: No screenshots found for {app_name} via iTunes API")
        print(f"Try: curl 'https://apps.apple.com/us/app/id{app_id}' and search for mzstatic.com URLs")
        # Still save metadata
        base_path = Path(output_dir) / "competitors" / app_name
        base_path.mkdir(parents=True, exist_ok=True)
        with open(base_path / "metadata.json", "w") as f:
            json.dump(data, f, indent=2)
        return data

    # Create directories
    base_path = Path(output_dir) / "competitors" / app_name
    iphone_path = base_path / "iphone"
    ipad_path = base_path / "ipad"
    iphone_path.mkdir(parents=True, exist_ok=True)
    ipad_path.mkdir(parents=True, exist_ok=True)

    # Download iPhone screenshots
    for i, img_url in enumerate(data.get("screenshotUrls", []), 1):
        print(f"Downloading iPhone screenshot {i}...")
        img_response = requests.get(img_url, timeout=30)
        with open(iphone_path / f"{i:02d}.png", "wb") as f:
            f.write(img_response.content)

    # Download iPad screenshots
    for i, img_url in enumerate(data.get("ipadScreenshotUrls", []), 1):
        print(f"Downloading iPad screenshot {i}...")
        img_response = requests.get(img_url, timeout=30)
        with open(ipad_path / f"{i:02d}.png", "wb") as f:
            f.write(img_response.content)

    # Save metadata
    with open(base_path / "metadata.json", "w") as f:
        json.dump(data, f, indent=2)

    print(f"Downloaded {len(data.get('screenshotUrls', []))} iPhone screenshots")
    print(f"Downloaded {len(data.get('ipadScreenshotUrls', []))} iPad screenshots")
    return data
```

---

## CDN Dimension Warning

```
⚠️ The Apple CDN (mzstatic.com) does NOT upscale images.

Screenshot URLs contain dimensions like /1290x2796bb.png but these are
REQUESTED dimensions, not guaranteed dimensions. The CDN returns the
original upload resolution regardless.

Example: If a developer uploaded 1242x2208 screenshots, requesting
.../1290x2796bb.png silently returns the original 1242x2208 image.

ALWAYS verify actual downloaded dimensions:
  macOS:  sips -g pixelWidth -g pixelHeight screenshot.png
  Linux:  identify screenshot.png  (ImageMagick)
  Python: from PIL import Image; Image.open("screenshot.png").size
```

---

## iTunes Search API

Search for apps by keyword.

### Endpoint

```
GET https://itunes.apple.com/search?term={query}&entity=software&country={cc}&limit={n}
```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `term` | Yes | Search query (URL encoded) |
| `entity` | Yes | Use `software` for apps |
| `country` | No | Two-letter country code (default: us) |
| `limit` | No | Results limit, 1-200 (default: 50) |
| `attribute` | No | Filter: `softwareDeveloper` for dev search |

### Example Requests

```bash
# Search for meditation apps
curl "https://itunes.apple.com/search?term=meditation&entity=software&country=us&limit=25"

# Search by developer
curl "https://itunes.apple.com/search?term=Spotify&entity=software&attribute=softwareDeveloper"
```

---

## Apple RSS Feeds (Top Charts)

Get top apps by category and chart type.

### Endpoint

```
GET https://rss.applemarketingtools.com/api/v2/{country}/apps/{chartType}/{limit}/apps.json
```

### Parameters

| Parameter | Values |
|-----------|--------|
| `country` | `us`, `gb`, `de`, `jp`, etc. |
| `chartType` | `top-free`, `top-paid`, `top-grossing` |
| `limit` | 1-100 |

### Filter by Category

Add `?genre={genreId}` to filter by category.

### Example Requests

```bash
# Top 50 free apps in US
curl "https://rss.applemarketingtools.com/api/v2/us/apps/top-free/50/apps.json"

# Top 25 paid games in US
curl "https://rss.applemarketingtools.com/api/v2/us/apps/top-paid/25/apps.json?genre=6014"

# Top grossing health apps
curl "https://rss.applemarketingtools.com/api/v2/us/apps/top-grossing/50/apps.json?genre=6013"
```

### Example Response

```json
{
  "feed": {
    "title": "Top Free Apps",
    "country": "us",
    "updated": "2024-01-15T12:00:00.000Z",
    "results": [
      {
        "artistName": "Spotify AB",
        "id": "324684580",
        "name": "Spotify - Music and Podcasts",
        "releaseDate": "2011-07-14",
        "kind": "apps",
        "artworkUrl100": "https://...",
        "genres": [
          { "genreId": "6011", "name": "Music" }
        ],
        "url": "https://apps.apple.com/us/app/spotify/id324684580"
      }
    ]
  }
}
```

**Note:** RSS feeds don't include screenshot URLs. Use the `id` to lookup full details via the Lookup API.

---

## Category (Genre) IDs

| ID | Category |
|----|----------|
| 6000 | Business |
| 6001 | Weather |
| 6002 | Utilities |
| 6003 | Travel |
| 6004 | Sports |
| 6005 | Social Networking |
| 6006 | Reference |
| 6007 | Productivity |
| 6008 | Photo & Video |
| 6009 | News |
| 6010 | Navigation |
| 6011 | Music |
| 6012 | Lifestyle |
| 6013 | Health & Fitness |
| 6014 | Games |
| 6015 | Finance |
| 6016 | Entertainment |
| 6017 | Education |
| 6018 | Books |
| 6020 | Medical |
| 6021 | Magazines & Newspapers |
| 6022 | Catalogs |
| 6023 | Food & Drink |
| 6024 | Shopping |
| 6025 | Stickers |
| 6026 | Developer Tools |
| 6027 | Graphics & Design |

---

## Rate Limits

| API | Limit | Recommendation |
|-----|-------|----------------|
| iTunes Search | ~20 req/min | Add 3s delay between calls |
| iTunes Lookup | ~20 req/min | Add 3s delay between calls |
| RSS Feeds | Undocumented | Add 5s delay between calls |

---

## Error Handling

### Common Errors

| Status | Meaning | Solution |
|--------|---------|----------|
| 200 with `resultCount: 0` | App not found | Check app ID, try different country |
| 200 with empty `screenshotUrls` | No screenshots in API | Use fallback strategy (see above) |
| 403 | Rate limited | Wait and retry with backoff |
| 503 | Service unavailable | Retry after delay |

### Retry Logic

```python
import time
import requests

def fetch_with_retry(url, max_retries=3, delay=3):
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=10)
            if response.status_code == 200:
                return response.json()
            elif response.status_code == 403:
                print(f"Rate limited, waiting {delay * (attempt + 1)}s...")
                time.sleep(delay * (attempt + 1))
        except requests.RequestException as e:
            print(f"Request failed: {e}")
            time.sleep(delay)

    return None
```
