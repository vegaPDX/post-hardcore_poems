# Classics in the Key of Post-Hardcore
 
**A companion site for a series of post-hardcore songs built from public domain poetry.**
 
Live site: [vegapdx.github.io/post-hardcore_poems](https://vegapdx.github.io/post-hardcore_poems/)
 
Listen and remix: [suno.com/@vegapdx](https://suno.com/@vegapdx)
 
---
 
## Why This Exists
 
I never understood poetry in school. The poems were about people living in the 1700s and 1800s that I had no connection to. Waves crashing on rocks. Angels breathing on armies. A guy who couldn't talk to women at a party in 1915. Teachers read these poems out loud in classrooms and expected something to click, but nothing ever did. The words were old. The people were old. The feelings were buried under language I couldn't reach.
 
Then I started turning these poems into post-hardcore songs. I kept the original themes, the original structures, the original emotional engines — but I replaced the images with things I could see. A borrowed pen instead of a gleaming spear. A phone screen instead of a party full of strangers. A woman coming home from work instead of a woman walking through a storm to a cottage. An undocumented worker mowing lawns instead of an Indian water carrier on a battlefield.
 
And the poems finally made sense. Not because the words changed. Because the distance closed. The feelings in these poems are not old. Grief is not old. Guilt is not old. Loneliness is not old. The poets knew exactly what they were writing about. I just needed to hear it in a language I already understood.
 
## What the Songs Are
 
Every song in this series starts with a public domain poem and turns it into a post-hardcore track. The music is fast, heavy, and loud — thick downtuned guitars, double kick drums, screamed and raw vocals at ~155-180 BPM. The lyrics are modern, concrete, and specific.
 
Each song takes a public domain poem and uses it as a blueprint. The original poem provides the emotional structure, key images, and thematic arc. The adaptation translates those into modern concrete details with entirely new, original lyrics. The poem is the skeleton. The song is a new body built on top of it.
 
## What This Site Does
 
Each song page on the site shows three things:
 
1. **The original poem and the song lyrics displayed side by side.** The full text of the public domain poem on the left, the full song lyrics on the right.
 
2. **A detailed structure mapping.** This traces specific lines, images, and structural choices from the original poem to the song. It doesn't just note that parallels exist — it explains what each parallel does, why it works, and what it makes the listener feel. Individual word choices, the function of gaps and absences, and the emotional weight of specific details are all analyzed.
 
3. **How the title works as a structural element.** Every song page ends with an analysis of how the poem's title and the song's title each function within their respective texts.
 
## The Songs

The project is organized into volumes of 12 songs each.

### Volume 1

| # | Song | Source Poem | Poet | Year |
|---|---|---|---|---|
| 1 | The Old Lie | Dulce et Decorum Est | Wilfred Owen | 1920 |
| 2 | The Floor Gave Out | I Felt a Funeral, in my Brain | Emily Dickinson | 1861 |
| 3 | Same As Everyone | A Litany in Time of Plague | Thomas Nashe | 1592 |
| 4 | The Weight of Her Hand | Break, Break, Break | Alfred, Lord Tennyson | 1842 |
| 5 | And God Said Nothing | Porphyria's Lover | Robert Browning | 1836 |
| 6 | Nothing Would Come Out | The Love Song of J. Alfred Prufrock | T.S. Eliot | 1915 |
| 7 | Three Seconds | The Rime of the Ancient Mariner | Samuel Taylor Coleridge | 1798 |
| 8 | Marcus Has My Pen | The Destruction of Sennacherib | Lord Byron | 1815 |
| 9 | Jesús | Gunga Din | Rudyard Kipling | 1890 |
| 10 | Followed Every Rule | The Mask of Anarchy | Percy Bysshe Shelley | 1819 |
| 11 | None of It Was Real | The Waste Land | T.S. Eliot | 1922 |
| 12 | There Was Never Anyone There | Hap | Thomas Hardy | 1866 |

### Volume 2 (in progress)

| # | Song | Source Poem | Poet | Year |
|---|---|---|---|---|
| 1 | Friend of a Friend | La Belle Dame sans Merci | John Keats | 1819 |
| 2 | My Father's Son | The Welsh Marches (A Shropshire Lad, XXVIII) | A.E. Housman | 1896 |
 
## How the Songs Are Made
 
The music and the lyrics are both created with AI tools. The lyrics and all creative direction are written collaboratively with [Claude](https://claude.ai) by Anthropic. The music is generated with [Suno](https://suno.com), an AI music generation platform. The website is hand-coded HTML and CSS, also written collaboratively with Claude.
 
The process for each song works like this: I start with a poem and an idea for how to modernize it. Claude and I work through the adaptation together — identifying the poem's emotional engine, choosing a modern scenario that maps to the same structure, and writing original lyrics that translate the poem's images into concrete, present-day details. Claude then assembles the Suno prompt (genre tags, vocal style, BPM, instrumentation) and I generate variations in Suno until one lands. The website analysis pages are drafted collaboratively and reviewed by me before publishing.
 
I am not a musician, a poet, or a programmer. These tools made it possible for me to build something I couldn't have built alone. I think that's worth being transparent about.
 
## Listening and Remixing
 
The songs are available on my [Suno profile](https://suno.com/@vegapdx). They are not on streaming platforms. I am not monetizing this project.
 
All source poems are in the public domain. The song lyrics are original works. In the spirit of the public domain artwork that inspired this project, all songs are open for edits, remixes, and covers. Take them, change them, make them yours. That's what I did with the poems.
 
## Repo Structure

The site is organized into volumes, each in its own subdirectory. Shared assets and the hub page live at the root.

```
/
├── index.html              Hub page — links to all volumes
├── styles.css              Shared stylesheet used by every page
├── prompts.html            Suno prompts index — base templates, techniques, song cards
├── README.md               This file
├── vol1/
│   ├── index.html          Volume 1 song grid
│   ├── the-old-lie.html    Owen's "Dulce et Decorum Est"
│   ├── the-floor-gave-out.html       Dickinson's "I Felt a Funeral, in my Brain"
│   ├── same-as-everyone.html         Nashe's "A Litany in Time of Plague"
│   ├── the-weight-of-her-hand.html   Tennyson's "Break, Break, Break"
│   ├── and-god-said-nothing.html     Browning's "Porphyria's Lover"
│   ├── nothing-would-come-out.html   Eliot's "The Love Song of J. Alfred Prufrock"
│   ├── three-seconds.html            Coleridge's "The Rime of the Ancient Mariner"
│   ├── marcus-has-my-pen.html        Byron's "The Destruction of Sennacherib"
│   ├── jesus.html                    Kipling's "Gunga Din"
│   ├── followed-every-rule.html      Shelley's "The Mask of Anarchy"
│   ├── none-of-it-was-real.html      Eliot's "The Waste Land"
│   └── there-was-never-anyone-there.html   Hardy's "Hap"
├── vol2/
│   ├── index.html          Volume 2 song grid
│   ├── friend-of-a-friend.html       Keats's "La Belle Dame sans Merci"
│   └── my-fathers-son.html           Housman's "The Welsh Marches"
└── song_prompts/
    ├── the-old-lie.html    Full Suno inputs for each song
    ├── ...                 (one page per song, all volumes)
    ├── friend-of-a-friend.html
    └── my-fathers-son.html
```

Each volume directory contains its own `index.html` (song grid) and individual song pages. The `song_prompts/` directory contains one page per song showing the exact Suno inputs (Style of Music, Lyrics, Exclude Styles). All pages reference `../styles.css` from subdirectories. A volume switcher nav (Home | Vol. 1 | Vol. 2) appears in the header of every page. Adding a new volume means creating a new directory (e.g. `vol3/`) and adding a link to the switcher.
 
## Technical Details
 
The site is pure HTML and CSS. No JavaScript, no build tools, no frameworks, no dependencies other than Google Fonts. Pages in subdirectories reference `../styles.css`; root pages reference `styles.css` directly. The site is responsive — two-column layouts collapse to single-column below 700px. Hosted on GitHub Pages, deployed from the `main` branch.
 
Fonts loaded from Google Fonts: Cormorant (display/headings), Libre Baskerville (body text/poem text/lyrics), Work Sans (labels/navigation).
 
## Contributing
 
This is a personal project. Contributions are not open. But the repo is public — fork it, take the structure, and build your own version with your own poems and your own genres. That's the whole point.
 
---
 
A project by **vegaPDX**.
