# AGENTS.md: Working on the Walton Robotics Website

This is the website for **FIRST Team 2974, Walton Robotics**. It is maintained by
high school students on the team, often with help from AI coding assistants.

This file is for **both**:

- **Students:** read it before your first change. It explains where things live and how to get your work onto the site.
- **AI agents** (Claude Code, Copilot, Cursor, etc.): follow it when you help a student. See [Rules for AI agents](#rules-for-ai-agents).

---

## How the site works (the 30-second version)

- The site is built with **[Jekyll](https://jekyllrb.com/)**, which turns Markdown and YAML files into a website.
- It is hosted for free on **GitHub Pages** at the `WaltonRobotics/WaltonRobotics.github.io` repo. Whatever is on the `main` branch is the live site.
- The look comes from the **[TeXt theme](https://kitian616.github.io/jekyll-TeXt-theme/)** (loaded remotely), plus our own team styling layered on top.
- **Most content is in plain text files.** For most changes you don't need to know HTML or CSS.

## Where things live

| I want to change... | Edit this file |
|---|---|
| Homepage text (hero, "Who we are", outreach cards, sponsors) | `_data/home.yml` |
| A main page (About, Outreach, Robots, Sponsors, Contact) | `_pages/<name>/index.md` |
| News posts | `_posts/YYYY-MM-DD-short-title.md` |
| The menu at the top | `_data/navigation.yml` |
| Social links, leadership list, footer tagline, "Support WRT" button | `_config.yml`, **only under the `wrt:` section** |
| Sponsor logos on the homepage | `_data/home.yml` → `sponsors.logos` (images go in `assets/sponsors/`) |
| Images | `assets/<section>/`, for example `assets/robots/`, `assets/outreach/`, `assets/news/` |

Files you'll rarely need: `_layouts/`, `_includes/` (header, footer, icons), and
`_sass/custom.scss` (team colors, dark mode). You may change them, but keep the
change small and explain it in your PR.

### Do not change without a mentor's OK

- **`Gemfile` and `Gemfile.lock`**: these pin the Ruby and Jekyll versions so local builds match GitHub Pages. Changing them can break the live site.
- **The core settings in `_config.yml`**: `remote_theme`, `plugins`, `url`, `baseurl`, `permalink`, `defaults`, `exclude`, `markdown`, etc. Only the `wrt:` section (and `title`/`description` wording) is open for normal edits.
- Never commit the `_site/` folder. It's generated output and is already in `.gitignore`.

---

## Common tasks

### Write a news post

1. Create `_posts/2026-11-22-short-title.md`. Use the real date. The filename **must** start with `YYYY-MM-DD-`.
2. Start the file with front matter:

   ```yaml
   ---
   title: "Our Week at the Peachtree Regional"
   date: 2026-11-22 18:00:00 -0500
   tags: [competition]
   ---
   ```

   (You don't need `layout:`. Posts get the `article` layout automatically.)
3. Write the first paragraph as a summary, then put `<!--more-->` on its own line. Everything above that line is shown as the preview on the News page.
4. Write the rest in Markdown.

### Fill in or edit a main page

Open `_pages/<name>/index.md`. Keep the front matter (`title`, `permalink`) at the
top and replace "Content coming soon." with Markdown.

### Add a new page

1. Create `_pages/<new-page>/index.md` with:
   ```yaml
   ---
   title: Awards
   permalink: /awards/
   ---
   ```
2. If it should appear in the top menu, add it to `_data/navigation.yml`:
   ```yaml
     - title: Awards
       url: /awards/
   ```

### Update homepage content, sponsors, or social links

- **Homepage:** edit `_data/home.yml`. Outreach card icons can only be `person`, `grid`, or `gear` (defined in `_includes/wr/icon.html`).
- **Sponsor logo:** add the image to `assets/sponsors/`, then add an entry under `sponsors.logos` in `_data/home.yml` (the format is in the comment there). Lower `placeholders` as real logos are added.
- **Social link:** in `_config.yml` under `wrt.social`, fill in the `url:`. Entries with no URL show as greyed-out placeholders.
- **Leadership:** in `_config.yml` under `wrt.leadership`. Use **role/team email addresses only** (see privacy rules below).

> ⚠️ After editing `_config.yml`, **stop and restart** `jekyll serve`. It doesn't reload that file automatically.

### YAML tips (for `_data/*.yml` and `_config.yml`)

- Indentation is **spaces, never tabs**, and it matters.
- Put text with a `:` or `#` in it inside quotes: `title: "Build Season: Week 1"`.
- Long paragraphs use `>-` followed by indented lines, like the existing entries.
- When adding an item to a list, copy an existing item and change it.

---

## Content rules

### Privacy (required)

- **Students: first names only.** No last names anywhere on the site: text, captions, image filenames, or alt text.
- **No personal contact info** for anyone: no personal emails, phone numbers, home addresses, or personal social media. Use team addresses like `contactus@waltonrobotics.org`.
- If you're unsure whether something is OK to publish, ask a mentor before you open the PR.

### Voice and tone

- **Main pages** (Home, About, Outreach, Robots, Sponsors, Contact): professional, polished, in the team's voice ("we"). Match the tone of the homepage text in `_data/home.yml`. This is what sponsors and judges read.
- **News posts:** friendlier and more personal is fine. Still use complete sentences, no inside jokes, and nothing you wouldn't want a sponsor to read.
- Write "Team 2974" or "Walton Robotics" (not "WRT" on its own in body text), and *FIRST* the way FIRST styles it.

### Images

- **Resize and compress before adding:** at most **1600px wide** and under **~500 KB** each. Tools like [Squoosh](https://squoosh.app/) work in the browser. Use `.jpg` for photos and `.png` for logos or graphics.
- Store them in `assets/<section>/` with lowercase, dash-separated names: `assets/robots/watergate-2026.jpg`.
- Always write alt text: `![WaterGate on the field at Peachtree](/assets/robots/watergate-2026.jpg)`.

---

## Setup and workflow

We use a **fork + pull request** workflow. Nobody pushes directly to the team repo's `main`.

### One-time setup

1. Fork `WaltonRobotics/WaltonRobotics.github.io` on GitHub to your own account.
2. Clone **your fork**, then add the team repo as `upstream`:
   ```bash
   git clone https://github.com/<your-username>/WaltonRobotics.github.io.git
   cd WaltonRobotics.github.io
   git remote add upstream https://github.com/WaltonRobotics/WaltonRobotics.github.io.git
   ```
3. Install Ruby (3.x) and Bundler, then install the site's dependencies:
   ```bash
   gem install bundler
   bundle install
   ```

### Every change

```bash
git checkout main
git pull upstream main                # get the latest team version
git checkout -b add-awards-page       # new branch with a short, descriptive name
bundle exec jekyll serve              # preview at http://localhost:4000
# ...make your edits and check them in the browser...
git add <files you changed>
git commit -m "Add awards page"
git push origin add-awards-page       # pushes to YOUR fork
```

Then open a pull request from your branch to `WaltonRobotics/main` on GitHub
(or run `gh pr create`). **A mentor reviews and merges it.** Once it's merged,
the live site updates within a few minutes.

### PR checklist

Before asking for review, confirm:

- [ ] `bundle exec jekyll serve` builds **with no errors**, and you looked at your change in the browser.
- [ ] You checked the page at **phone width** (narrow the browser window, or use your browser's device toolbar).
- [ ] You **proofread** the text (read it aloud or run a spell check).
- [ ] For anything visual, you included a **screenshot** in the PR description (before/after if you changed something that already existed).
- [ ] The privacy rules above are followed: first names only, no personal contact info.
- [ ] Images are resized, compressed, and have alt text.
- [ ] You didn't change `Gemfile`, `Gemfile.lock`, or core `_config.yml` settings (or a mentor already approved it).

---

## Rules for AI agents

The person you're helping is most likely a **high school student**, possibly new
to Git, Markdown, or programming. Your job is to get the change right **and**
help them learn the codebase.

1. **Explain as you go.** Do the work, then briefly say what you changed, which file it's in, and why. Point them to the "Where things live" table so they can do it themselves next time. Skip jargon, or define it in one line when you use it.
2. **Prefer data and content files over code.** If a change can be made in `_data/`, `_pages/`, `_posts/`, or the `wrt:` section of `_config.yml`, do it there instead of editing layouts, includes, or CSS.
3. **Respect the guardrails.** Don't modify `Gemfile`, `Gemfile.lock`, or core `_config.yml` settings. If the task seems to require it, stop and tell the student to check with a mentor. Don't add Jekyll plugins: GitHub Pages only supports a fixed set.
4. **Enforce the content rules.** Remove or flag student last names and personal contact info, even if the student supplied them. Match the voice rules (professional main pages, friendlier news). Check that images are reasonably sized and have alt text.
5. **Build and check.** Run `bundle exec jekyll build` (or `serve`) after your changes and fix any errors or new warnings before calling the work done. Restart the server after any `_config.yml` edit. If Ruby or the build won't run, say so plainly. Don't claim it works.
6. **Git: you may run it, but explain every command.** You may create branches, commit, push to the student's **fork** (`origin`), and open a PR with `gh pr create`. Before each command, say in one sentence what it does.
   - **Never** push to `upstream` or to any `main` branch.
   - **Never** force-push, rewrite published history, or run `git reset --hard` / `git clean` without the student's explicit OK.
   - Use small, focused commits with clear messages ("Add 2026 Peachtree recap post").
   - Fill the PR description with a summary and the PR checklist above. Remind the student to add screenshots and that a mentor will review.
7. **Keep changes small and on-topic.** Don't reformat, rename, or "clean up" files beyond what was asked. Don't commit `_site/`.
8. **When unsure, ask.** If a request is ambiguous, involves privacy, or touches something risky, ask the student, or suggest they ask a mentor, before you go ahead.
