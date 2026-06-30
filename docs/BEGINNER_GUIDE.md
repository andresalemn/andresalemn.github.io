# Beginner's Customization & Deployment Guide

This guide is designed for beginners to customize the `al-folio` template, test changes locally, avoid merge conflicts when syncing with original maintainers, and safely deploy updates to the live website.

---

## 1. Directory Strategy: What to Edit vs. What to Avoid

To easily pull future updates, features, and security patches from the original `al-folio` maintainers without dealing with complex merge conflicts, structure your edits according to the table below.

### 🟢 Safe to Customize (Your Content & Settings)
These files and directories contain your personal data and content. Modifying them will **not** conflict with upstream template code updates:

| File/Directory | Description | What to do with it |
| :--- | :--- | :--- |
| `_config.yml` | Main website configuration | Edit this to toggle features, change the site title, description, and configure main settings. |
| `_data/` | Data folders for YAML profiles | Edit `socials.yml` (social links), `repositories.yml` (featured GitHub repos), and `cv.yml` (CV entries if not using JSON resume). |
| `assets/` | Static media, images, & CV files | Add profile photos (e.g., `assets/img/prof_pic.jpg`), PDFs, and customize `assets/json/resume.json` (JSON Resume format). |
| `_pages/` | Main website pages | Modify files like `about.md` (your homepage description), `blog.md`, `projects.md`, etc. |
| `_posts/` | Blog posts | Add/edit Markdown files here to write blog articles. |
| `_projects/` | Projects showcase | Add/edit Markdown files to showcase research projects, software, or portfolios. |
| `_news/` | News updates | Create files for short news announcements displayed on your homepage. |
| `_bibliography/papers.bib` | Publication list | Paste your publication list in BibTeX format here to automatically render publications. |

### 🔴 Avoid Editing (Core Framework & Template Code)
These folders contain the build environment, pipelines, and runtime files. **Avoid** modifying these files unless you are comfortable resolving merge conflicts or require advanced customizations:

*   **System Files**: `Gemfile`, `Gemfile.lock`, `package.json`, `package-lock.json`. These manage programming libraries (Ruby Gems and Node packages) that run the site.
*   **Docker Configuration**: `Dockerfile`, `docker-compose.yml`, `docker-compose-slim.yml`.
*   **CI/CD Workflows**: `.github/workflows/` (manages automated deployment scripts).
*   **Layouts & Includes**: `_layouts/` and `_includes/`. In `al-folio` v1.x, layouts and site structures are packaged into independent gems (like `al_folio_core`). While you can create local files here to override them, doing so shields you from future layout improvements and creates overrides that must be tracked.
*   **Tests & Helpers**: `bin/`, `test/`, and linter configurations (e.g., `.prettierrc`, `.prettierignore`).
*   **Agent Assist Tools**: `.agents/`, `.claude/`, `.codex/`, `.gemini/` directories.

---

## 2. Step-by-Step Customization Workflow

Follow this cycle whenever you want to change a feature, add content, or tweak configuration settings.

### Step 1: Make Your Changes
Locate the file you want to edit in the **Safe** directories.
*   *Example:* Open `_config.yml` to change the `title` of your site, or open `_pages/about.md` to edit your biography.

### Step 2: Build and Test Locally (Using Docker)
Testing locally ensures your changes don't crash the build before updating the live site. Docker is the recommended setup as it isolates dependencies.

1.  **Start your Docker container** by running this command in your terminal from the repository root:
    ```bash
    docker compose pull
    docker compose up
    ```
