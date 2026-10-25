# Facebook Ad Library Scraper | $0.50 per 1,000 ads: export to CSV or JSON with Python, Node.js or curl

Collect public ads from the Meta Ad Library by keyword, advertiser Page ID or Ad Library link: ad text, headline, landing page, button, images and videos, dates, platforms and version count, plus spend and reach ranges where Meta publishes them. Not affiliated with Meta.

This repository has ready scripts for the [Facebook Ad Library Scraper | $0.50 per 1,000 ads](https://apify.com/v0iddo/facebook-ads-library-scraper) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/facebook-ads-library-scraper), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "searchTerms": [
    "nike"
  ],
  "country": "US",
  "activeStatus": "active",
  "maxItems": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `searchTerms` | array | Words to search the Ad Library for, for example a brand or product name. |
| `pageIds` | array | Numeric Facebook Page IDs of advertisers whose ads you want, one per line. |
| `adLibraryUrls` | array | Optional search links copied from the browser, such as https://www.facebook.com/ads/library/?active_status=active&ad_type=all&country=US&q=nike&search_type=k... |
| `country` | string | Where the ads were shown: a two-letter country code such as US, GB or DE, or ALL for every country. |
| `activeStatus` | string | Which ads to return: ads that are running now, ads that stopped, or both. |
| `adType` | string | Ad category in the Ad Library. |
| `mediaType` | string | Only ads that contain this kind of media. |
| `startDateMin` | string | Optional. Only ads that started running on or after this date, as YYYY-MM-DD. |
| `startDateMax` | string | Optional. Only ads that started running on or before this date, as YYYY-MM-DD. |
| `maxAdsPerSearch` | integer | The most ads to read for each search term, Page ID or link. |
| `maxItems` | integer | The run stops after this many result rows. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "adArchiveId": "1550534860016399",
  "adLibraryUrl": "https://www.facebook.com/ads/library/?id=1550534860016399",
  "pageId": "115939511520844",
  "pageName": "Perfect themes",
  "pageCategories": [
    "Topic"
  ],
  "pageLikeCount": 18619,
  "isActive": true,
  "startDate": "2026-05-19",
  "endDate": null,
  "platforms": [
    "facebook",
    "instagram",
    "audience_network",
    "messenger",
    "threads"
  ],
  "body": "🔥🍭Themepack is a FREE and personalized APP😍 various styles of Theme💎 、Weather💖、Photo😊、Clock、Calendar to pick any widget to dress up your home screen！",
  "title": "𝕊𝕦𝕡𝕖𝕣 𝕙𝕠𝕥 𝕨𝕚𝕕𝕘𝕖𝕥𝕤 𝕒𝕡𝕡 ！",
  "linkUrl": "http://play.google.com/store/apps/details?id=com.iconchanger.widget.theme.shortcut",
  "linkDescription": null,
  "ctaText": "Install now",
  "displayFormat": "IMAGE",
  "imageUrls": [
    "https://scontent.fpit1-1.fna.fbcdn.net/v/t39.35426-6/700927950_933331229664450_6487763311056032772_n.jpg?_nc_cat=111&ccb=1-7&_nc_sid=c53f8f&_nc_ohc=4A442v61YxYQ7kNvwGxzLwC&_nc_oc=AdoPuW_V0EaFc-xQEt6c10fzMZOU57v7Jab4RCAVHPGU6PCs2tZFcmQIjuUnGSlZHmw&_nc_zt=14&_nc_ht=scontent.fpit..."
  ],
  "videoUrls": [],
  "collationCount": null,
  "collationId": null,
  "currency": null,
  "impressionsText": null,
  "impressionsLower": null,
  "impressionsUpper": null,
  "spendLower": null,
  "spendUpper": null,
  "reachEstimate": null,
  "disclaimer": null,
  "targetedCountries": [],
  "demographics": [],
  "searchedFor": "nike",
  "searchCountry": "US"
}
```

Fields: `adArchiveId`, `adLibraryUrl`, `pageId`, `pageName`, `pageCategories`, `pageLikeCount`, `isActive`, `startDate`, `endDate`, `platforms`, `body`, `title`, `linkUrl`, `linkDescription`, `ctaText`, `displayFormat`, `imageUrls`, `videoUrls`, `collationCount`, `collationId`, `currency`, `impressionsText`, `impressionsLower`, `impressionsUpper`, `spendLower`, `spendUpper`, `reachEstimate`, `disclaimer`, `targetedCountries`, `demographics`, `searchedFor`, `searchCountry`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/facebook-ads-library-scraper').call({"searchTerms": ["nike"], "country": "US", "activeStatus": "active", "maxItems": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~facebook-ads-library-scraper/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"searchTerms": ["nike"], "country": "US", "activeStatus": "active", "maxItems": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$0.50 per 1,000 results**, nothing per run. `maxItems` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
