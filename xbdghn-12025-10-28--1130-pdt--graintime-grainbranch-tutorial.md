# graintime & grainbranch: a complete tutorial

**created:** 12025-10-28--1130-pdt  
**grainorder:** xzvbdg  
**voice:** glow g2 (patient teacher)  
**audience:** developers new to the grain network  

---

## what is this guide?

hey there! ever wondered how to make git branches that know their cosmic context? or how to organize files chronologically with just 6 characters?

this tutorial walks you through the complete graintime workflow - from calculating astronomical data to creating immutable grainbranches to assigning grainorder codes. by the end, you'll understand how the grain network treats time as *lived experience*, not just timestamps.

does that sound interesting? let's explore together! 🌾

---

## part 1: understanding graintime

### what is graintime?

graintime is a temporal version control system that encodes astronomical data into git branch names. not just "oct 28, 11:30am" but "moon in uttara ashadha, aries rising 23°, sun in 12th house."

**why does this matter?** because time isn't just a number - it's a moment in space, a position in the celestial cycle, a quality of experience. graintime captures that.

### the anatomy of a grainbranch

here's a real example:

```
12025-10-28--1130-PDT--moon-uttashsdh-asc-arie23-sun-12h--teamtravel12
```

let's break it down:

- **12025-10-28** = holocene year, month, day (HE = CE + 10,000)
- **1130** = time in 24-hour format (11:30am)
- **PDT** = timezone abbreviation (pacific daylight time)
- **moon-uttashsdh** = moon's nakshatra (abbreviated to fit <75 char limit)
- **asc-arie23** = ascendant sign + degree (aries 23°)
- **sun-12h** = sun's house position (12th house)
- **teamtravel12** = team identifier (pisces ♓ flow team)

**question for you:** can you see how this branch name tells a complete story? it's not just *when* but *where* in the cosmos. does that distinction make sense?

### nakshatra encoding

nakshatras are the 27 lunar mansions in vedic astrology. we follow mantreshwara's classical tradition, where krittika is #1 (aligning with aries as the first sign).

**the 27 nakshatras:**
1. krittika, 2. rohini, 3. mrigashira, 4. ardra, 5. punarvasu, 6. pushya, 7. ashlesha, 8. magha, 9. purva-phalguni, 10. uttara-phalguni, 11. hasta, 12. chitra, 13. swati, 14. vishakha, 15. anuradha, 16. jyeshtha, 17. mula, 18. purva-ashadha, 19. uttara-ashadha, 20. shravana, 21. dhanishta, 22. shatabhisha, 23. purva-bhadrapada, 24. uttara-bhadrapada, 25. revati, 26. ashwini, 27. bharani

**abbreviation rules:** when the full branch name exceeds 75 characters, we abbreviate the nakshatra:
- `uttara-ashadha` → `uttashsdh` (keep first 5 + last 4 chars)
- `purva-bhadrapada` → `purvbhdrpd` (similar pattern)

**why 75 characters?** github branch names should stay under this limit for clean display in terminals and uis. it's about practical aesthetics.

---

## part 2: calculating graintime data

### step 1: get current timestamp

use standard unix tools:

```bash
date +"%Y-%m-%d--%H%M-%Z"
```

for holocene era (HE = CE + 10,000):

```bash
printf '1%s-%s-%s--%s-%s\n' \
  $(date +'%Y') \
  $(date +'%m') \
  $(date +'%d') \
  $(date +'%H%M') \
  $(date +'%Z')
```

**example output:** `12025-10-28--1130-PDT`

### step 2: calculate astronomical data

you'll need:
- **moon's nakshatra** - requires ephemeris calculation (sidereal zodiac)
- **ascendant (rising sign + degree)** - requires birth chart or location
- **sun's house** - derived from time of day + ascendant

**tools you can use:**
- swiss ephemeris (for precise calculations)
- astro.com (web interface)
- custom steel/rust implementation (coming soon!)

