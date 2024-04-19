---
title: "The Threshold"
dg-home: true
dg-publish: true
dg-note-icon: "signpost"
dg-pinned: true
dg-hide-in-graph: true
cssClasses: ['cards', 'cards-cols-3', 'cards-cover', 'cards-cover-no-border', 'cards-title-hide-icons']
dg-metatags:
  description: "Utsob's Digital Garden"
  "og:description": "Utsob's Digital Garden"
created: 2023-01-02T21:30:15+06:00
updated: 2024-02-27T09:19:58+06:00
---
> [!quote] Socrates (from Plato's Apology)
> The unexamined life is not worth living.

With such noise and bustle around us, one cannot help but build a hermitage[^1] in one's mind for contemplation and peace.

This [digital garden](https://cagrimmett.com/notes/2020/11/08/what-are-digital-gardens/) contains some contemplations and musings from my retreats there.

Unsurprisingly, thoughts here are, like every thought, ever-changing.

If you have any questions or opinions, please feel free to contact me at [connect@utsob.me](mailto:connect@utsob.me) or see the [[#Discussion|Discussion]] section for other options.

## Maturity Levels
The maturity level for each note is represented by plant icons of various growth.

### Seedling
Seedlings (![Maturity Level: 1|14](https://hermitage.utsob.me/img/tree-1.svg)) are thoughts barely started. Maybe Jotted down in haste or simply showed only a brief amount of what I thought about it by far.

### Sapling
Saplings (![Maturity Level: 2|14](https://hermitage.utsob.me/img/tree-2.svg)) have a substantial amount of content, but much work to be done. Coherence and patterns are just emerging in it.

### Tree
Trees (![Maturity Level: 3|14](https://hermitage.utsob.me/img/tree-3.svg)) are grown-up coherent pieces of thought/essay/expression that should not change much except for some editorial enhancements.

### Withered
Withered (![Maturity Level: withered|14](https://hermitage.utsob.me/img/withered.svg)) are the notes expressing outdated views (totally or partially) but kept for the historicity of our thoughts. For partially outdated notes, warnings will be placed wherever appropriate.

### Stone
Stones (![Maturity Level: stone|14](https://hermitage.utsob.me/img/stone.svg)) are notes exported/extracted from other mediums (e.g. Reading highlights and notes). Growth is irrelevant for these notes.

### Signpost
Signposts (![Maturity Level: signpost|14](https://hermitage.utsob.me/img/signpost.svg)) are notes that allow us to navigate easily (e.g. Collection of books or writings).

> [!Warning] 
> Maturity Levels are subjective. It might only mean that I'm a very immature person.


## On Top of My mind…
```dataview
TABLE WITHOUT ID
"<img src='https://hermitage.utsob.me/img/" + dg-note-icon + "-cover-card.jpg'/>" as Cover,
link(file.path, title) as Title,
dateformat(updated, "'<i icon-name=calendar-clock></i><small>'MMM dd, yyyy hh:mm a'</small>'") as Updated,
dateformat(created, "'<i icon-name=calendar-plus></i><small>'MMM dd, yyyy hh:mm a'</small>'") as Created,
join(file.tags, " ") as Tags,
"<img class=inset-cover src='" + default(cover, "") + "'/>" as Inset
from "/" WHERE dg-publish = true AND dg-home != true AND garden-index != true
SORT updated DESCENDING
LIMIT 12
```
## Discussion
Digital gardens are not about marketing/expressing opinion/prophesize etc. It is about nurturing our thoughts publicly and collectively. To discuss with me, there are two ways.

### Emails
From any note's page, click the email button on the bottom right to start an email thread. This is private.

### Comments
Every note has a comment section powered by Disqus. Feel free to use that if you don't mind public discussions.

### Policy
I don't seek publicity. If you find anything praiseworthy (I doubt you will), tell me privately. For commenting, these are the basic rules I believe not too much to abide by:
1. Constructive Criticism.
2. Adhere to the rules of arguments. To name a few:
    1. No personal attack (it's a fallacy)
    2. Other logical fallacies
    3. Avoid irrelevance.
3. Be civil, please.

[^1]: Here, I'm using the word hermitage as a loose translation of the word [Tapovan](https://en.wikipedia.org/wiki/Tapovan).