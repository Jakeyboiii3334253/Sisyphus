# Sisyphus & the Fly

A single page: a small neural network (~49 weights) learns, by neuroevolution,
to control a fruit fly pushing a boulder up a hill that gets steeper the
higher it goes. It renders as a real 3D scene (three.js) that you can drag to
orbit and scroll to zoom, and it runs entirely client-side — no backend, no
API keys, no data files. It does load the three.js library itself from a
public CDN, so it needs an internet connection at runtime (Netlify hosting
the page doesn't change that — the browser fetches three.js separately).

Press **Start sim** to begin training and watch the fly live in 3D. Press
**Stop sim** at any point and it opens a summary screen with the full
fitness chart for the whole run and stats on how far it got (best height
ever reached, generations trained, stuck/reset count, and more). You can
resume training from the summary screen or close it and keep looking around.

## About the "fly brain"

There isn't a lightweight version of a real open-source fruit-fly AI model
that runs in a static webpage. The actual open Google/Janelia project in this
space is **FlyWire** — a connectome mapping ~140,000 real neurons and their
wiring in a Drosophila brain — built for research-scale simulation, not
browser deployment. What's running here instead is a small neuroevolved
network built from scratch for this simulation: a fair and honest stand-in,
not a repackaging of that dataset.

## How the learning works

- A population of 40 tiny networks is evaluated every generation by
  simulating an 18-second episode each (physics: gravity along the slope,
  friction, a stamina meter that drains when pushing hard and regenerates
  when idle).
- Fitness rewards *sustained* height, not just a single touch of the top —
  so a good fly has learned to pace itself, not just sprint once.
- The best 4 networks survive unchanged (elitism); the rest of the next
  generation comes from crossover + mutation of top performers.
- The canvas always shows the current best-ever network controlling the fly
  live, in real time, while training keeps running underneath it.

## Deploying to Netlify

This is a plain static site (just `index.html`), so any of these work:

1. **Fastest:** go to [app.netlify.com/drop](https://app.netlify.com/drop)
   and drag this folder (or just `index.html`) onto the page. It deploys
   immediately and gives you a live URL.
2. **Git-based:** push this folder to a GitHub/GitLab repo, then in Netlify
   choose "Import an existing project" and point it at the repo. No build
   command is needed — leave the publish directory as the repo root (or
   wherever `index.html` lives).
3. **Netlify CLI:** `netlify deploy --prod` from inside this folder.

No environment variables, build steps, or backend are required.