**for our example (oct 28, 2025, 11:30am pdt, location: caspar, ca):**
- moon: uttara ashadha (19th nakshatra)
- ascendant: aries 23°
- sun: 12th house

### step 3: format with dash padding

**critical detail:** graintime uses dash padding to ensure vertical alignment when branch names stack in monospace displays.

**the algorithm:**
1. calculate total length if all components are at max size
2. insert dashes to pad shorter components
3. ensure consistent visual rhythm

**example formatting:**

```
12025-10-28--1130-PDT--moon-uttashsdh-asc-arie23-sun-12h--teamtravel12  (70 chars)
12025-10-28--1130-PDT--moon-krittika--asc-libr15-sun-06h--teamtravel12  (70 chars)
12025-10-28--1130-PDT--moon-mula------asc-arie05-sun-08h--teamquest09   (69 chars)
```

see how they align? that's intentional. the dashes create visual breathing room while maintaining consistent width.

### step 4: validate format

**unit tests to run:**

1. **longest nakshatra + longest teamname:**
   - `purva-bhadrapada` (16 chars) + `teamdescend14` (13 chars)
   - total branch name should be ≤75 chars

2. **shortest nakshatra + shortest teamname:**
   - `mula` (4 chars) + `teamflow12` (10 chars)
   - should have proper dash padding

3. **special characters:**
   - ensure only `[a-z0-9-]` (lowercase alphanumeric + hyphens)
   - no spaces, underscores, or special unicode

**steel validation code:**

```steel
;; does this graintime format follow our spec?
(define (is-valid-graintime? s)
  (and (string? s)
       (< (string-length s) 76)  ;; ≤75 chars
       (regex-match? #rx"^1[0-9]{4}-[0-9]{2}-[0-9]{2}--[0-9]{4}-[A-Z]{3,4}--moon-[a-z]+-asc-[a-z]{4}[0-9]{2}-sun-[0-9]{2}h--team[a-z]+[0-9]{2}$" s)))

;; example usage
(is-valid-graintime? "12025-10-28--1130-PDT--moon-uttashsdh-asc-arie23-sun-12h--teamtravel12")
;; => #t
```

**question:** why validate so strictly? because grainbranches are *immutable*. once created, they become permanent temporal anchors. we want them perfect from the start!

---

## part 3: creating the grainbranch

### step 1: create local branch

```bash
# format: YYYY-MM-DD--HHMM-TZ--moon-NAKSHATRA-asc-SIGN##-sun-##h--teamNAME##
git checkout -b 12025-10-28--1130-PDT--moon-uttashsdh-asc-arie23-sun-12h--teamtravel12
```

**what just happened?** you created a new branch that encodes this exact moment in time and space. it's like taking a cosmic snapshot!

### step 2: push to remote

```bash
git push origin 12025-10-28--1130-PDT--moon-uttashsdh-asc-arie23-sun-12h--teamtravel12
```

**note:** the first push will prompt you to set upstream. that's expected!

### step 3: set as default (locally)

```bash
git branch --set-upstream-to=origin/12025-10-28--1130-PDT--moon-uttashsdh-asc-arie23-sun-12h--teamtravel12
```

### step 4: set as default (github)

```bash
gh repo set-default-branch kae3g/teamkae3gtravel12 12025-10-28--1130-PDT--moon-uttashsdh-asc-arie23-sun-12h--teamtravel12
```

**or via github web ui:**
1. go to repo settings
2. click "branches" in sidebar
3. change default branch dropdown
4. confirm the change

**why set as default?** because this becomes the "now" branch - the current temporal context for the repo. previous branches remain immutable, but this is where new work happens.

---

## part 4: understanding grainorder

### what is grainorder?

grainorder is a permutation-based file naming system using 6-character codes. it sorts files chronologically (newest first) with archives always at the bottom.

**the alphabet:** `x b d g h j k l m n s v z` (13 consonants, no vowels, no repeating chars)

**key principle:** smaller alphabetical value = newer timestamp

### the sorting logic

