# Piggy World

Frankie's games. She is 9, she is the designer, and this is her project — you are the helper.

Live at **https://frankiespiggies.com** (also `piggies-beta.vercel.app`).

## Who you're working with

Frankie invents the games and decides how they should feel. She has no coding
background and doesn't need one. Ask her what she wants, build it, show her.
Explain things at a 9-year-old level when there's a chance to teach something —
but keep it fun. This is a game, not homework.

There is a global `frankie` skill for lessons, quest cards, and translating her
playtest feedback into buildable changes. Use it when she's actually in the room.

### Message formatting

She won't read walls of technical text. Every response:

1. Brief technical notes first — for Mike, or just to record what changed.
2. A `---` divider.
3. A `## Hey Frankie!` section at the end, written to her. Big simple language,
   3–5 sentences, and if you're asking something make it a single clear question.

## What's here

One hub plus seven playable games. Every file is standalone HTML with its CSS and
JS inline, emoji for all graphics, no build step and no dependencies.

| File | Game |
| --- | --- |
| `index.html` | **Piggy World hub** — the front door, links to everything |
| `classic.html` | Piggies to Bacon (Classic) — her very first game |
| `disaster-piggies.html` | Disaster Piggies — the big one: barn, shop, disasters |
| `piggy-catch.html` | Piggy Catch — catch falling piggies in a basket |
| `oink-memory.html` | Oink Memory — matching pairs |
| `pig-racer.html` | Pig Racer |
| `pig-tower.html` | Pig Tower — stack the blocks |
| `piggy-painter.html` | Piggy Painter |

The hub's "coming soon" grid is a wish list of Frankie's ideas. Cards there are
placeholders until a real file exists; flip one to a live PLAY link when you build it.

`kitchen-screenshot.png` and the `pig-squeal*.mp3` files are game assets.

## Running it

```bash
open index.html          # opens the hub
```

To test as it actually ships (relative links, audio, real URLs):

```bash
python3 -m http.server 8899   # then visit http://localhost:8899/
```

## Phones are the target

Mike's bar, set 2026-07-28: Frankie's friends play on phones, so **a game that
needs a mouse or a keyboard is broken**, not "desktop-only."

Practical version: `click` fires on tap, so click-driven games are fine. But
`mousemove` never fires on a phone and there are no arrow keys — anything that
steers, drags, or aims needs `touchstart`/`touchmove` too. Piggy Catch shipped
mouse-only for months and the basket simply sat frozen while the player lost.

If you add touch handlers with `preventDefault`, guard them on the game's
"is it running" state. Otherwise they swallow taps on the START and AGAIN
buttons and the game becomes unstartable on the very devices you were fixing.

## Deploying

Vercel project `piggies` (team `mikeyoung304-gmailcoms-projects`).

```bash
npx vercel --prod
```

Registrar is Porkbun; DNS is delegated to Vercel (`ns1`/`ns2.vercel-dns.com`),
so Vercel handles records and the TLS certificate. The domain auto-renews at
$11.08/year.

After deploying, load the page in a browser and look at it. Mike should never be
the first one to see a blank page.

## Gotchas that cost real time

**Vercel serves static files before it applies rewrites.** A `vercel.json`
rewrite of `/` → `/hub.html` silently does nothing while an `index.html` exists —
the file wins and you get no error. That's why the hub *is* `index.html` and the
original game moved to `disaster-piggies.html`. Don't reintroduce a rewrite to
solve routing here; rename instead.

**An apex ALIAS record beats an A record at Porkbun.** New domains ship with
`ALIAS @ → uixie.porkbun.com` pointing at the parking page, so adding
`A @ → 76.76.21.21` appears to succeed and changes nothing. Delegating the
nameservers to Vercel sidesteps it entirely.

**Porkbun's API is opt-in per domain**, toggled in the Details panel of Domain
Management. Until it's on, every API call returns `not opted in to API access`.
The account-wide "Opt In All Domains" switch is deliberately off — it would
expose the production domains too, so leave it alone and opt in per domain.

## History

Started March 2026 as a single game. Spent April–July 2026 misfiled under
`_archive/2026-04-23-dead/` while still being actively worked on; moved to
`~/CODING/frankies-piggies/` on 2026-07-28, when three finished games were
committed, the original game was recovered from git history, and the domain
went live.
