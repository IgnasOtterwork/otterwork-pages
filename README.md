# Otter Work — marketing site

The public landing page for **Otter Work**, an AI decision layer that predicts the spare
parts a truck repair will need before the vehicle reaches the workshop bay.

Static site, deployed via **GitHub Pages**, served at **https://otterwork.app**.

## Design

The page reuses the **"Depot"** design language from the product
(`servicelead/docs/modern-design.md`): warm paper ground, two type voices
(Newsreader serif for human language, Familjen Grotesk for system labels),
pine/brass palette, layered warm shadows, and the otter-and-wrench mascot.

## Structure

```
index.html            Single-page landing (hero → problem → solution → product → tour → integrations → value → pricing → CTA)
assets/css/style.css  Depot design tokens + components
assets/img/otter.svg  Otter mascot (from the product brand assets)
assets/img/favicon.svg
CNAME                 Custom domain: otterwork.app
.nojekyll             Serve files as-is (skip Jekyll processing)
```

## Local preview

Any static server works, e.g.:

```bash
python3 -m http.server 8080
```

Then open http://localhost:8080.

## Deploy

Deployment and custom-domain steps are in [`DEPLOY.md`](DEPLOY.md).
