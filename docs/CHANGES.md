# Customization Log & Changes Track

This document tracks all customizations, disabled features, and configuration tweaks made to the **al-folio** website template. Use this as a reference if you want to restore any functionality in the future.

---

## 1. Commented-out Collections & Features (`_config.yml`)

### Blog & Newsletter

The following sections in `_config.yml` have been commented out to completely disable and hide the blog and newsletter functionalities:

- **Blog settings**: `blog_name`, `blog_description`, `permalink`, `lsi`
- **Pagination**: `pagination` (enabled: true)
- **Related posts**: `related_blog_posts`
- **Comments**: Giscus and Disqus configurations
- **External sources**: RSS feed ingestions (Medium, Google Blog)
- **Newsletter**: Loops.so newsletter integration

### Collection Output Configuration

In the `collections:` configuration block, output generation has been set to `false` for:

- `books: output: false` (hides book collection generation)
- `teachings: output: false` (hides teachings/courses generation)

---

## 2. Excluded Directories & Pages (`_config.yml`)

To prevent default templates and unused files from being compiled into the final site, the following folders and files were added to the `exclude:` list under the `# Added by me` section:

- `_posts/` (Blog posts folder)
- `_pages/blog.md` (Blog landing page)
- `_news/` (News/announcements folder)
- `_pages/news.md` (News landing page)
- `_pages/publications.md` (Publications landing page)
- `_teachings/` (Teachings/courses folder)
- `_pages/teaching.md` (Teaching landing page)
- `_books` (Books folder)
- `_pages_books.md` (Bookshelf page)
- `_pages/dropdown.md` (Dropdown menu configuration)
- `_pages/profiles.md` (Profiles page)
- `docs/CHANGES.md` (This file)

---

## 3. Component & Layout Customizations

### CV Page (Multilingual English/Spanish PDFs)

- **File**: `_pages/cv.md`
- **Change**:
  - Commented out the single `cv_pdf` field.
  - Added HTML and scoped CSS to the frontmatter `description` block to render two custom styled language buttons (`English Edition` and `Edición en Español`) linking to `../assets/pdf/cv_english.pdf` and `../assets/pdf/cv_spanish.pdf`.
  - Used scoped CSS (`.post .cv-pdf-btn`) and single-quoted HTML attributes to prevent duplicate buttons from rendering due to metadata tags in `<head>`.

### Repositories Page (Personal Stats & Projects)

- **Files**: `_config.yml`, `_data/repositories.yml`, `_pages/repositories.md`
- **Changes**:
  - Configured `github_readme_stats_url` to point to a custom Vercel app instance (`https://github-stats-extended-alemaniaco.vercel.app/`).
  - Commented out `github_profile_trophy_url`.
  - Configured `github_users` to target `andresalemn`.
  - Updated `github_repos` to display personal repositories (`arduino-bot`, `uni-3dof-manipulator`, `robotics-sandbox`, `ModernRobotics`).

### Projects Page (Custom Projects Grid)

- **Files**: `_pages/projects.md`, `_projects/`
- **Changes**:
  - Cleaned up and deleted default template projects (`2_project.md`, `3_project.md`, `5_project.md`, `6_project.md`, `7_project.md`, `8_project.md`, `9_project.md`).
  - Added/Renamed personal projects: `uni-manipulator.md` (renamed from `4_project.md`), `arduino-bot.md`, and `pr3.md`.
  - Added project cover images to `assets/img/`.

### Footer Text

- **File**: `_config.yml`
- **Change**: Removed the "Photos from Unsplash" attribution from the `footer_text` field.

---

## 4. GitHub Actions Workflows

### Disabled Integration Tests

- **File**: `.github/workflows/unit-tests.yml`
- **Change**: Commented out the **comments** and **distill** integration test runs.
  - _Reason_: These tests check the generated HTML paths for sample blog posts. Since the blog collection has been excluded and hidden, these tests would fail CI builds.

---

## 5. Reference Commit Log

Below is the log of the initial commits where these changes were introduced:

- `828789d`: Custom English/Spanish CV download buttons in `cv.md` and disabled comments/distill integration tests.
- `4d98b6b`: Customized repositories page with personal Vercel app stats and selected repositories.
- `6030239`: Cleared default project templates and added custom project pages/covers.
- `d32a4a1`: Edited footer text (removed Unsplash credit).
- `e34f622`: Configured the first custom project page (`pr3.md`).
- `234c8a1`: Added social links, edited socials configuration, and removed `_bibliography` from excludes list.
- `9dbf25d`: Main customization commit that disabled blog, newsletter, news, teachings, books collections, dropdown menu, profiles, and added excludes.
- `719dd56`: Added `.nojekyll` file to bypass GitHub Pages default Jekyll pipeline.
