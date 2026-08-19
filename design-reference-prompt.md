# Design Reference Prompt — Gujarati Government Exam Study Guide (single-file HTML)

Paste this whole prompt into a new chat (with the subject changed) whenever you want a new
topic — Reasoning, GK, Science, Current Affairs, English Grammar, Gujarati Grammar,
Geography, History, Computer Awareness, etc. — built in the **exact same look and structure**
as the existing "ભારતીય બંધારણ" and "ગણિત સૂત્રો" guides.

---

## 1. What to build

A single self-contained `.html` file (no external framework, one `<style>` block, one
`<script>` block) that is a searchable, sidebar-navigated study guide for **[SUBJECT NAME]**
for Gujarat competitive exams (PSI / GPSC / તલાટી / બિન સચિવાલય etc.), written entirely in
Gujarati with English terms in brackets where useful.

Structure per page:
- Left sidebar: brand header, fuzzy search box, category-grouped topic list (numbered),
  collapsible to a mini icon-rail on desktop, off-canvas drawer on mobile.
- Right content area: hero intro, then one card per topic, each with a numbered heading,
  an English subtitle, a small "art badge" (a one-line teaser fact), and a formatted body.
- Footer with guide name + exam tagline.
- Tooltip-on-hover for the sidebar's decorative mark icon.

## 2. Design tokens (CSS variables — reuse verbatim)

```css
:root {
  --ink: #1c2b24;
  --paper: #f6f2e8;
  --paper2: #efe8d8;
  --saffron: #c9622a;
  --saffron-dark: #a34b1c;
  --green-deep: #1f4d3a;
  --green-line: #2f6b50;
  --gold: #b48a2e;
  --card: #fffdf7;
  --line: #ddd3ba;
  --muted: #6b6152;
  --shadow: 0 8px 24px rgba(28, 43, 36, 0.08);
  --radius: 10px;
}
```
Fonts: `Noto Sans Gujarati` (400/500/600/700/800) for body/UI, `Noto Serif Gujarati`
(500/600/700) for topic headings, `Noto Sans` as Latin fallback. Import from Google Fonts.

Sidebar: dark green gradient (`--green-deep` → `#163b2c`), gold right border (4px),
white-ish text. Topic cards: `--card` background, 1px `--line` border, `--radius` corners,
`--shadow` drop shadow, 24–26px padding, 20px gap between cards.

## 3. Content readability rules (non-negotiable — apply to every topic)

- **Formulas / key one-liners get their own block**, never buried inside a bullet or
  paragraph:
  ```html
  <div class="formula"><span class="flabel">optional short label</span>ACTUAL FORMULA</div>
  ```
  Multiple related formulas go inside a `.formula-group` (flex column, 12px gap) so each
  sits on its own line with clear space above/below/between. Formula box style: soft
  gradient background, 4px saffron left border, centered text, 13–20px radius, horizontal
  scroll (not wrap) on overflow so nothing breaks awkwardly on mobile.
- **Tables** are wrapped in `<div class="table-scroll"><table>...</table></div>` so wide
  tables scroll horizontally on small screens instead of squeezing.
- **Sub-headings** (`<span class="sub">`) get a bottom border and generous top margin
  (~24–28px) to visually separate sections within a topic.
- **Notes / exam tips** use `<div class="note">` — soft gold-tinted callout box, left
  border accent, smaller font, always placed after the content it clarifies.
- Explanatory prose/lists stay left-aligned; formulas stay centered. Line-height ≥ 1.8 for
  body text so Gujarati conjuncts stay legible. Never mix a formula and its explanation in
  the same line — explanation first (paragraph or bullet), formula in its own box.
- Mobile: sidebar collapses to a hamburger-triggered overlay drawer; topic-body font drops
  slightly (~14.5px); formula/table padding tightens but the same box structure is kept.

## 4. Content data schema (JS)

All topic content lives in one JS array at the top of the `<script>` block, rendered by
existing DOM-building logic (sidebar TOC + topic cards + fuzzy search). Keep this exact
shape:

```js
const DATA = [
  {
    cat: "શ્રેણીનું નામ (ગુજરાતીમાં)",
    items: [
      {
        t: "ટોપિકનું શીર્ષક (ગુજરાતી)",
        en: "English title",
        art: "ટૂંકું ટીઝર ટેક્સ્ટ / બેજ",
        body: `
<p>પરિચય ફકરો.</p>
<span class="sub">પેટા-મથાળું</span>
<ul><li>મુદ્દો એક</li><li>મુદ્દો બે</li></ul>
<div class="formula-group">
  <div class="formula">સૂત્ર = અહીં લખો</div>
</div>
<div class="table-scroll"><table><tr><th>...</th></tr><tr><td>...</td></tr></table></div>
<div class="note">GPSC ટીપ: ...</div>
`,
      },
    ],
  },
];
```

- One category → several items (topics). Each item's `body` is a JS template literal —
  never use a raw backtick or `${` inside it.
- Use `<mark>` for a single key term worth highlighting inline, sparingly.
- Keep every topic self-contained and exam-focused: definitions, the formula/rule, a
  worked convention or table, and a "GPSC ટીપ" note flagging common traps or frequently
  asked twists — mirror the tone already used in the Constitution and Math Formulas guides.

## 5. What to hand back

- One complete `.html` file, ready to open directly in a browser — same sidebar/search/
  responsive JS as the existing guides, only `DATA`, the `<title>`, the sidebar `<h1>`/`<p>`
  brand text, the hero heading/description, and the footer text changed for the new subject.
- Do not introduce new CSS classes beyond `.formula`, `.formula-group`, `.table-scroll`,
  `.sub`, `.note`, `mark` unless a genuinely new content shape requires it — and if so, follow
  the same spacing/border/radius language as the existing tokens.

---

**Example use:** replace `[SUBJECT NAME]` above with "રિઝનિંગ (Reasoning)" or
"સામાન્ય જ્ઞાન (GK)" or "અંગ્રેજી વ્યાકરણ (English Grammar)" etc., paste this whole prompt,
and the same design will carry over automatically.
