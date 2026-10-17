# A House Is Not A Home as a single page - session transcript

Transcript of the A House Is Not A Home part of a Claude Code session on 2026-10-13. Earlier in the same session, Tome, Word Perhect and Adelaide were worked on; those parts are in their repositories' `_sessions` folders. Tool calls are shown as one-line notes; their output is omitted. Message times weren't recorded, so only the date is given. Claude's longer replies are shortened where they repeat what the summary records.

---

## User - 2026-10-13

Can you now do the same type of thing with repos/ahouse. This project starts with the fr.html file. Ideally I'd like this to use flexbox to place images centrally, scale up for large screens and be driven from a single html page with images preloaded and SVG Image Maps to make them responsive

## Claude

> *Bash: Survey the ahouse repo and read fr.html*

Branch is `redux`. Reading the body of every page and all the `.map` files. Most pages repeat the same head and analytics, so I'm stripping those out:

> *Bash: Read the body of every other page*

Every page's links are in its client-side `<map>`. I'm checking whether the `.map` files (server-side NCSA image maps, the fallback for browsers of that time) say anything different, and whether the animated GIFs loop or play once.

> *Bash: Read server-side maps and inspect GIF animation*

All the animated GIFs loop forever, so the animations need no special handling in a single page. The server-side `.map` files mostly agree with the pages, but not entirely. In `bra.map` the left area goes to `la.html` where the page sends it to `had.html`, and `hab.map` lacks the page's link back to `fr.html`. Browsers always used the client-side maps in the pages, so I'll follow those.

> *Bash: Check image tools available*

> *Bash: Convert sample scenes to PNG for viewing*

The drawings are flat-colour illustrations, so normal smooth scaling will look fine when they're enlarged.

Here's the plan for `index.html`:

- **Each page becomes an inline SVG** with a `650 500` viewBox. Its images are SVG `<image>`s at their original positions, and its links are SVG `<a>`s: rectangles over the hotspots, or the images themselves on the pages built from several slices.
- **Navigation uses the old page names as `#fragments`**, and CSS `:target` shows the current scene. `#fr` is the default.
- **Layout:** flexbox centres the SVG, which grows to fill the window at the 650:500 proportions.
- **Preloading:** a small script loads every GIF up front.

I'll generate the scenes from the old pages with a script rather than retyping 40 hotspot coordinates by hand.

> *Bash: Generate SVG scenes from the old pages*

The generated scenes match the old pages. I also noticed a quirk of the original, which I'll keep: `tbd` shows `tba.gif` with `tbb`'s hotspots, and its middle hotspot links to itself.

> *Bash: Write index.html with the generated scenes*

Testing in Chrome. For every scene, the test clicks the centre of each original hotspot, mapped onto the scaled-up drawing, and checks it lands on the scene the old page linked to. It runs at a large desktop size and at phone size, and also checks the front-door rollover, background colours, the back button and that every image loads.

> *Bash: Test every hotspot at two screen sizes*

All 53 links land on the right scene at both sizes. The front door lights up on hover, the back button works, the backgrounds are right, nothing scrolls, and there are no errors. 30 of the 31 GIFs load; the one that doesn't is `ba4.gif`, which was only a Netscape `lowsrc` placeholder.

One correction: my CSS comment guessed the lighter-background scenes were "the landing and bathroom", but `sia` is clearly the sitting room. I'll make the comment neutral, then remove the old pages and server-side maps.

> *Bash: Fix comment, remove old pages and maps*

> *Bash: Stop the test server*

"A house is not a home" now runs from a single page, `index.html`, and the 21 old pages and 15 `.map` files are removed. I tested it in Chrome at 1600×1000 and at phone width: all 53 links land on the same scene as in the original.

- **One page, one SVG per scene**, named after the old pages (`fr.html` is `#fr`). Hotspots are invisible SVG rectangles in the drawing's own coordinates, so they scale exactly with it.
- **Generated from the old pages by a script**, so the coordinates and links are the originals.
- **Navigation** by `#fragment` links and CSS `:target`, so the back button and links to a scene work.
- **Layout:** flexbox centres the scene, which fills the window at 650:500.
- **Preloading:** all the GIFs load when the page opens.
- **Kept:** the front door that opens under the pointer, the looping animations, the lighter background on `sia` and `tba`, and the `tbd` quirk. **Added:** keyboard focus on hotspots.
- **Worth knowing:** the server-side `.map` files had drifted from the pages, and the pages' maps were followed. `ba4.gif` isn't used. Links to the old pages will now get a 404.

## User - 2026-10-13

Yes, please, again in a _sessions directory

## Claude

> *Write: _sessions/2026-10-13-summary.md*

> *Write: _sessions/2026-10-13-transcript.md*

Added the summary and transcript in `ahouse/_sessions/`.
