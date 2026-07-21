---
name: airbnb-calendar
description: "Read an Airbnb listing calendar — day-by-day availability over a date window. Use when a user asks whether an Airbnb is open on specific dates. Powered by StayingAPI."
version: "1.0.0"
license: MIT-0
author: StayingAPI
homepage: https://stayingapi.com
repository: https://github.com/stayingapi/airbnb-skills
user-invocable: true
compatibility: Requires internet access to reach api.stayingapi.com. No additional runtimes or dependencies needed.
required_environment_variables:
  - name: STAYINGAPI_KEY
    prompt: Your StayingAPI key (starts with stay_)
    help: Free key at https://stayingapi.com/signup — no card. A stay_test_ sandbox key returns fixtures at zero cost.
    required_for: all API requests
tags: ["airbnb", "airbnb-calendar", "availability", "calendar", "travel"]
metadata: {"openclaw":{"emoji":"📆","requires":{"env":["STAYINGAPI_KEY"]},"primaryEnv":"STAYINGAPI_KEY","homepage":"https://stayingapi.com"},"hermes":{"tags":["airbnb","airbnb-calendar","availability","calendar","travel"],"category":"travel"}}
---

# Airbnb calendar

The Airbnb calendar as data: day-by-day availability for a known listing over any window. (A keyword-friendly alias of the availability skill.)

## Setup

If `$STAYINGAPI_KEY` is not set, read [references/auth-setup.md](references/auth-setup.md) and follow it to get and store the key. A `stay_test_` sandbox key works for evaluation at zero cost.

## When to use this skill

**DO use when the user asks:**

- "Show the Airbnb calendar for this listing"
- "Which August nights are open on this Airbnb?"

**Do NOT use when:**

- You do not have a listing id — search first

## Required headers

Every request needs:

- **Authorization:** `Bearer $STAYINGAPI_KEY`
- **User-Agent:** your agent's name (e.g. `ClaudeCode/1.0`).

Base URL: `https://api.stayingapi.com/v1`.

## Tools

### `GET /v1/availability`

Get day-by-day availability for a known listing (or a batch of listings) on one platform over a date window. Each day reports whether it is available, its minimum-night requirement, and whether check-in / check-out / booking is allowed.

Key parameters:
- `platform` — Single platform (not a fan-out endpoint).
- `listingId` — A single listing id on platform.
- `listingIds[]` — A batch of listing ids on platform.
- `url` — Full listing URL (alternative to an id).
- `startDate` — YYYY-MM-DD; not in the past.
- `endDate` — After startDate; window ≤ 365 days.


## MCP (no key pasted into the agent)

On an MCP-capable runtime, connect `https://mcp.stayingapi.com/mcp` (OAuth 2.1 + PKCE) and use: `check_availability`.

## The cross-OTA advantage

StayingAPI is **cross-platform**: Airbnb data comes back in the *same unified schema* as Airbnb, Booking.com, Vrbo and Google Hotels, and the price-compare tool returns a computed **min** and **median** across every OTA — something a single-platform wrapper can't do.

## Async & partial failures

A live call that has to scrape returns `202` + a `jobId`; poll `GET /v1/jobs/{jobId}` (free) until `data.status` is `completed`, then read `data.result`. On a fan-out, check `meta.partial` and `meta.platformResults[]`.

## Credits

Number-free by design — **failed, empty and blocked calls are never billed**, and `stay_test_` sandbox calls are always free. Current costs: <https://stayingapi.com/pricing> · full contract: <https://api.stayingapi.com/openapi.json>.

## Trademark

StayingAPI is an independent service and is not affiliated with, endorsed by, or sponsored by Airbnb. Airbnb is a trademark of its respective owner.

---

**Get your free key → https://stayingapi.com/signup** · Docs: https://stayingapi.com/docs
