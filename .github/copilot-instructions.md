<!-- Keep in sync with AGENTS.md at the repository root. -->

# Agent instructions: Containerizing .NET Applications

This repository holds the slides and demos for Chris Ayers' talk **.NET in a Box: Containerizing .NET Applications**. The deck is published to <https://chris-ayers.com/containerizing-dotnet/>.

## Repository layout

- `slides/`: Marp deck, images in `slides/img/`, themes in `slides/themes/`.
- `src/hello-containers/`, `src/SampleApi/`, `src/aspire-demo/`: the demo apps; `containerizing-dotnet.slnx` builds them.
- `demos.md`: topics and the exact demo commands used on stage.
- `.devcontainer/containers/` (demos) and `.devcontainer/slides/` (deck authoring).

## Slides

- Write the deck in Markdown with [Marp](https://marp.app/). `slides/Slides.md` is the source; `build/` is generated output and is never edited or committed.
- Keep the frontmatter at the top of the deck:

  ```yaml
  ---
  marp: true
  theme: custom-default
  ---
  ```

- Store themes in `slides/themes/` and images and diagram sources in `slides/img/`. Prefer theme classes and CSS variables over inline styles.
- Write speaker notes as HTML comments (`<!-- -->`).
- Render locally with the Marp CLI version the Pages workflow uses (v4.5.0):

  ```bash
  npx @marp-team/marp-cli@4.5.0 --theme-set slides/themes --html -o build/index.html -- slides/Slides.md
  npx @marp-team/marp-cli@4.5.0 --theme-set slides/themes --html --pdf --allow-local-files -o build/Slides.pdf -- slides/Slides.md
  ```

- `.github/workflows/marp-pages.yml` builds the HTML deck and `Slides.pdf` and publishes both to GitHub Pages. If you add slide assets outside `slides/img/` and `slides/themes/`, add them to the workflow's copy step.

## Shared skills

Use the shared skills from [codebytes/skills](https://github.com/codebytes/skills) (for Copilot CLI: `copilot plugin install https://github.com/codebytes/skills`). Don't copy their instructions or scripts into this repo.

| Task                                                                          | Skill               |
| ----------------------------------------------------------------------------- | ------------------- |
| Write or revise slides, notes, order, or layouts                              | `marp-authoring`    |
| Charts, Mermaid diagrams, or other visuals                                    | `marp-visuals`      |
| Editable `.drawio.svg` diagrams (stored in `slides/img/`)                     | `drawio-diagrams`   |
| Check overflow, clipping, and HTML/PDF parity before presenting or publishing | `marp-slide-review` |

## Build and run

```bash
dotnet build containerizing-dotnet.slnx
dotnet publish src/hello-containers -t:PublishContainer   # image via the .NET SDK container support
docker build -t sampleapi src/SampleApi                 # image via the Dockerfile
```

## Talk-specific rules

- `demos.md` is the source of truth for demo commands; update it when a demo changes.
- The image workflows sign images with cosign and push to a registry; keep registry credentials in repository secrets.

## Readme

- Keep the readme sections in this order: Slides, Recordings, Resources, Contact.
- When a new recording of the talk is published, add it at the top of `## Recordings` as `- [Event Year](https://www.youtube.com/watch?v=VIDEO_ID)`. When the video's title differs from the talk's, add an em dash and the recorded title after the event. Keep the list newest first.
- The talk also appears on <https://chris-ayers.com/speaking/> (source: `content/speaking.md` in [codebytes/codebytes.github.io](https://github.com/codebytes/codebytes.github.io)); new events and recordings belong there too.

## General

- Keep changes scoped to the request; don't reformat unrelated slides or code.
- Never commit secrets, keys, or connection strings. Demo placeholders must be obviously fake.
