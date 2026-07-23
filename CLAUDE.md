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

### SCHEDULING & CALENDAR

**S1 — Google Calendar sync**  
Whenever an event is added to a day page (`days/dec-XX.html`), also add it as a Google Calendar event for **lee.kok.kurbat@gmail.com** with the same name, time (local), location, duration, and notes.

**S2 — Chronological ordering**  
Always order items on day pages chronologically by time. When inserting a new event, place it in time order, not just at the end.

**S3 — Sunset time**  
Add the local sunset time to every day page.

**S4 — Public holiday annotation**  
Annotate the top of each day page with a note if there is a public holiday at that location on that date.

**S5 — Party size**  
**Default: 3 people** for all asia2026 events, unless otherwise stated.  
(Travelers: Michael Lee, Uwen Kok, Carl Kurbat)  
Use the correct count in calendar events, reservation notes, and any count-dependent content.

**S6 — WhatsApp update on new events**  
Each time a new event or reservation is added, produce a WhatsApp-style plain-text summary that can be copy-pasted to notify others. Include: what was added, date/time, location, and any key details (reservation time, party size, etc.).

---

### MAPS & NAVIGATION

**M1 — Agenda route maps on day pages**  
Each day page should have a map showing the best route (by metro or on foot) for back-to-back events in that day's agenda. Add or update the map whenever events are added or reordered.

**M2 — Hotel pin on location map**  
The location map must include a separate colored pin for the hotel/accommodation in each city. Update whenever hotels are confirmed.

**M3 — Metro/MRT stations for events**  
When adding a new event or reservation, automatically identify the nearest metro/MRT/rail station(s) and add them to the metro scheduling feature on the relevant day page.

---

### RESTAURANTS & DINING

**R1 — Restaurant classification**  
For every new restaurant, verify it belongs on the correct city page (Tokyo / KL / Singapore) based on its actual location. Retrieve: confirmed address, operating hours, menu link, reservation link.

**R2 — Required rlinks**  
Every restaurant card must include (where available): Website, Menu (if distinct URL), Reserve, Michelin (with rating: 1 Star / Bib Gourmand / Green Star), Infatuation review. See rlinks section for format.

**R3 — Reservation color coding**  
Restaurants with confirmed reservations use `class="rest-card reserved"` with a distinct background. Unreserved use `class="rest-card not-reserved"`.

**R4 — Meal tracker at top of dining pages**  
Each city dining page must have a tracker at the top listing every day's breakfast, lunch, and dinner with checkboxes. Check off days with a confirmed meal plan. Mark travel days. Update the tracker each time a new reservation is added.

---

### WEATHER

**W1 — Weather panel per city**  
Automatically add (or update) a weather panel for each city where an event is added. This applies to all sites and all new cities as they are added to the itinerary.

---

### SITE FEATURES & CROSS-SITE RULES

**F1 — Mobile-first design**  
Test and design all pages for mobile friendliness. Any new feature must work on small screens. When in doubt, check at 375px width.

**F2 — Feature replication across all sites**  
When adding a non-location-specific feature (UI, functionality, layout, tooling) to any one site, replicate it automatically across all other active trip sites built from this template (currently: parislondon2026, asia2026). Do not wait to be asked.

**F3 — New site CLAUDE.md bootstrap**  
Each time a new trip site is created, copy the then-current CLAUDE.md into the new repo as its starting CLAUDE.md. Update only the site-specific sections (trip dates, cities, party size, hotels). All universal rules carry forward unchanged.

**F4 — Universal rules apply to all future sites**  
All rules in this Standing Rules section that are not explicitly marked as site-specific apply to every future trip site using this template.

**F5 — Activity planned status synced to agenda**  
Whenever a sight or activity is added to a day-page agenda, mark the corresponding card on the activities page as planned by changing `class="act-card"` to `class="act-card act-planned"`. If the activity does not yet exist on the activities page, add it first, then mark it planned. Whenever an agenda item is removed, revert its card back to `class="act-card"` (no green). This keeps the activities page as a live checklist of what is actually on the itinerary.

CSS implementation (in `assets/styles.css`):
```css
.act-card.act-planned {
  background: #E7EDD8;
  border-color: #BFCE9E;
}
```

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
