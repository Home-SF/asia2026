# Tokyo · KL · Singapore 2026–27 — Site Maintenance Guide

## Overview
GitHub Pages static site. Repo: https://github.com/Home-SF/asia2026  
Trip: Dec 19, 2026 – Jan 1, 2027  
Travelers: Michael Lee · Uwen Kok · Carl Kurbat (3 people)

### Itinerary
| City | Dates |
|------|-------|
| Tokyo | Dec 20–21 |
| Kuala Lumpur | Dec 22–27 |
| Singapore | Dec 28–31 |
| Return SFO | Jan 1 |

Hotels: TBD for all cities (update when confirmed)

---

## Key Files
| File | Contents |
|------|----------|
| `index.html` | Home — day-by-day grid with city sections |
| `restaurants-tokyo.html` | Tokyo restaurant list |
| `restaurants-kl.html` | Kuala Lumpur restaurant list |
| `restaurants-singapore.html` | Singapore restaurant list |
| `activities-tokyo.html` | Tokyo sights/activities |
| `activities-kl.html` | KL sights/activities |
| `activities-singapore.html` | Singapore sights/activities |
| `location-map.html` | GPS track map (per-person location upload) |
| `weather.html` | Weather page |
| `days/dec-19.html` … `days/jan-01.html` | Per-day itinerary pages |
| `assets/styles.css` | Shared styles |

**Note:** There is no separate dining map (unlike parislondon2026). The map page is a GPS-track viewer only.

---

## Current Card Counts (as of July 2026)
All restaurant and activity pages are **empty** — no cards added yet.

Next restaurant number: **rest-1** for each city (numbering restarts per city page)  
Next activity number: n/a (act-cards do not use IDs)

---

## Restaurant Card HTML Format

```html
<div class="rest-card not-reserved" id="rest-N">
<div class="rest-card-head">
<div class="rest-title"><span class="rest-num">N</span><h3>NAME</h3></div>
<span class="rest-status not-reserved">No reservation yet</span>
</div>
<div class="rest-addr">FULL ADDRESS<span class="rest-neighborhood">NEIGHBORHOOD</span></div>
<div class="rest-hours">HOURS</div>


<div class="rlinks">LINKS</div>
</div>
```

Note the two blank lines between `rest-hours` and `rlinks` — preserve these.

Reserved cards use `class="rest-card reserved"` and `class="rest-status reserved">Reservation confirmed</span>`.

---

## Activity Card HTML Format

```html
<div class="act-card">
<h3>NAME</h3>
<div class="rest-addr">ADDRESS</div>
<div class="rest-hours">HOURS</div>
<div class="act-fee">FEE</div>
<div class="act-fact"><span class="act-fact-label">LABEL</span> FACT</div>
<a href="URL" target="_blank" rel="noopener" class="act-website">Website &rarr;</a>
</div>
```

---

## Insertion Point

### First card on a page (removing the empty placeholder)
Pages start with:
```html
<div class="wrap"><div class="rest-list"><div class="empty-day">No restaurants added yet.</div></div></div>
```
Replace the entire inner content, keeping the outer `wrap` and `rest-list` divs:
```python
content = content.replace(
    '<div class="empty-day">No restaurants added yet.</div></div></div>',
    'CARDS_HTML</div></div>'
)
```
Same pattern for activities (replace `act-list` / `No activities added yet.`).

### Subsequent cards
Insert immediately before:
```
</div>
</div>
<footer>
```
Use `content.replace(marker, new_html + '\n' + marker, 1)`.

---

## rlinks Convention

**Order:** Website → Menu → Reserve → Michelin → Infatuation → muted notes

```html
<a href="URL" target="_blank" rel="noopener">Website</a>
<a href="MENU_URL" target="_blank" rel="noopener">Menu</a>
<a href="RESERVE_URL" target="_blank" rel="noopener">Reserve</a>
<a class="rlink-michelin" href="MICHELIN_URL" target="_blank" rel="noopener">Michelin &middot; 1 Star</a>
<a class="rlink-infatuation" href="INFATUATION_URL" target="_blank" rel="noopener">Infatuation</a>
<span class="rlink-muted">NOTE</span>
```

**Michelin suffix options:** `Michelin &middot; 1 Star`, `Michelin &middot; Bib Gourmand`, `Michelin &middot; Green Star`, or plain `Michelin`

**Menu link rules:**
- Only include if there is a dedicated menu page (different URL from website)
- Omit Menu link for walk-in spots / markets / street food where no dedicated menu page exists
- If Website and Menu are the same URL, use only Website

**Muted notes when applicable:**
- `Walk-in only — no reservations`
- `No official website — call ahead: +XX X XXXX XXXX`
- `Not Michelin-listed`
- `Not reviewed by Infatuation`

---

## Workflow for Adding New Entries

1. Classify: restaurant/café/food stop → restaurants page; museum/park/sight → activities page
2. Research: confirmed address, hours, website, menu URL, Michelin listing, Infatuation review
3. Add to the appropriate city HTML page using the card formats above
4. **No dining map to update** (unlike parislondon2026)
5. Add a Google Calendar event for lee.kok.kurbat@gmail.com (see Standing Rules)
6. Commit and push

---

## Standing Rules

### 1. Party size
**Default: 3 people** for all asia2026 events, unless otherwise stated.  
(Travelers: Michael Lee, Uwen Kok, Carl Kurbat)

Use the correct party size when adding calendar events, reservation notes, or any count-dependent content.

### 2. Google Calendar sync
Whenever an event is added to a **day page** (`days/dec-XX.html`), the same information must also be added as a Google Calendar event for **lee.kok.kurbat@gmail.com**.  
Fields to populate: name, time (local), location, duration, any extra notes.

### 3. Cross-site feature parity
Any feature, UI component, or structural improvement added to parislondon2026 must also be applied here (and vice versa), as both sites share the same template. This includes new page types, map improvements, style changes, and workflow scripts.

---

## Commit & Push

The sandbox cannot push to GitHub (HTTPS auth not available). After making changes:
```bash
cd /path/to/asia2026
git add -A
git commit -m "Description"
# Then tell the user to run: git push origin main
```

---

## Common Mistakes to Avoid

1. **Wrong addresses**: Always verify addresses via web search — do not rely on memory or training data.
2. **Duplicate Menu links**: Don't add a Menu link if it's the same URL as the Website link.
3. **No dining map**: This site has no `restaurants-map.html`. Don't try to update one.
4. **HTML entities in any JSON attributes**: Use `\uXXXX` escapes for non-ASCII in JSON; plain UTF-8 in HTML content.
5. **Numbering per city**: Each city's restaurant list starts at rest-1 independently — do not carry numbers across cities.
6. **rlinks order**: Always Website → Menu → Reserve → Michelin → Infatuation → muted.
7. **Empty-state first insertion**: The first card added to any page must replace the `empty-day` placeholder div, not just append before `<footer>`.
