# Logan Billet - Personal Website

A clean, academic Jekyll website for hosting on GitHub Pages.

## Deploying to GitHub Pages

### Option 1: Using username.github.io (Recommended)

1. **Create a new GitHub repository** named `yourusername.github.io` (replace `yourusername` with your actual GitHub username)

2. **Initialize and push this folder to GitHub:**
   ```bash
   cd Personal_Website
   git init
   git add .
   git commit -m "Initial commit - personal website"
   git branch -M main
   git remote add origin https://github.com/yourusername/yourusername.github.io.git
   git push -u origin main
   ```

3. **Enable GitHub Pages:**
   - Go to your repository on GitHub
   - Click **Settings** > **Pages** (in the left sidebar)
   - Under "Source", select **GitHub Actions**
   - Your site will be live at `https://yourusername.github.io`

### Option 2: Using a project repository

1. Create a new repository with any name (e.g., `website`)

2. Update `_config.yml`:
   ```yaml
   baseurl: "/website"  # Use your repository name
   ```

3. Follow steps 2-3 from Option 1, using your repository name instead

## Adding the GitHub Actions Workflow

Create `.github/workflows/jekyll.yml` with this content:

```yaml
name: Deploy Jekyll site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.2'
          bundler-cache: true
      - name: Setup Pages
        uses: actions/configure-pages@v4
      - name: Build with Jekyll
        run: bundle exec jekyll build
        env:
          JEKYLL_ENV: production
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## Local Development

To preview your site locally:

1. Install Ruby (https://www.ruby-lang.org/)
2. Install dependencies:
   ```bash
   bundle install
   ```
3. Run the development server:
   ```bash
   bundle exec jekyll serve
   ```
4. Open http://localhost:4000 in your browser

## Customizing Your Site

### Adding Publications
Edit `publications.md` and add entries following the commented format.

### Adding Your CV PDF
1. Place your CV PDF in the `assets/` folder as `Billet_CV.pdf`
2. The download link on the CV page will automatically work

### Adding Images
1. Place images in `assets/images/`
2. Reference them in markdown: `![Alt text](/assets/images/filename.jpg)`

### Updating Contact Info
Edit `_config.yml` to update your email and other site-wide settings.

## File Structure

```
├── _config.yml          # Site configuration
├── _layouts/
│   └── default.html     # Main page template
├── assets/
│   ├── css/
│   │   └── style.css    # Site styles
│   └── images/          # Your images
├── index.md             # Home page
├── research.md          # Research page
├── publications.md      # Publications page
├── cv.md               # CV page
├── Gemfile             # Ruby dependencies
└── README.md           # This file
```

## Need Help?

- Jekyll documentation: https://jekyllrb.com/docs/
- GitHub Pages documentation: https://docs.github.com/en/pages
