# Big 2

A phone-first web version of the card game Big 2 (Big Two / Cho Dai Di). You play against three bots. It uses standard rules and keeps a running score.

Everything is in one self-contained file, `index.html`: game engine, bots and UI. There is no build step and nothing to install. Open it in a browser, or serve it with GitHub Pages.

## Run locally

```sh
python3 -m http.server 8000
# open http://localhost:8000 (or your machine's LAN IP from an iPhone)
```

## Host on GitHub Pages

Go to Settings → Pages → Deploy from branch → `main` / root. Note that Pages sites are public even when the repo is private, unless you're on a plan that supports private Pages.

## Rules implemented

- **Card order:** 3 low through 2 high. Suits break ties: ♦ < ♣ < ♥ < ♠.
- **First play:** whoever holds the 3♦ leads, and the first play must include it.
- **Valid plays:** singles, pairs, triples, and five-card hands. The five-card hands rank, low to high: straight, flush, full house, four of a kind plus one card, straight flush.
- **Straights:** A is high or low. 10-J-Q-K-A is the top straight and A-2-3-4-5 the lowest. Straights don't wrap around.
- **Passing:** passing doesn't lock you out of the trick. When everyone else passes, the last player to play leads anything.
- **Scoring:** penalty points, like Hearts. When someone goes out, every other player takes 1 point per card left, 2 per card at 10 or more, 3 per card at 13. The game ends after the hand where anyone reaches 50. Lowest total wins. A tie goes to whoever won more hands; if still tied, the win is shared.

## UI notes

- **Selecting cards:**
  - Tap a card to select it, or drag across several.
  - When you're following, tapping a card also selects the cards that every legal play containing it shares. For example, tap one of three 6s against a triple and all three are picked.
- **Dimmed cards:** on your turn, cards that can't be part of any legal play are dimmed and can't be selected.
- **Auto-pass:** if nothing in your hand beats the table, you pass automatically.
- **Flush strip:** cards in a suit you hold 5 or more of get a thin strip along the bottom, so possible flushes are easy to spot.
- **End of trick:** a bar on the **Next trick** button runs down over 5 seconds and then the next trick starts. It starts right away if you tap **Next trick** or the table. If you won the trick, tapping a card in your hand also starts it. If the page loses focus, the countdown is cancelled and the button is the only way to continue.
- **Opening lead:** 3♦ is selected for you and can't be deselected.
- **Sound:** effects are synthesized with Web Audio, so there are no audio files. The speaker button mutes them, and the choice is remembered. On iPhone, the silent switch also mutes them.
- **Game over:** a full-screen standings view, with a trophy and confetti if you win. The win celebration plays even with Reduce Motion on.
- **Saving:** game state is saved in `localStorage`, so closing the tab doesn't lose a hand.

## Dev hooks

`window.__big2` exposes the engine (`classify`, `allPlays`, `beats`, `hintList`, state `S`, and so on). Use it for scripted testing, such as Playwright simulations.
