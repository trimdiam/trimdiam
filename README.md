# Trimdiam

Personal website of Trimdiam, a civil engineer, teacher and builder in Shillong, Meghalaya.

The whole site is one file, `index.html`, served by GitHub Pages. No build step and no framework.

- **Home:** a circuit board you can explore (each gold pad opens a section), plus live panels.
- **Engineering, Projects, Lab, Teaching, Music, Contact:** the rest of the site.
- **Lab demos:** a fingerprint attendance simulator, a seeded (verifiable) housie draw, and a Python "predict the output" quiz.
- **Ask the bench:** an optional AI assistant. The Cloudflare Worker that powers it is in [`ask-worker/`](ask-worker/), with setup steps in its README.

To change the site, edit `index.html`, commit and push. GitHub Pages republishes it within a minute or two.