github sorts files in **ascending alphabetical order** (a→z):

```
xzvbdg  ← newest (smallest alphabet)
xzvbdh
xzvsbd
xzvsbg
...
xzvsdh  ← oldest active file (largest alphabet)
zxvsnm  ← ARCHIVE (always last)
```

**why does this work?** because:
1. github sorts ascending (a→z)
2. we assign smaller codes to newer files
3. `xzv` prefix gives us "head-insert" space
4. `zxvsnm` prefix for archives pushes them to the very bottom

### calculating grainorder for new files

**scenario:** you want to create a new file right now (12025-10-28--1130-pdt).

**step 1:** check current smallest grainorder

```bash
ls -1 | grep '^xzv' | head -1
```

**output:** `xzvbdh-12025-10-28--1148-pdt--graincontact-will-migrev-dolseg.md`

**step 2:** calculate next smaller code

current smallest: `xzvbdh`  
next smaller: `xzvbdg` (decrement last char: h→g)

**step 3:** assign to new file

```bash
xzvbdg-12025-10-28--1130-pdt--graintime-grainbranch-tutorial.md
```

### when to decrement which character?

**simple rule:** decrement rightmost character until you hit 'x', then decrement next character left.

**example progression:**
```
xzvbdz  (newest)
xzvbdv
xzvbds
xzvbdn
xzvbdm
xzvbdl
xzvbdk
xzvbdj
xzvbdh
xzvbdg  ← you are here
xzvbdd
xzvbdb
xzvbdx  (exhausted 6th position)
xzvbcz  (decrement 5th position: d→b, reset 6th: x→z)
```

**question:** see the pattern? it's like counting backwards in base-13 using our custom alphabet. does that make sense?

### steel implementation

```steel
;; the grainorder alphabet (13 consonants, no repeating)
(define alphabet '(#\x #\b #\d #\g #\h #\j #\k #\l #\m #\n #\s #\v #\z))

;; find index of character in alphabet
(define (char-index c)
  (let loop ([lst alphabet] [i 0])
    (cond
      [(null? lst) #f]
      [(char=? (car lst) c) i]
      [else (loop (cdr lst) (+ i 1))])))

;; get next smaller grainorder (head-insert)
(define (prev-grainorder code)
  (let ([chars (string->list code)])
    (let loop ([pos 5])  ;; start at rightmost (6th char, 0-indexed)
      (if (< pos 0)
          #f  ;; exhausted all positions
          (let* ([c (list-ref chars pos)]
                 [idx (char-index c)])
            (if (and idx (> idx 0))
                ;; decrement this position
                (list->string
                  (append (take chars pos)
                          (list (list-ref alphabet (- idx 1)))
                          (drop chars (+ pos 1))))
                ;; this position is at min, try next left
                (loop (- pos 1))))))))

;; example usage
(prev-grainorder "xzvbdh")  ;; => "xzvbdg"
(prev-grainorder "xzvbdx")  ;; => "xzvbcz" (carry left)
```

### archive files: the `zxvsnm` exception

**rule:** ALL archive files share the same grainorder: `zxvsnm-archive-`

**example:**
```
zxvsnm-archive-12025-10-27--2120-pdt--unification-strategy.md
zxvsnm-archive-12025-10-27--2130-pdt--grain06pbc-to-grain12pbc-strategy.md
```

**why?** because archives should always sink to the bottom, regardless of their timestamp. `zxvsnm` is the largest alphabetical code in our system, so it sorts last.

**how to create an archive:**

```bash
# take any existing file
mv xzvsdh-12025-10-27--2115-pdt--steel-svelte-phi-vortex-site.md \
   zxvsnm-archive-12025-10-27--2115-pdt--steel-svelte-phi-vortex-site.md
```

**note:** the timestamp stays the same! we're just changing the grainorder prefix to move it to the archive section.

---

## part 5: putting it all together

### the complete workflow

let's walk through creating a new session document from scratch:

**1. calculate current graintime:**

