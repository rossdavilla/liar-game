# The Liar Game

A phone-first classroom game for PSY001 (Intro) and PSY157 (Relationships). Each student writes
one surprising truth and one dramatic lie; the app secretly picks one of the two, and the student
has to convince the class it is true.

**Live:** https://rossdavilla.github.io/liar-game/

## How it plays

1. Every student opens the page on their own phone and writes their truth and their lie.
2. The app draws one of the two at random and shows the student only their assignment.
3. They tell the class that story and take questions; the class votes on whether it was true.

Because the draw is private and random, nobody — including the instructor — knows who got which.

## What's in here

```
index.html                 the whole game — one self-contained file, no build step
images/                    five illustrations; each slot is hidden until its file exists,
                           so the app works with none of them
tools/generate-images.md   a Claude-in-Chrome runbook that regenerates those five images
```

## Running it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

The two statements and the draw are kept in `localStorage` on the student's own phone. There is
no backend, no analytics and no account — nothing leaves the device, which is what makes the
private draw credible.
