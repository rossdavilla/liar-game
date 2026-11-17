# The Liar Game

A phone-first classroom game for **PSY001 (Intro)** and **PSY157 (Relationships)**. Each student
writes one surprising truth and one dramatic lie; the app secretly picks one of the two, and the
student has to convince the class it is true.
Live at <https://rossdavilla.github.io/liar-game/>.

## Layout

```
index.html                 the whole game — one self-contained file, no build step
images/hero.jpg            the five illustrations the app shows; each slot is
images/truth.jpg           hidden until its file exists, so the app works
images/lie.jpg             without any of them
images/sealed.jpg
images/stage.jpg
tools/generate-images.md   a Claude-in-Chrome script that regenerates those five images
```

The app is HTML, CSS and JS inline in `index.html`. The only outside request is Google Fonts.

## Things worth knowing

- **The draw is the point.** The app picks truth or lie at random and shows the student only
  their assignment. Nothing is sent anywhere; the student's two statements and the draw live in
  `localStorage` on their own phone, so a refresh does not lose them and the instructor never
  sees them. Keep it that way — no backend, no analytics.
- **Images are optional.** Do not add a hard dependency on a file in `images/`; follow the
  existing pattern where a missing image just hides its slot.
- `tools/generate-images.md` is a runbook Ross triggers by saying "Run tools/generate-images.md" —
  it drives ChatGPT in his Chrome to remake the five illustrations and save them into `images/`.
  Edit the prompts there rather than regenerating images ad hoc.
- Phone-first: test the layout at phone width before anything else. It is played on students'
  own phones, not projected.

## Publishing

GitHub Pages serves `main` from the repository root, so a push to `main` is the deploy.

This app is already in `data/sites.json` in the
[`rossdavilla.github.io`](https://github.com/rossdavilla/rossdavilla.github.io) repo, tagged
`PSY001` and `PSY157`. Add an entry there only for a genuinely new page.
