# teamtravel12 - grainstore for team 12 (pisces ♓ flow)

> **📢 WORK IN PROGRESS**: This repository is being generalized from the personal `kae3g/teamkae3gtravel12` repo. The core systems (grainorder, graintime, graindb, grainui) are solid and production-ready, but documentation and structure are evolving to be more accessible for broader team use! 🌊✨
>
> **Personal version**: https://github.com/kae3g/teamkae3gtravel12  
> **Status**: Personal → org transition in progress

---

**grainbranch**: `12025-10-28--1130-PDT--moon-uttaradha-asc-arie23-sun-12h--teamtravel12`  
**team**: team 12 - travel (pisces ♓ / xii. the hanged man)  
**voice**: glow g2 (patient listening teacher)  
**focus**: flow, trust, perspective shift, letting go  
**org repo**: https://github.com/teamtravel12/teamtravel12

---

## 🌊 what is team 12?

**pisces ♓** (mutable water) - flowing, adaptive, transcendent  
**the hanged man xii** (tarot) - perspective shift, surrendering to flow, trust

this is **team 12's grainstore** - systems for temporal computing and organizational flow:

* **grainorder** - chronological file naming (1.2m unique codes, smallest=newest)
* **graintime** - astronomical git branches (nakshatra, ascendant, solar house)
* **grainmirror** - multi-repo synchronization without symlinks
* **graindb** - immutable database with time-travel queries
* **grainui** - gpu-accelerated gui framework (steel + gpui)
* **steel** - pure rust+lisp stack (no jvm, no babel, just rust & scheme)

---

## 🎯 the grain network

team 12 is part of the **14-team grain network** - a tarot/zodiac-inspired organizational system where each team has:
- a zodiac sign (energy/mode)
- a tarot archetype (purpose/philosophy)
- specific domains of responsibility

### team 12 responsibilities:

* **grainflow** - deployment automation (flow between repos)
* **grainbranch** - immutable temporal branches
* **grainmirror** - conscious duplication across repos
* temporal awareness and version control innovation

_we flow between states, trust the process, and see from new perspectives_ 🌊

---

## 📁 key systems

### grainorder - chronological naming

**6-character codes** from alphabet `xbdghjklmnsvz` (13 consonants, no repeats):
- **1,235,520 unique codes** total
- **newest = smallest** (e.g., `xbdghj` for newest)
- **oldest = largest** (up to `xzvsnm` for active files)
- **archives** use `zxvsnm` prefix (z sinks to bottom in a→z sort)

→ [learn more](https://github.com/teamtreasure02/grainorder)

### graintime - astronomical branches

git branches encode **exact astronomical moment**:
- holocene era timestamp (12025, not 2025)
- moon's nakshatra (27 vedic lunar mansions)
- ascendant (rising sign + degree)
- sun's house (time of day, 1-12)

example: `12025-10-28--1130-PDT--moon-uttaradha-asc-arie23-sun-12h--teamtravel12`

→ [learn more](https://github.com/teamshine05/graintime)

### grainmirror - conscious duplication

**hard copies** instead of symlinks:
- github displays all copies in web ui
- each repo tracks independently
- unique filenames: `{grainorder}-{timestamp}--readme-{org}-{repo}.md`
- sync on demand, detect drift with sha256

→ implementation in [teamshine05/graintime/grainmirror.scm](https://github.com/teamshine05/graintime/blob/12025-10-28--1130-PDT--moon-uttaradha-asc-arie23-sun-12h--teamshine05/grainmirror.scm)

---

## 🔗 team repos

### active team projects

* [teamshine05/graintime](https://github.com/teamshine05/graintime) - astronomical timestamps & branch automation
* [teamrebel10/graincard](https://github.com/teamrebel10/graincard) - 80×110 monospace teaching cards
* [teamtreasure02/grainorder](https://github.com/teamtreasure02/grainorder) - permutation-based naming system
* [teamtreasure02/graindb](https://github.com/teamtreasure02/graindb) - immutable database (datomic-inspired)
* [teamtravel12/grainflow](https://github.com/teamtravel12/grainflow) - deployment automation

### personal development

* [kae3g/teamkae3gtravel12](https://github.com/kae3g/teamkae3gtravel12) - personal exploration hub
* [kae3g/grainkae3g](https://github.com/kae3g/grainkae3g) - main development monorepo

---

## 🛠️ technology stack

### steel - rust lisp

**why steel?**
- R5RS-compliant scheme
- pure rust implementation (no jvm!)
- FFI to rust crates (gpui, iroh, swiss ephemeris)
- fast, safe, and elegant

**core modules:**
- `grainorder.scm` - permutation generation
- `graintime.scm` - astronomical calculations  
- `grainbranch.scm` - git automation
- `grainmirror.scm` - multi-repo sync
- `graincard.scm` - teaching card generation

### steel migration status

**21% complete** (15/73 scripts migrated from babashka)

recent additions:
- `grain-macros.scm` - `println!`, `with-box` helpers
- `grain-specs.scm` - data validation (spec-inspired)

---

## 📚 documentation

**start here:**
- grainmirrored readmes show up as `xzvsk*-...-readme-{org}-{repo}.md` files
- each explains a specific subsystem
- all timestamped and grainordered (newest first!)

**deep dives:**
- graintime patent whitepaper
- grainorder patent whitepaper  
- graindb steel database design
- grainui gpui integration strategy

---

## ⚠️ current status

this repo is **actively evolving** from personal exploration to professional org use:

**what's solid:**
- ✅ core steel implementations work
- ✅ grainorder, graintime, graindb proven
- ✅ grainmirror pattern established
- ✅ gpui integration strategy documented

**what's in flux:**
- 🚧 documentation being refined for broader audience
- 🚧 examples being generalized (less kae3g-specific)
- 🚧 steel migration ongoing (73 scripts to convert)
- 🚧 testing across different environments

**contributions welcome!** especially:
- bug reports
- documentation improvements
- steel module contributions
- testing on different platforms

---

## 💭 philosophy

### immutability as honesty

when you encode astronomy into a git branch, you're saying: "this happened HERE, at THIS moment, under THESE stars."

you can't change that. the moon doesn't reverse its orbit!

honest computing acknowledges the irreversible flow of time.

### flow, not force

team 12 (pisces / hanged man) teaches: sometimes you have to surrender, trust the process, hang upside down to see clearly.

we don't force systems into rigid structures. we let them flow into their natural shape.

### glow g2 voice

every piece of code is a teaching moment:
- ask socratic questions
- explain WHY not just WHAT
- check for understanding
- patient, hand-holding explanations

_teach through the code itself_ 🌾

---

## ☀️ getting started

1. **explore the grainmirrored readmes** (they're in this repo!)
2. **try creating a grainbranch**: `steel grainbranch.scm create yourteam`
3. **read the patents** for deep understanding
4. **join the flow** - contributions welcome!

---

**org repo**: https://github.com/teamtravel12/teamtravel12  
**personal repo**: https://github.com/kae3g/teamkae3gtravel12  
**main monorepo**: https://github.com/kae3g/grainkae3g  
**grainbranch**: `12025-10-28--1130-PDT--moon-uttaradha-asc-arie23-sun-12h--teamtravel12`

_may your code flow like water..._ 🌊⚡

_now == next + 1_ 🌾
