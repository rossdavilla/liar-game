# Generate the Liar Game illustrations with ChatGPT (Claude in Chrome script)

**How to use:** in Claude Code, open this project and say:

> Run tools/generate-images.md

Claude follows the steps below in your Chrome using the Claude in Chrome extension.
It makes five images in ChatGPT, saves them to `images/`, and checks them in the app.

---

## What to make

The app looks for these five files. Until a file exists, its slot is hidden, so the app works without them.

| File | Where it appears | Subject |
|---|---|---|
| `images/hero.png` | Welcome screen | Confident performer with a theater mask, fingers crossed behind their back |
| `images/truth.png` | Step 1 (the surprising truth) | Lightbulb with a surprised face popping out of a gift box |
| `images/lie.png` | Step 2 (the dramatic lie) | Crossed fingers in front of a swirling, dramatic speech bubble |
| `images/sealed.png` | Front of the "Tap to reveal" card | Envelope closed with a question-mark wax seal |
| `images/stage.png` | After the reveal, next to the "Sell it" tips | Microphone in a spotlight on a stage |

The app is golden yellow (`#F9DC5C`, `#FAE588`, `#FCEFB4`, `#FDF8E1`). The illustrations use the complementary blues and gray, on a plain pale-cream background (`#FDF8E1`) so they sit seamlessly inside the app's cream frames. That avoids depending on transparent PNGs.

## Shared style (paste with the first prompt)

```
Style for every image in this chat:
Flat vector illustration with a subtle grain texture, rounded geometric shapes, bold clean silhouettes, playful and friendly mood, like a modern editorial spot illustration.
Strict color palette: royal blue #0057FF, sky blue #00A8FF, light cyan #65D8FF, cool gray #C4CCD6, plus white and a deep navy #0A1A3F used only for outlines and shadows. Do not use yellow, gold, orange or any warm color in the subject.
Background: one solid flat pale-cream color, exactly #FDF8E1, edge to edge. No gradient, no vignette, no border, no frame, no drop shadow on the background.
Square 1:1 image. Subject centered with generous empty margin (about 12% on every side) so it can be cropped into a rounded square.
Absolutely no text, letters, words, numbers or logos anywhere.
```

## Prompts

Send them in this order **in the same ChatGPT conversation**, so the style stays consistent.

1. **hero.png**
   ```
   [Shared style above]

   Image 1: A charismatic, gender-neutral cartoon person shown from the waist up, smiling confidently at the viewer and holding a theater mask up beside their face with one hand. Their other hand is behind their back, and we can see its fingers are crossed. A few small sparkles around them.
   ```
2. **truth.png**
   ```
   Same style, same palette, same cream #FDF8E1 background, no text.
   Image 2: A glowing lightbulb with a tiny surprised face (wide eyes, round open mouth) bursting up out of an open gift box, with small stars and bits of confetti. The feeling of "a surprising fact".
   ```
3. **lie.png**
   ```
   Same style, same palette, same cream #FDF8E1 background, no text.
   Image 3: A big, swirling, dramatic empty speech bubble with a long curly twisting tail, and in front of it a hand with crossed fingers. Small lightning bolts and drama sparkles around it. The feeling of "a dramatic lie".
   ```
4. **sealed.png**
   ```
   Same style, same palette, same cream #FDF8E1 background.
   Image 4: A closed envelope seen from the front at a slight playful tilt, sealed with a round wax seal stamped with a question-mark symbol (the question mark is the only mark allowed; no other text). A few small sparkles suggest a secret inside.
   ```
5. **stage.png**
   ```
   Same style, same palette, same cream #FDF8E1 background, no text.
   Image 5: A vintage microphone on a stand, lit by a cone of spotlight on a simple stage floor, with the soft edge of a gray curtain at one side. The feeling of "your turn to perform".
   ```

---

## Steps for Claude (Claude in Chrome)

1. **Load the browser tools in one call:**
   `ToolSearch` with `select:mcp__claude-in-chrome__tabs_context_mcp,mcp__claude-in-chrome__tabs_create_mcp,mcp__claude-in-chrome__navigate,mcp__claude-in-chrome__computer,mcp__claude-in-chrome__read_page,mcp__claude-in-chrome__find,mcp__claude-in-chrome__get_page_text`
2. **Open ChatGPT in a new tab** (`tabs_context_mcp`, then `tabs_create_mcp`, then navigate to `https://chatgpt.com/`). Don't touch the user's other tabs.
   - If ChatGPT shows a sign-in page, stop and ask the user to sign in in that tab, then continue. Never type credentials.
   - Start a new chat. If a model or tool picker is visible, make sure image generation is available (the default model generates images).
3. **Before downloading anything, ask the user once in chat** for permission to download the five PNGs from chatgpt.com into `~/Downloads`. Wait for a yes.
4. **For each prompt, in order:**
   1. Click the message box, type the prompt (for prompt 1, the shared style block followed by the Image 1 text), and send it.
   2. Wait for the image to finish. Generation takes about 30–90 seconds. Check with a screenshot roughly every 15 seconds, for up to 3 minutes.
   3. **Check the image** against the brief: square, cream background, only blues/grays/white/navy in the subject, no text (except the question mark on the seal), the subject well inside the edges. If it fails, send one follow-up asking for that specific fix (for example: "Same image, but remove the text and make the background flat #FDF8E1"). Accept the second try unless it is clearly worse.
   4. **Download it:** hover over the image and click its download button (or open the image and use the download button there). Note the downloaded file name.
5. **Move the files into the project** with Bash. Convert to PNG and cap the size at 1024px:
   ```bash
   cd "/Users/rossavilla/GitHub/Liar Game" && mkdir -p images
   # repeat for each file, newest download first; check names with: ls -t ~/Downloads | head
   sips -s format png -Z 1024 "$HOME/Downloads/<downloaded file>" --out images/hero.png
   ```
6. **Verify** the five files:
   ```bash
   cd "/Users/rossavilla/GitHub/Liar Game/images" && for f in hero truth lie sealed stage; do sips -g pixelWidth -g pixelHeight "$f.png" | tail -2 | tr '\n' ' '; echo " $f"; done
   ```
   Each should be square and at most 1024px. Each should be under ~1.5 MB; if one is larger, shrink it with `sips -Z 768 images/<name>.png`.
7. **Look at them in the app.** Open `index.html` in the built-in browser at phone size (preset `mobile`), go through the welcome, both steps and the reveal card, and take screenshots. Check that each illustration fills its frame with no stray background edge.
8. Tell the user which images were made, any that needed a retry, and anything that still looks off. Leave the ChatGPT tab open.
