<p align="center">
  <img src="./assets/logo.svg" alt="wots.live logo" width="360">
</p>

<h3 align="center">wots.live API Documentation</h3>

<p align="center">
  Unofficial community documentation for the public <a href="https://api.livebeam.live">api.livebeam.live</a> endpoints used by <a href="https://wots.live">wots.live</a>.
</p>

<p align="center">
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-GPLv3-blue.svg" alt="License: GPL v3"></a>
  <img src="https://img.shields.io/badge/status-community--maintained-lightgrey.svg" alt="Status">
</p>

---

## Table of Contents

- [Overview](#overview)
- [Base URL](#base-url)
- [Endpoints](#endpoints)
  - [GET /api/top-vods](#get-apitop-vods)
  - [GET /api/recent-streams](#get-apirecent-streams)
  - [GET /api/predictions-enabled](#get-apipredictions-enabled)
  - [GET /api/pokes](#get-apipokes)
  - [GET /api/leaderboard/points](#get-apileaderboardpoints)
  - [GET /api/featured-streams](#get-apifeatured-streams)
  - [GET /api/directory](#get-apidirectory)
- [Response Envelope](#response-envelope)
- [Notes](#notes)
- [License](#license)

## Overview

This repository documents the public JSON API that backs [wots.live](https://wots.live/), a live-streaming platform. Endpoints are read-only (`GET`) and return stream metadata, VOD listings, leaderboard standings, and site feature flags. No authentication is required for the endpoints below.

## Base URL

```
https://api.livebeam.live
```

All paths below are relative to this base URL.

## Endpoints

### `GET /api/top-vods`

Returns the top-performing VOD (video on demand) recordings.

**Example request**

```
GET https://api.livebeam.live/api/top-vods
```

**Example response**

```json
{
  "success": true,
  "data": [
    {
      "streamer_name": "MrBased",
      "title": "FRIDAY MAY 22nd 🌹🌹🌹🌹🌹",
      "thumbnail_url": "https://image.mux.com/{playback_id}/thumbnail.jpg?width=400&height=225&fit_mode=smartcrop&time=10",
      "vod_playback_id": "ZxawmoFcCFEqyMMZMnxg1iqg9imiHU01cKJNNMCvTlco",
      "stream_id": 1126
    }
  ]
}
```

**Fields**

| Field | Type | Description |
|---|---|---|
| `streamer_name` | string | Display name of the streamer |
| `title` | string | Title of the stream/VOD |
| `thumbnail_url` | string | Mux-generated thumbnail image URL |
| `vod_playback_id` | string | Mux playback ID for the VOD |
| `stream_id` | integer | Internal numeric stream identifier |

---

### `GET /api/recent-streams`

Returns the most recently ended streams, including VOD and channel metadata.

**Query parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `limit` | integer | No | Maximum number of streams to return |

**Example request**

```
GET https://api.livebeam.live/api/recent-streams?limit=6
```

**Example response**

```json
{
  "success": true,
  "data": [
    {
      "stream_id": 1126,
      "channel_id": "4c9d0f62-cbd3-4f62-8e6d-eed0185e08c7",
      "title": "FRIDAY MAY 22nd 🌹🌹🌹🌹🌹",
      "description": "Description of my first stream",
      "thumbnail_url": "https://image.mux.com/{playback_id}/thumbnail.jpg?width=400&height=225&fit_mode=smartcrop&time=10",
      "duration": "VOD",
      "time_ago": "113d ago",
      "vod_playback_id": "ZxawmoFcCFEqyMMZMnxg1iqg9imiHU01cKJNNMCvTlco",
      "streamer_name": "MrBased",
      "streamer_pfp": "https://jcbvjakdtywqikciddqq.supabase.co/storage/v1/object/public/user-assets/pfp/{user_id}-{hash}.jpeg",
      "user_id": "849d85f9-10b1-4825-ade6-a717f8b992ed"
    }
  ]
}
```

**Fields**

| Field | Type | Description |
|---|---|---|
| `stream_id` | integer | Internal numeric stream identifier |
| `channel_id` | string (UUID) | Channel the stream belongs to |
| `title` | string | Stream title |
| `description` | string | Stream description |
| `thumbnail_url` | string | Mux-generated thumbnail image URL |
| `duration` | string | Stream status/type (e.g. `"VOD"`) |
| `time_ago` | string | Human-readable relative time since the stream ended |
| `vod_playback_id` | string | Mux playback ID for the VOD |
| `streamer_name` | string | Display name of the streamer |
| `streamer_pfp` | string \| null | URL to the streamer's profile picture |
| `user_id` | string (UUID) | Streamer's user ID |

---

### `GET /api/predictions-enabled`

Returns whether the predictions feature is currently enabled site-wide.

**Example request**

```
GET https://api.livebeam.live/api/predictions-enabled
```

**Example response**

```json
{
  "success": true,
  "data": {
    "predictions_enabled": false
  }
}
```

**Fields**

| Field | Type | Description |
|---|---|---|
| `predictions_enabled` | boolean | Whether the predictions feature is active |

---

### `GET /api/pokes`

Returns recent "poke" activity/notifications.

**Query parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `limit` | integer | No | Maximum number of pokes to return |

**Example request**

```
GET https://api.livebeam.live/api/pokes?limit=8
```

**Example response**

```json
{
  "success": true,
  "data": []
}
```

> `data` is an empty array when there are no active pokes.

---

### `GET /api/leaderboard/points`

Returns the site-wide points leaderboard, ranked highest first.

**Query parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `limit` | integer | No | Maximum number of leaderboard entries to return |

**Example request**

```
GET https://api.livebeam.live/api/leaderboard/points?limit=10
```

**Example response**

```json
{
  "success": true,
  "data": [
    {
      "rank": 1,
      "user_id": "8e89c31a-ea64-4906-a53a-8154f6a2000c",
      "username": "AyeSnuggs",
      "pfp_url": "https://jcbvjakdtywqikciddqq.supabase.co/storage/v1/object/public/user-assets/pfp/{user_id}-{hash}.jpg",
      "points": 147
    }
  ]
}
```

**Fields**

| Field | Type | Description |
|---|---|---|
| `rank` | integer | Position on the leaderboard (1 = highest points) |
| `user_id` | string (UUID) | Unique user identifier |
| `username` | string | User's display name |
| `pfp_url` | string \| null | URL to the user's profile picture |
| `points` | integer | Total accumulated points |

---

### `GET /api/featured-streams`

Returns streams currently marked as "featured".

**Query parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `limit` | integer | No | Maximum number of streams to return |

**Example request**

```
GET https://api.livebeam.live/api/featured-streams?limit=8
```

**Example response**

```json
{
  "success": true,
  "data": []
}
```

> `data` is an empty array when no streams are currently featured.

---

### `GET /api/directory`

Returns the full channel directory, split into `live` and `offline` channels. Each channel aggregates one or more linked platforms (e.g. wots, Kick, YouTube), and reports the currently active or most recent stream per platform.

**Example request**

```
GET https://api.livebeam.live/api/directory
```

**Example response**

```json
{
  "success": true,
  "data": {
    "live": [
      {
        "channel_id": "8983d6c6-740e-41c8-a5f9-3581279d82b0",
        "name": "ac7ionman",
        "display_name": "Ac7ionman",
        "pfp_url": "https://files.kick.com/images/user/1251175/profile_image/conversion/{hash}-fullsize.webp",
        "pinned": false,
        "order": 1,
        "external_only": true,
        "is_live": true,
        "primary_platform": "kick",
        "title": "MONKEY APP MARATHON CLICK JOIN NOW",
        "category": "Chat Roulette",
        "viewer_count": 7800,
        "thumbnail_url": null,
        "started_at": "2026-09-13T22:03:23+00:00",
        "last_live_at": "2026-09-14T01:27:55.672+00:00",
        "platforms": [
          {
            "platform": "kick",
            "slug": "ac7ionman",
            "is_live": true,
            "title": "MONKEY APP MARATHON CLICK JOIN NOW",
            "category": "Chat Roulette",
            "viewer_count": 7800,
            "thumbnail_url": null,
            "avatar_url": "https://files.kick.com/images/user/1251175/profile_image/conversion/{hash}-fullsize.webp",
            "stream_ref": "127178696",
            "started_at": "2026-09-13T22:03:23+00:00",
            "last_live_at": "2026-09-14T01:27:55.672+00:00",
            "external_url": "https://kick.com/ac7ionman"
          }
        ]
      }
    ],
    "offline": [
      {
        "channel_id": "4c9d0f62-cbd3-4f62-8e6d-eed0185e08c7",
        "name": "MrBased",
        "display_name": "MrBased",
        "pfp_url": "https://jcbvjakdtywqikciddqq.supabase.co/storage/v1/object/public/user-assets/pfp/{user_id}-{hash}.jpeg",
        "pinned": true,
        "order": 0,
        "external_only": false,
        "is_live": false,
        "primary_platform": null,
        "title": null,
        "category": null,
        "viewer_count": 0,
        "thumbnail_url": null,
        "started_at": null,
        "last_live_at": "2026-09-12T03:07:59.765+00:00",
        "platforms": [
          {
            "platform": "wots",
            "slug": "MrBased",
            "is_live": false,
            "title": null,
            "category": "irl",
            "viewer_count": 0,
            "thumbnail_url": null,
            "avatar_url": "https://jcbvjakdtywqikciddqq.supabase.co/storage/v1/object/public/user-assets/pfp/{user_id}-{hash}.jpeg",
            "stream_ref": null,
            "started_at": "2026-05-23T03:30:10.118+00:00",
            "last_live_at": "2026-05-23T03:35:21.632+00:00",
            "external_url": null
          },
          {
            "platform": "kick",
            "slug": "mrbasednyc",
            "is_live": false,
            "title": "MR BASED & @Gucciho HA!",
            "category": "IRL",
            "viewer_count": 0,
            "thumbnail_url": "https://stream.kick.com/thumbnails/livestream/{id}/thumb0/video_thumbnail/thumb0.jpg",
            "avatar_url": "https://files.kick.com/images/user/15731362/profile_image/conversion/{hash}-fullsize.webp",
            "stream_ref": null,
            "started_at": null,
            "last_live_at": "2026-09-12T03:07:59.765+00:00",
            "external_url": "https://kick.com/mrbasednyc"
          }
        ]
      }
    ]
  }
}
```

**Fields**

`data` contains two arrays, `live` and `offline`, of the same channel object shape:

| Field | Type | Description |
|---|---|---|
| `channel_id` | string (UUID) | Internal channel identifier |
| `name` | string | Internal/unique channel name |
| `display_name` | string | Display name shown to users |
| `pfp_url` | string \| null | Channel's profile picture URL |
| `pinned` | boolean | Whether the channel is pinned in the directory |
| `order` | integer | Sort order/priority within the directory |
| `external_only` | boolean | Whether the channel only streams on external platforms (no native `wots` presence) |
| `is_live` | boolean | Whether the channel is currently live on any platform |
| `primary_platform` | string \| null | Platform currently considered primary (e.g. `"kick"`), `null` when offline |
| `title` | string \| null | Current/most recent stream title |
| `category` | string \| null | Current/most recent stream category |
| `viewer_count` | integer | Current combined/primary-platform viewer count |
| `thumbnail_url` | string \| null | Current stream thumbnail, if available |
| `started_at` | string (ISO 8601) \| null | Start time of the current live stream |
| `last_live_at` | string (ISO 8601) \| null | Timestamp the channel was last live |
| `platforms` | array | Per-platform breakdown, see below |

Each entry in `platforms`:

| Field | Type | Description |
|---|---|---|
| `platform` | string | Platform identifier (e.g. `"wots"`, `"kick"`, `"youtube"`) |
| `slug` | string | Channel's slug/handle on that platform |
| `is_live` | boolean | Whether this specific platform is currently live |
| `title` | string \| null | Stream title on this platform |
| `category` | string \| null | Stream category on this platform |
| `viewer_count` | integer | Viewer count on this platform |
| `thumbnail_url` | string \| null | Stream thumbnail on this platform |
| `avatar_url` | string \| null | Profile picture on this platform |
| `stream_ref` | string \| null | Platform-specific stream/broadcast ID |
| `started_at` | string (ISO 8601) \| null | Stream start time on this platform |
| `last_live_at` | string (ISO 8601) \| null | Last time this platform was live |
| `external_url` | string \| null | Link to the channel on that external platform |

## Response Envelope

Every endpoint wraps its payload in the same top-level envelope:

```json
{
  "success": true,
  "data": {}
}
```

| Field | Type | Description |
|---|---|---|
| `success` | boolean | Whether the request succeeded |
| `data` | object \| array | The endpoint's payload (shape varies per endpoint) |

## Notes

- All endpoints observed so far are `GET` requests and appear to require no authentication.
- `thumbnail_url` and profile picture URLs are hosted on third-party CDNs (Mux and Supabase Storage) and are subject to change.
- Optional `limit` query parameters were observed on `recent-streams`, `pokes`, `leaderboard/points`, and `featured-streams`; behavior when omitted is not documented here and should be verified against the live API.
- `directory` aggregates multiple external platforms (`wots`, `kick`, `youtube` observed so far) per channel; a channel is considered `is_live` if any linked platform is currently live.
- This is unofficial, community-sourced documentation based on observed responses and is not guaranteed to be complete or stable. Field names, endpoints, and behavior may change without notice.

## License

This project is licensed under the **GNU General Public License v3.0**. See [LICENSE](./LICENSE) for the full text.

<p align="center">
  <sub>Not affiliated with or endorsed by wots.live or livebeam.live.</sub>
</p>
