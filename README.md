# Python Burgers Game

This repository contains a Hugo-powered tutorial site for the Pygame Zero burgers project. It is not a standalone Python game; instead, it publishes the step-by-step lesson content, screenshots, and burger artwork used throughout the tutorial.

## What’s Included

- Tutorial content under `content/docs/navigation/`
- Site configuration in `hugo.toml`
- Burger artwork in `code/burgers/burgers/`
- Screenshots and other site images in `static/_images/`
- The Hugo Book theme in `themes/hugo-book/`

## Project Structure

```text
python-burgers-games/
├── content/              # Hugo content for the tutorial site
├── static/               # Static site assets such as screenshots
├── code/burgers/burgers/ # Burger image assets used by the tutorial
├── themes/hugo-book/     # Hugo theme
├── hugo.toml             # Hugo site configuration
├── Makefile              # Convenience target for running the site
└── README.md
```

## Prerequisites

- [Hugo](https://gohugo.io/) installed locally
- A desktop browser for previewing the site

## Download / Clone

### Option 1: Clone with Git (recommended)

```bash
git clone https://github.com/CoderDojoBrighton/python-burgers-games.git
cd python-burgers-games
git submodule update --init --recursive
```

The Hugo Book theme is tracked as a Git submodule, so run the submodule update command after cloning to fetch the theme files.

### Option 2: Download ZIP

1. Open the repository on GitHub.
2. Click **Code** and choose **Download ZIP**.
3. Extract the archive.
4. Open a terminal in the extracted folder.

## Run the Site

From the project root, start the local Hugo server:

```bash
hugo server
```

You can also use the provided Makefile target:

```bash
make docs
```

Then open the local preview URL shown in the terminal.

## Editing the Tutorial

- Add or update lesson pages in `content/docs/navigation/`.
- Update the landing page in `content/_index.md`.
- Replace or add tutorial screenshots in `static/_images/`.
- Replace burger artwork in `code/burgers/burgers/` if the lesson assets change.

## Testing and Previewing

The main check for this repository is a clean Hugo build and preview.

- `hugo server` should start without errors.
- Lesson pages should render correctly in the browser.
- Images referenced from the content should resolve correctly.

## Troubleshooting

### `hugo: command not found`

Install Hugo and make sure it is on your `PATH`.

### Pages or images are missing

- Check the referenced file paths in the Markdown front matter and body.
- Confirm the image exists under `static/_images/` or `code/burgers/burgers/` as appropriate.

## Contributing

Contributions are welcome.

- Keep changes focused on a single lesson or topic.
- Update screenshots or asset paths when content changes.
- Preview the site locally before opening a pull request.

## License

If this repository includes a license file, its terms apply. If no license file is present, ask the maintainers before reusing or redistributing the content or artwork.