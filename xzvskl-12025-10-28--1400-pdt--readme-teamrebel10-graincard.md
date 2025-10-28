# graincard - 80×110 monospace teaching cards

**team:** teamrebel10 (capricorn ♑ / X. wheel of fortune - cycles & transformation)  
**grainbranch:** `12025-10-28--1130-PDT--moon-uttaradha-asc-arie23-sun-12h--teamrebel10`  
**status:** ⚠️ **work in progress** - migrating to org structure  
**voice:** glow g2 (patient teacher, asks questions, hand-holds)

---

> **🎴 what if knowledge came in perfectly-sized cards?**

this is **teamrebel10's graincard** - a system for creating 80×110 monospace teaching cards with beautiful ASCII art:

* **80 characters wide**: classic terminal width, readable everywhere
* **110 lines tall**: extended depth for comprehensive content
* **ASCII box borders**: beautiful typography with box-drawing characters
* **grainorder IDs**: 6-char unique identifiers (1,235,520 permutations!)
* **grainbook collections**: cards organized into numbered decks

**graincard** makes your documentation beautiful, portable, and self-contained. like flash cards meets man pages meets tarot! 🎴

does this intrigue you? let me show you what's inside... 🌾

---

## 📁 current modules (newest first!)

**`graincard.scm`** (1130 pdt 10-28) - **card generation + validation** 🎴  
→ [read it here](https://github.com/teamrebel10/graincard/blob/12025-10-28--1130-PDT--moon-uttaradha-asc-arie23-sun-12h--teamrebel10/graincard.scm)

* 426 lines of steel magic
* text wrapping (preserves words, handles paragraphs)
* ASCII box generation (perfect 80×110 format)
* validation (checks all 116 lines, borders, width)
* CLI interface (create + validate commands)
* **usage**: `steel graincard.scm create xbdghj "title" "content"`

---

## 🎡 what is team 10?

**capricorn ♑** (cardinal earth) - ambitious, structured, transformative  
**wheel of fortune X** (tarot) - cycles, karma, turning points, destiny

_the wheel reminds us: what goes around comes around, and every ending is a new beginning, right?_ 🎡⚡

---

## 🎯 what graincard does

### 80×110 format specification

every graincard follows this exact structure:

```
Line 1-4:   Header (title, file path, live URL)
Line 5:     Opening ``` code fence
Line 6-7:   Box top border (┌──...──┐)
Line 8-108: Content area (101 lines)
Line 109:   Middle divider (├──...──┤)
Line 110-113: Footer metadata (grainbook, card #, "now == next + 1")
Line 114:   Box bottom border (└──...──┘)
Line 115:   Closing ``` code fence
Total: 116 lines
```

**why 80×110?**
- **80 chars**: the classic terminal width since the 1960s. familiar. readable.
- **110 lines**: extended from standard 80×25 to give room for comprehensive content
- **ASCII art**: box-drawing characters (┌┐└┘├┤─│) look beautiful in monospace
- **self-contained**: each card is a complete learning unit

### intelligent text wrapping

graincard wraps content to 78 chars (80 - 2 for borders):
- preserves words (no mid-word breaks)
- handles multiple paragraphs
- respects blank lines
- perfect for long-form teaching content

example:
```steel
(wrap-text "This is a very long line that needs to be wrapped to fit within the 78-character content width of the graincard box format..." 78)
;; → splits into multiple lines at word boundaries
```

### grainorder unique IDs

every graincard has a **grainorder** - a 6-character unique identifier:
- alphabet: `xbdghjklmnsvz` (13 consonants, no vowels)
- no repeating characters
- 13P6 = 1,235,520 possible combinations!
- example: `xbdghj`, `zvsnml`, `hklmns`

grainorder enables:
- **chronological sorting** (newest = smallest alphabet when head-inserting)
- **unique identification** (no UUID bloat)
- **collision-free** (1.2 million possibilities)
- **human-readable** (pronounceable 6-char codes)

### grainbook collections

graincards are organized into **grainbooks** (like decks of cards):
- each card knows its position (e.g., "Card 42 of 1,235,520")
- grainbooks can be thematic (e.g., "intro to graintime")
- cards can link to prev/next (like pages in a book)
- footer shows grainbook name (e.g., "ember harvest 🎃")

### validation

before saving, graincard validates:
- total line count (must be 116)
- opening/closing code fences (```)
- top border (┌) and bottom border (└)
- every line width (exactly 80 chars)
- box content (exactly 110 lines)

if validation fails, you get helpful error messages! ✨

---

## 🛠️ usage

### quick start (create a card)

```bash
# basic usage
steel graincard.scm create xbdghj "introduction to graintime" "Graintime is a system for encoding astronomical data into git branch names. It includes nakshatra (moon position), ascendant (rising sign), and sun's house..."

# this creates: xbdghj-graincard.md

# validate a card
steel graincard.scm validate xbdghj-graincard.md

# help
steel graincard.scm help
```

### advanced usage (from steel code)

```steel
(require "graincard.scm")

;; create a card with custom metadata
(define my-card
  (make-graincard "xbdghj" 
                  "introduction to grainorder"
                  "Grainorder is a permutation-based file naming system..."))

;; customize metadata
(hash-set! my-card :card-num 42)
(hash-set! my-card :total-cards 100)
(hash-set! my-card :grainbook-name "grainorder fundamentals")
(hash-set! my-card :author "your-name-here")

;; generate the card string
(define card-str (generate-graincard my-card))

;; validate it
(validate-graincard card-str)
;; → (ok "valid graincard!") or (err [...errors...])

;; save to file
(save-graincard my-card "xbdghj-intro-to-grainorder.md")
```

### text wrapping example

```steel
;; wrap a long line
(wrap-line "This is a very long line that will be wrapped to 78 characters preserving word boundaries and looking beautiful in the ASCII box." 78)
;; → ("This is a very long line that will be wrapped to 78 characters"
;;    "preserving word boundaries and looking beautiful in the ASCII box.")

;; wrap multiple paragraphs
(wrap-text "First paragraph with lots of text.\n\nSecond paragraph here.\n\nThird paragraph!" 78)
;; → ("First paragraph with lots of text."
;;    ""
;;    "Second paragraph here."
;;    ""
;;    "Third paragraph!")
```

---

## 🎨 example output

here's what a graincard looks like:

```
# graincard xbdghj - introduction to graintime

**file**: xbdghj-graincard.md
**live**: https://github.com/teamrebel10/graincard/...

```
┌──────────────────────────────────────────────────────────────────────────┐
│ GRAINCARD xbdghj                          Card 1 of 1,235,520            │
│                                                                            │
│ graintime is a system for encoding astronomical data into git branch      │
│ names. it includes nakshatra (moon position), ascendant (rising sign),    │
│ and sun's house (time of day).                                            │
│                                                                            │
│ example branch: 12025-10-28--1130-PDT--moon-uttaradha-asc-arie23-sun-12h │
│                                                                            │
│ ... (content continues, wrapped to 78 chars) ...                          │
│                                                                            │
├──────────────────────────────────────────────────────────────────────────┤
│ grainbook: ember harvest 🎃                                                │
│ card: xbdghj (1 of 1,235,520)                                             │
│ now == next + 1 🌾                                                         │
└──────────────────────────────────────────────────────────────────────────┘
```
```

beautiful, right? 🎴

---

## 🔗 architecture

### template repo structure

this is the **template/org side** - shared specs and base implementations:

* **location**: `teamrebel10/graincard/`
* **purpose**: canonical steel implementations, shared by all teams
* **what**: graincard.scm (the core logic)
* **symlinked**: into personal grainstores for development

### personal repo connections

teams using graincard will:
1. **symlink** this repo into their grainstore (e.g., `grainstore/teamrebel10/graincard/`)
2. **import** the steel module in their scripts
3. **extend** with custom logic if needed (but keep core here!)

example symlink:
```bash
ln -s ~/github/teamrebel10/graincard ~/kae3g/grainkae3g/grainstore/teamrebel10/graincard
```

this keeps the template clean while allowing personal customization!

---

## 🌊 philosophy

### portable knowledge

graincards are **self-contained learning units**. like:
- **flash cards**: portable, bite-sized, perfect for memorization
- **man pages**: self-documenting, no external dependencies
- **index cards**: one concept per card, organized by number
- **tarot cards**: symbolic, numbered, part of a larger deck

each card tells a complete story in 80×110 characters.

### beautiful typography

why ASCII art? because:
- **monospace fonts** are readable and precise
- **box-drawing characters** look elegant in terminals
- **80 chars** is the universal standard (since punch cards!)
- **terminal UIs** need beautiful documentation too

graincards prove that CLI documentation can be *gorgeous*. ✨

### grainorder as identity

every card has a **grainorder** - its unique DNA:
- no UUIDs (too long, unreadable)
- no sequential numbers (collision-prone in distributed systems)
- just 6 consonants, 1.2 million possibilities
- human-readable and memorable

grainorder makes every card discoverable, sortable, and unique.

### glow g2 voice

every steel script is a teaching moment!

comments ask socratic questions. explain WHY, not just WHAT. check for understanding.

code should read like a patient teacher explaining to an eager student.

---

## 🎓 learning paths

**start here**: read `graincard.scm` - it's full of teaching comments!

**then explore**:
1. **create your first card** - try `steel graincard.scm create ...`
2. **validate a card** - see what good structure looks like
3. **customize metadata** - experiment with card numbers, grainbook names
4. **wrap long content** - test the text wrapping with different lengths

each step builds on the last. follow your curiosity! 🎡

---

## 📋 implementation status

### ✅ phase 1 complete

* graincard.scm (426 lines)
  - text wrapping (word-preserving, multi-paragraph)
  - line padding & formatting (exactly 80 chars)
  - graincard structure (hash with metadata)
  - generation (full 80×110 markdown)
  - validation (checks 116 lines, borders, width)
  - file i/o (save with validation)
  - CLI interface (create + validate)

### 🚧 phase 2 in progress

* grainbook management (collections of cards)
* card linking (prev/next navigation)
* template support (reusable card layouts)
* syntax highlighting (code blocks in cards)

### 📋 phase 3 planned

* graincard viewer (terminal UI for browsing)
* integration with grainorder (auto-assign IDs)
* batch generation (create multiple cards)
* interactive mode (prompt for values)
* markdown export (convert cards to regular markdown)

---

## 💭 questions?

**"why 80 characters specifically?"**  
→ it's the classic terminal width since punch cards! most terminals default to 80 cols. readable on any device!

**"can i use colors/ANSI codes?"**  
→ not in phase 1 - pure ASCII art only. but phase 2 might add optional color support for terminals that support it!

**"what if my content is longer than 110 lines?"**  
→ split it into multiple cards! graincards are meant to be bite-sized. use prev/next links to connect them!

**"can i embed code in graincards?"**  
→ absolutely! just include it in the content. phase 2 will add syntax highlighting!

**"why steel instead of clojure/babashka?"**  
→ pure rust+steel stack! safety meets elegance. R5RS compliant scheme. no JVM, no java interop, just rust and lisp harmony!

---

## 🌐 related projects

* **teamtreasure02/grainorder** - permutation-based file naming (provides the unique IDs)
* **teamshine05/graintime** - astronomical timestamps for version control
* **teamtreasure02/graindb** - immutable database with time-travel queries
* **teamkae3gtravel12** - personal grainstore exploring all grain network concepts

---

## ⚠️ status note

this repository is currently **transitioning from personal exploration to org structure**.

what this means:
- **core logic is stable** (graincard.scm works!)
- **documentation is being refined** for broader audience
- **examples are being generalized** from kae3g-specific to team-agnostic
- **testing is in progress** across different content types

**feel free to explore**, but know that some rough edges remain as we polish for production use!

issues and PRs welcome - especially if you find bugs or unclear documentation! 🙏

---

## 🎡 final thoughts

this is a **journey**. the wheel of fortune teaches us about cycles - every ending is a new beginning.

graincards are about making knowledge **portable** and **beautiful**. they're about teaching one concept at a time, perfectly sized for human consumption.

you don't need fancy UIs or web frameworks. sometimes the best interface is 80 monospace characters and good ASCII art. 🎴✨

_may your graincards spin the wheel of wisdom..._ 🎡🌾

---

**org repo**: https://github.com/teamrebel10/graincard  
**grainbranch**: `12025-10-28--1130-PDT--moon-uttaradha-asc-arie23-sun-12h--teamrebel10`  
**related**: [team 12 grainstore](https://github.com/kae3g/teamkae3gtravel12)  
**main monorepo**: https://github.com/kae3g/grainkae3g

_now == next + 1_ 🌾

