# Phanh Portfolio: design review

This review is based on the Figma file *Phanh Portfolio*, Page 1, which has 6 screens: Home, Works, Writings, Fahrenheit 451 article, Notes, and Findings article. Measurements come from each 1280 px screen, taken from its left edge.

## 1. Alignment

| # | Issue | Where | Fix |
|---|---|---|---|
| A1 | **Left margin changes from screen to screen.** The site name starts at 72, 84, 82, 72, 95 and 85 px. | Every header | Use one 72 px margin. A 12-column layout grid on each frame makes this easy to keep. |
| A2 | **The right margin of the tabs changes too.** It is 140 px on Home, 55 on Works, 158 on Writings, 75 on the articles and 80 on Notes, so the nav moves when you switch pages. | Tabs | Right-align the tabs to the same 72 px margin on every screen. |
| A3 | **The site name and the content below it don't line up.** On Home the name is at 72 px, but the avatar, "Location" and "Works" are at 62–63 px. | Home | Put everything on the 72 px edge. |
| A4 | **"Location" and "Socials" headings don't share a baseline.** They sit at y −819 and y −828, a 9 px difference. The "Taipei, Taiwan" text and the social links are also off by 4 px. | Home | Put both columns in one auto-layout row with top alignment. |
| A5 | **Nested items use different indents.** Descriptions start at −541, −539, −538 and −537. On Works, the link icon is at 1004 but the titles are at 1032 and 1038, and the years are at 1000, 1003 and 1004. | Home, Works | Pick 3 levels (for example 0 / 16 / 56) and give the icon a fixed 12 px gap. The HTML already does this. |
| A6 | **Vertical spacing is uneven.** On Works there is about 260 px of empty space between the 2024 item and "Personal Projects". Articles are spaced about 165 px apart on Writings and 56 px apart on Home. | Works, Writings | Use a spacing scale: 8 / 16 / 24 / 40 / 64 / 120. |
| A7 | **Some underlines are misplaced.** A tab underline sits under part of the site name ("Thi Phuong"), and "CV" is underlined on every screen, so it looks selected. | All headers | Use the tab's active state only on the current page. Don't reuse a Tab instance for the site name. |
| A8 | **Some layers are duplicated.** The DailyBite title and the GitHub icon on Works are stacked twice. The header on the Findings screen is duplicated (Rectangle 7 and Rectangle 8). | Works, Findings | Delete the extra layers. Otherwise they cause trouble during handoff. |
| A9 | **The screens are rectangles, not frames.** All the layers sit loose on the canvas, with no auto layout, so the design can't adapt to other sizes. | All | Turn each rectangle into a Frame (Desktop 1280) with auto layout. Add Tablet (768) and Mobile (390) frames. |

## 2. Flow and content

- **Placeholder text is still in the design:** "Subtitle", "Text Small", "Description", "Subheading", "[Github Link]" and "Abstract of the work", plus some empty text layers. Replace them with real content before you ship.
- **One description is under the wrong project.** On the Works page, the Taiwan description appears under the Sentiment Analysis project.
- **Not every page is designed.** The CV page and the article page for "The Burnout Society" have no screen. The HTML includes stand-in pages so the links work. Choose whether CV is its own page or a PDF download.
- **Links look different across pages.** Work titles are underlined, but writing titles are not. Only one project has a "View My Work →" link. Give every project the same set of links: *Code · Live · Paper/Write-up*.
- **Article pages have no way back.** They also have no date, reading time or next/previous links. The breadcrumb link ("/ Writings") helps, and the HTML already makes it clickable.
- **Writings are grouped only under 2026**, while Works has a year on each item. Add dates to each writing, or group both pages the same way.
- **Home shows a Works preview but no Writings preview.** Add 2–3 recent writings with a "See all writings" link.
- **Contact:** the email address wasn't a link in the design. It's a `mailto:` link in the HTML. Consider adding a clear "Get in touch" button.
- **Notes reads like an About / Now page.** Consider renaming the tab so visitors know what it holds.
- **The avatar is the stock photo from the Simple Design System.** Replace it with your own photo.

## 3. Content storage

Right now each project and writing is typed separately on each screen. Home and Works repeat the same projects, and some text layers are copies of each other. Every edit has to be made in several places.

A suggested setup:

```
content/
  projects.json         ← one entry per project
  writings/
    fahrenheit-451.md   ← front matter + body
    the-burnout-society.md
    taiwan-digital-use.md
assets/
  images/<slug>/...
```

```json
{
  "slug": "reddit-sequel-sentiment",
  "title": "Sentiment Analysis of Popular Sequels on Reddit Comments",
  "year": 2025,
  "type": "group",
  "collaborators": ["Ewald Seelich", "Maelys Lafaurie", "Phuong Anh", "Sora Schultz", "Lillian Hsiao"],
  "summary": "...",
  "links": { "code": "https://github.com/...", "live": null, "writeup": "/writings/..." },
  "featured": true
}
```

- A static site generator such as **Eleventy** or **Astro** builds Home, Works and Writings from these files. `featured: true` decides what shows on Home. It can be hosted for free on GitHub Pages or Netlify.
- In Figma, turn *Project item* and *Writing item* into components with properties for title, year, authors and links. Then the design follows the same structure as the data.

## 4. What the HTML already does differently

- Sets one 72 px margin, one set of indents and one spacing scale (A1–A6).
- Right-aligns the tabs and shows the active tab on the current page only (A2, A7).
- Makes the email a `mailto:` link and the breadcrumb and site name clickable.
- Adapts to screen size: at ≤1024 px the layout tightens, and at ≤760 px it becomes a single column, the tabs move under the name and the type gets smaller.
- Keeps the placeholder text as it appears in Figma so you can compare the two side by side. Each placeholder is marked `<!-- TODO -->`.