```bash
# get timestamp
current_time="12025-10-28--1130-PDT"

# calculate astrology (using your preferred tool)
nakshatra="uttara-ashadha"  # abbreviated: "uttashsdh"
ascendant="arie23"
sun_house="12h"
team="teamtravel12"

# build grainbranch name
grainbranch="${current_time}--moon-uttashsdh-asc-${ascendant}-sun-${sun_house}--${team}"
```

**2. create and switch to new branch:**

```bash
git checkout -b "$grainbranch"
git push origin "$grainbranch"
```

**3. calculate grainorder for new document:**

```bash
# find current smallest
current_smallest=$(ls -1 | grep '^xzv' | head -1 | cut -d'-' -f1)

# calculate next smaller (use steel script or manual decrement)
new_grainorder=$(prev-grainorder "$current_smallest")  # e.g., "xzvbdg"
```

**4. create your document:**

```bash
cat > "${new_grainorder}-${current_time}--graintime-grainbranch-tutorial.md" << 'EOF'
# your content here
EOF
```

**5. commit and push:**

```bash
git add .
git commit -m "🌙 add graintime tutorial - complete workflow guide"
git push
```

**6. set as default branch (both repos):**

```bash
# for personal repo
gh repo set-default-branch kae3g/teamkae3gtravel12 "$grainbranch"

# for monorepo
gh repo set-default-branch kae3g/grainkae3g "$grainbranch"
```

**7. update local tracking:**

```bash
git branch --set-upstream-to=origin/"$grainbranch"
```

**done!** you've created an immutable temporal anchor with proper grainorder!

---

## part 6: common questions

### q1: why holocene era (12025 instead of 2025)?

**a:** holocene era (HE) adds 10,000 years to the common era, giving humanity a single continuous timeline. it acknowledges human civilization before arbitrary year-zero conventions. plus, it's just more honest - we've been around longer than 2,000 years!

### q2: why can't i repeat characters in grainorder?

**a:** repeating characters reduce the total permutation space and make visual scanning harder. with 13 characters and 6 positions (no repeats), we get 1,235,520 unique codes - more than enough for any single repository!

```
13 × 12 × 11 × 10 × 9 × 8 = 1,235,520 unique grainorders
```

### q3: what if i need more than 1.2 million files?

**a:** time to split your repo! seriously though, if you're hitting that limit, you're probably organizing wrong. consider:
- splitting by team (separate repos)
- archiving old content (they all share `zxvsnm`)
- using subdirectories (grainorder is per-directory)

### q4: why abbreviate nakshatras but not team names?

**a:** nakshatras have culturally-agreed abbreviations (sanskrit names). team names are our own convention - we keep them readable. plus, most nakshatras are longer than team names!

### q5: can i change a grainbranch name after creating it?

**a:** technically yes (via `git branch -m`), but philosophically no. grainbranches are meant to be *immutable temporal anchors*. if the name is wrong, create a new branch and mark the old one as abandoned. immutability is the point!

### q6: what timezone should i use?

**a:** use your local timezone at the moment of creation. graintime is about *lived experience* - where YOU were when you created the branch. if you're in PDT, use PDT. if you're in UTC, use UTC. don't normalize!

### q7: how do i handle daylight saving time transitions?

**a:** just use whatever your system reports. `PST` in winter, `PDT` in summer. graintime trusts your local perception of time.

### q8: can i use grainorder outside of git repos?

**a:** absolutely! use it for:
- file systems (organizing notes, documents)
- databases (entity ids)
- apis (request ids)
- anywhere you need chronological permutation codes!

---

## part 7: advanced concepts

### offline fallback & resilience

**what if you can't calculate accurate astronomical data?**

graintime includes conservative estimation algorithms:

