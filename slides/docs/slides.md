---
theme: ./slidev-theme-cscs
---

## User Documentation at CSCS

Ben Cumming

eth-cscs.github.io/pead26

PEAD at CuG 2026

---
layout: two-cols-header
layoutClass: gap-2
---

# It all started 18 months ago...

18 months ago CSCS had objectively bad documentation.

::left::

**<div class="text-center">How</div>**

- Low coverage of key topics
- Out of date information
- Different visual and writing style across sections
- `<opinion>`It looked "naff"`</opinion>`
- Staff were frustrated with it
    * `<opinion>`fixing it felt "too big"`</opinion>`
- Useful information was in FAQs
    * `<opinion>`FAQs are not docs`</opinion>`

::right::

**<div class="text-center">What</div>**

- We used a content managent system (CMS)
    * Confluence<br>`<opinion>` ☠️❗💣💩  `</opinion>` 
- There was one person responsible for the docs
    * Role was to politely ask groups for submissions
- There was no "CSCS style"

---

# Confluence challenges

The path of least resistance after CSCS adopted Confluence+Jira for internal use.
* docs are written by technical staff who wouldn't choose Confluence
* WYSIWYG editor is preferred by non-technical people

Separate [Production](https://confluence.cscs.ch/spaces/KB/overview) and [TDS](https://confluence.cscs.ch/spaces/KBTDS/overview) instances.
* change management was manual and error prone: copy pages from TDS to Production.
* broken links everywhere (copying from TDS to Prod required updating links)
* no clean history of changes

Confluence search (if you know, you know)

`<opinion>`Confluence looks and feels like... Confluence.`</opinion>`

---

# Process

* Individual teams responsible for their own documentation
    * islands of documentation
    * teams would reproduce other docs in their section instead of linking and updating other areas

* Little oversight of the documentation as a while
    * different "styles" and philosophies about what should be in the docs

* We started to optimize around Confluence search
    * An FAQ article was more likely to show up in search

---

# The change

1. A small group of frustrated engineers tried to fix the styling and content
    * the TDS was now completely out of sync with production
2. I created a small [mock up](https://bcumming.github.io/kb-poc/) vertical slice using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
    * **one hour** to style it and set up CI/CD for build and deploy to GitHub Pages
    * **one day** to copy and update the content
3. Very positive reaction from engineers
    * a self-appointed **benevolent committee for documentation** was formed
    * **one month part time** for me to port all existing docs
4. Little push-back
    * `<opinion>`speak up tool loudly and you might own the docs`</opinion>`

---

# The docs

`<opinion>`CSCS user docs are now quite good`</opinion>`

[Live demo](https://docs.cscs.ch)

- look and feel
- organisation
- pull request
    - TDS

---

# Benefits of MkDocs

Docs as code let's engineers use familiar tools
- clear history: multi-page edits are clearer
- CI/CD checks links and spelling

Markdown is not rich and MkDocs is simple to deploy
- focus on the content and don't fight styling
    - _except tables_: most contributors use html tables
- all engineers can own the deployment

Static docs are fast: to load and deploy

---

# New philosophy

- Updates to any part of the docs are encouraged
    - Request review from docs owners---if no response core team reviews and merges
- Incomplete docs are better than no docs
- Harmonised docs require central control
    - Core team rewrites and refactors contributions
        - easier than 
    - Take the time to discuss contributions beforehand

Not everybody was pleased with the change
- A small percentage liked WYSIWYG
- A small percentage liked zero oversight

---

# Topics for discussion

- We are getting more contributions written by AI Agents
    - They ignore our style guide (just like humans!)
- We get small fixes from our community---but nothing major
- Contributions from non-tech people
    - User facing docs are >90% technical in our case

