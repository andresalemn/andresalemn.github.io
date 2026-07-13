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

## 5. CV Web Rendering System (Plugin Overrides)

### Personal CV Data Migration (`_data/cv.yml`, `_data/cv_es.yml`)

- **Files**: `_data/cv.yml`, `_data/cv_es.yml`
- **Changes**:
  - Replaced the Albert Einstein placeholder content in `_data/cv.yml` with real personal data: name, headline, location, email, website, Experience (Inbiodroid), Projects (3-DOF Manipulator, Alexa Robot, Blind Spot Detection), Skills, Education (Universidad Anáhuac Mayab — BS Mechatronics + Cultural Management Diploma), Certificates (Industrial Robotics Integrator), Complementary Education, and LinkedIn social link.
  - Created `_data/cv_es.yml` as a full Spanish translation of the above.
  - Both files use the RenderCV YAML schema (`design:` + `cv:` + `sections:` structure).

### CV Page Updates (`_pages/cv.md`, `_pages/cv_es.md`)

- **Files**: `_pages/cv.md`, `_pages/cv_es.md`
- **Changes**:
  - Added `cv_source: cv` frontmatter to `cv.md` to explicitly load `_data/cv.yml`.
  - Updated download buttons: English page now links to `cv_english.pdf`; Spanish button replaced with a web link to `/cv/es/` (globe icon).
  - Created `_pages/cv_es.md` at permalink `/cv/es/` with `cv_source: cv_es` to serve the Spanish web CV using the same layout. Includes a "Descargar PDF" button for `cv_spanish.pdf` and a back-link to the English web edition.

### RenderCV Pipeline Disabled

- **Files**: `.github/workflows/render-cv.yml`, `assets/rendercv/design.yaml`, `assets/rendercv/settings.yaml`
- **Change**: Entire contents of all three files commented out to disable automatic PDF rendering via GitHub Actions. The CV PDFs (`cv_english.pdf`, `cv_spanish.pdf`) are managed manually in `assets/pdf/`.
- **Deleted**: `assets/rendercv/locale.yaml`, `assets/rendercv/rendercv_output/Albert_Einstein_CV.pdf`, `assets/pdf/example_pdf.pdf`.

### Plugin-Owned Include Overrides (`_includes/cv/`)

> ⚠️ These files are owned by the `al_folio_core` plugin. Local overrides are tracked in `.al-folio-overrides.yml`.

- **Modified**: `_includes/cv/render.liquid`
  - Added Spanish section-name dispatch (`Formación`, `Experiencia`, `Certificaciones`, `Proyectos`, `Voluntariado`, `Idiomas`, `Intereses`, `Referencias`, `Publicaciones`, `Premios`) so both `cv.yml` and `cv_es.yml` route to the correct partial includes.
  - Added plain-string fallback in the generic section renderer — fixes empty `Summary` / `Perfil Profesional` sections.
  - Fixed a bug where the JSONResume `awards` block incorrectly included `cv/education.liquid` instead of `cv/awards.liquid`.

- **Created** (new partials, not present in plugin):
  - `_includes/cv/experience.liquid` — renders company, position, location, dates, highlights.
  - `_includes/cv/education.liquid` — renders institution, degree, area, location, dates, summary.
  - `_includes/cv/awards.liquid` — renders title/name, awarder/issuer, date, summary.
  - `_includes/cv/publications.liquid` — renders name, publisher, date, url, summary.
  - `_includes/cv/languages.liquid` — renders language name and fluency/level.
  - `_includes/cv/interests.liquid` — renders name and keywords.
  - `_includes/cv/references.liquid` — renders name, icon, reference text.

---

## 6. Reference Commit Log

Below is the log of the initial commits where these changes were introduced:

- `828789d`: Custom English/Spanish CV download buttons in `cv.md` and disabled comments/distill integration tests.
- `4d98b6b`: Customized repositories page with personal Vercel app stats and selected repositories.
- `6030239`: Cleared default project templates and added custom project pages/covers.
- `d32a4a1`: Edited footer text (removed Unsplash credit).
- `e34f622`: Configured the first custom project page (`pr3.md`).
- `234c8a1`: Added social links, edited socials configuration, and removed `_bibliography` from excludes list.
- `9dbf25d`: Main customization commit that disabled blog, newsletter, news, teachings, books collections, dropdown menu, profiles, and added excludes.
- `719dd56`: Added `.nojekyll` file to bypass GitHub Pages default Jekyll pipeline.