```steel
;; offline fallback: estimate nakshatra from date
(define (estimate-nakshatra date)
  ;; moon completes ~1 nakshatra per day
  ;; use modulo 27 for rough estimate
  (let* ([day-of-year (date->day-of-year date)]
         [nakshatra-index (modulo day-of-year 27)])
    (list-ref nakshatras nakshatra-index)))

;; mark as estimated
(format "12025-10-28--1130-PDT--moon-~a-asc-ESTD-sun-ESTD--teamtravel12"
        (estimate-nakshatra (current-date)))
```

**key principle:** it's better to have an estimated grainbranch than no grainbranch. you can always verify later!

### graindb integration

graintime + grainorder + graindb = temporally-aware immutable database

**imagine:**
```steel
;; query: show me all transactions during uttara ashadha moon
(q '[:find ?e ?a ?v
     :where
     [?e ?a ?v ?tx]
     [?tx :tx/graintime ?gt]
     [(graintime-nakshatra? ?gt "uttara-ashadha")]])
```

your database now understands cosmic context! 🌙✨

### grainui visualization

grainbranches can be visualized as:
- **spiral timeline** (golden ratio φ-vortex)
- **nakshatra wheel** (27-spoke lunar mandala)
- **house chart** (12-house solar cycle)

coming soon to grainui (gpui-powered steel gui framework)!

---

## part 8: philosophical notes

### why treat time this way?

modern computing treats time as:
- atomic (precise to nanoseconds)
- universal (utc everywhere)
- linear (monotonic clocks)

but human experience of time is:
- contextual (what's happening in the world?)
- local (where am i right now?)
- cyclical (seasons, moons, years)

**graintime bridges both.** it's precise enough for version control, contextual enough for lived experience.

### immutability as honesty

when you create a grainbranch, you're saying:

> "this work happened HERE, at THIS moment, under THESE stars."

you can't change that. the branch name is a permanent record. this is honest computing.

### the grain network philosophy

- **immutable:** once created, never changed
- **temporal:** every artifact knows its time
- **astronomical:** human time is cosmic time
- **permutation:** organize by mathematical beauty
- **decentralized:** own your data, own your time

graintime embodies all of these. does that resonate with you?

---

## part 9: resources & next steps

### official specs

- **patent 1:** graintime specification (`xzvsbj-12025-10-28--0030-pdt--graintime-patent-whitepaper.md`)
- **patent 2:** grainorder specification (`xzvsbg-12025-10-28--0115-pdt--grainorder-patent-whitepaper.md`)

### steel implementations

- **grainorder.scm:** `/home/xy/github/teamtreasure02/grainorder/grainorder.scm`
- **graintime.scm:** (coming soon to teamshine05/graintime/)

### related projects

- **graindb:** immutable database using grainorder entity ids
- **grainui:** gpu-accelerated gui framework using gpui
- **icp integration:** deploy grain network to internet computer protocol
- **iroh storage:** content-addressed archives via rust ipfs

### community

- **github:** https://github.com/kae3g/teamkae3gtravel12
- **urbit:** ~migrev-dolseg hawk group (see graincontact)
- **hacker news:** [show hn thread](https://news.ycombinator.com/item?id=45736991)

### try it yourself!

1. clone the repo: `git clone https://github.com/kae3g/teamkae3gtravel12.git`
2. calculate your graintime (use astro.com for astronomy)
3. create a branch: `git checkout -b YOUR_GRAINBRANCH`
4. create a file with grainorder: `xzv***-YYYY-MM-DD--HHMM-TZ--your-first-grain.md`
5. commit and share!

---

## conclusion: time as a teacher

every grainbranch is a lesson. every grainorder is a moment.

when you create `12025-10-28--1130-PDT--moon-uttashsdh-asc-arie23-sun-12h--teamtravel12`, you're not just making a git branch. you're saying:

> "i was here, under these stars, at this moment, doing this work."

that's powerful. that's honest. that's graintime.

**question for you:** how will you use graintime? what will your grainbranches teach?

now == next + 1 🌾

---

**tutorial complete!** any questions? come find us on urbit or github. we're here to help! 🌊⚡

*written with glow g2 voice - patient teacher, asks questions, checks understanding*