2.  **Open your browser** and visit:
    [http://localhost:8080](http://localhost:8080)
3.  **Verify your changes**:
    *   **Live Updates:** For most content changes (modifying `.md` files in `_pages/`, `_posts/`, `_projects/`), the server automatically rebuilds the site in real-time. Simply save the file and refresh your browser page.
    *   **Config Updates:** If you modified `_config.yml` or files in `_data/`, you **must restart the server** for changes to take effect. Press `Ctrl + C` in your terminal to stop the server, and then run `docker compose up` again.
    *   **Cache issues:** If changes do not appear, perform a **Hard Refresh** (`Shift + F5` or `Ctrl + F5` on most browsers) or open the local link in a private/incognito window.

### Step 3: Run Linters & Checks (Optional but Recommended)
Before committing, format your code to keep it clean and standard. Run:
```bash
npm run lint:prettier
```
If you intentionally created local overrides of theme-packaged layout components, audit them using:
```bash
bundle exec al-folio upgrade overrides audit
```

### Step 4: Review Your Git Status
Run the following git command in your terminal to see exactly which files were created, modified, or deleted:
```bash
git status
```
*   Ensure that **only** files in the **Safe to Customize** category are modified. If you see core files in `_layouts/`, `test/`, or `Gemfile` modified by accident, you can discard those changes before proceeding.

Review a summary of changes line-by-line using:
```bash
git diff
```

### Step 5: Commit and Push
Once you are confident the changes look correct locally and there are no syntax errors, commit them to your repository:

1.  **Stage your changes** (mark them ready to commit):
    ```bash
    git add .
    ```
2.  **Commit the changes** with a clear description of what you did:
    ```bash
    git commit -m "Update biography in about.md and add custom social links"
    ```
3.  **Push to your remote repository** (this updates the actual website):
    ```bash
    git push origin main
    ```

> [!NOTE]
> Once pushed, GitHub Actions automatically builds and deploys your site to GitHub Pages. It may take 1-3 minutes for the updates to appear live at `<your-username>.github.io`.
> 
> **Pro-Tip (Skip Build for Docs):** If you are only updating files that are not visible on the website (like this guide, instructions, or `README.md`), you can add **`[skip ci]`** or **`[ci skip]`** to your commit message to prevent running the deployment pipeline. For example:
> `git commit -m "Update beginner guide [skip ci]"`

---

## 3. How to Sync Upstream Changes (Fetching Original Updates)

As the maintainers of `al-folio` release new functionalities and bug fixes, you can pull those updates into your site. Since you avoided editing the core files, this process is usually seamless.

1.  Add the original template repository as a remote source named `upstream` (you only need to do this once):
    ```bash
    git remote add upstream https://github.com/alshedivat/al-folio.git
    ```
2.  Fetch the latest updates from the upstream repository:
    ```bash
    git fetch upstream
    ```
3.  Merge the upstream changes into your local main branch:
    ```bash
    git checkout main
    git merge upstream/main
    ```
4.  If there are any merge conflicts, Git will guide you on how to resolve them. If you followed the directory guidelines, conflicts will be minimal or non-existent.
5.  After merging, test the site locally using `docker compose up` to ensure the new template code works with your content, then push the merged changes to your remote repository (`git push origin main`) to update your live website.

---

## 4. Excluded Files & Directories (Do Not Affect the Live Site)

The settings inside `_config.yml` define which files are ignored when Jekyll compiles your website. Changes to these files modify your repository code but **never** alter the content or layout of your live website:

*   **Documentation:** `docs/`, `README.md`, `CONTRIBUTING.md`, `CUSTOMIZE.md`, `BEGINNER_GUIDE.md`, `FAQ.md`, `INSTALL.md`, `LICENSE`, `al-folio-upgrade-report.md`.
*   **Infrastructure & Environment:** `Dockerfile`, `docker-compose.yml`, `docker-compose-slim.yml`, `requirements.txt`.
*   **Testing & Operations:** `test/`, `lighthouse_results/`, `bin/`.
*   **Dependencies & Styling Config:** `Gemfile`, `Gemfile.lock`, `package.json`, `package-lock.json`, `node_modules/`, `vendor/`, `purgecss.config.js`.
*   **Assets & Previews:** `readme_preview/`, `robots.txt`.
