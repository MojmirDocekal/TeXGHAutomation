# TeXGHAutomation

Automated LaTeX compilation and review workflow for collaborative document editing.

## Overview

This repository uses GitHub Actions to automatically compile LaTeX documents into PDFs whenever changes are made. It also provides a structured review workflow to help collaborators provide feedback on the document.

## Automated LaTeX Compilation Workflow

Every time you push changes to `.tex` files on the `main` branch or create a pull request, GitHub Actions automatically:
1. Installs the necessary TeXLive packages
2. Compiles `document.tex` using `pdflatex` (runs twice to resolve references)
3. Makes the compiled PDF available as a downloadable artifact

### Viewing Workflow Status

- Go to the **Actions** tab in this repository
- Click on the most recent workflow run
- Check if the compilation succeeded (green checkmark) or failed (red X)

## For Authors

### Making Changes to LaTeX Files

1. **Edit the `.tex` files** in your local clone or directly on GitHub
2. **Commit and push** your changes to the `main` branch or create a pull request
3. **Wait for the workflow** to run (usually takes 1-2 minutes)
4. **Download the compiled PDF** from the workflow artifacts (see below)

### Downloading Compiled PDFs

To download the compiled PDF after a workflow run:

1. Go to the **Actions** tab
2. Click on the workflow run you want to download from
3. Scroll down to the **Artifacts** section at the bottom of the page
4. Click on **compiled-pdf** to download the PDF

### Referencing Issues in Commits

When addressing reviewer feedback, link your commits to the relevant issues:

- **To close an issue automatically**: Include `fixes #123` or `closes #123` in your commit message
- **To reference without closing**: Use `addresses #123` or `relates to #123`

**Examples:**
```
git commit -m "Improved mathematical notation in Section 2 (fixes #45)"
git commit -m "Added clarification to introduction (addresses #12)"
```

### Adding LaTeX Packages

If you need to use additional LaTeX packages that aren't currently installed:

1. Edit `.github/workflows/compile-latex.yml`
2. Find the "Install TeXLive" step
3. Add the required package to the `apt-get install` command
4. Commit and push the changes

**Common packages to add:**
- `texlive-publishers` - For publisher-specific document classes (IEEE, ACM, Springer)
- `texlive-science` - For scientific packages (algorithms, chemistry, physics)
- `texlive-humanities` - For linguistics and critical editions
- `texlive-fonts-extra` - For additional fonts

**Example:**
```yaml
sudo apt-get install -y \
  texlive-base \
  texlive-latex-recommended \
  texlive-latex-extra \
  texlive-fonts-recommended \
  texlive-bibtex-extra \
  texlive-publishers
```

### Finding Missing Packages from Error Logs

If compilation fails:

1. Go to the **Actions** tab and click on the failed workflow run
2. Click on the **compile-latex** job
3. Expand the "Compile LaTeX document" step
4. Look for errors like `! LaTeX Error: File 'packagename.sty' not found`
5. Search online for "packagename.sty ubuntu texlive" to find which package to install
6. Common mappings:
   - `algorithm2e.sty` → `texlive-science`
   - `IEEEtran.cls` → `texlive-publishers`
   - `beamer.cls` → `texlive-latex-recommended` (already included)

## For Reviewers

### Creating Review Issues

To submit feedback on the document:

1. Go to the **Issues** tab
2. Click **New Issue**
3. Select the **Review Comment** template
4. Fill in all the fields:
   - **Section/Location**: Where in the document (e.g., "Section 2.1", "Page 3")
   - **Description**: What needs attention
   - **Suggested Improvement**: Your recommendation
   - **Priority Level**: Critical/Important/Nice to have
5. Submit the issue

The issue will automatically be labeled with `review-comment` for easy filtering.

### Checking Compiled PDFs

Before reviewing, download the latest compiled PDF:

1. Go to the **Actions** tab
2. Click on the most recent **successful** workflow run (green checkmark)
3. Download the **compiled-pdf** artifact from the bottom of the page
4. Open the PDF and review it
5. Create issues for any feedback using the review comment template

### Filtering Review Comments

To see all review comments:
- Go to **Issues** tab
- Click on **Labels** → **review-comment**
- Or use the search: `is:issue label:review-comment`

## Repository Structure

```
.
├── .github/
│   ├── workflows/
│   │   └── compile-latex.yml      # Automated compilation workflow
│   └── ISSUE_TEMPLATE/
│       └── review-comment.md      # Review issue template
├── document.tex                    # Main LaTeX document
└── README.md                       # This file
```

## Troubleshooting

### Workflow fails with "File not found" error
- Check the error logs to identify the missing `.sty` or `.cls` file
- Add the corresponding TeXLive package to the workflow (see "Adding LaTeX Packages" above)

### PDF has incorrect references or table of contents
- The workflow runs `pdflatex` twice, which should resolve most references
- If you're using bibliographies with BibTeX, you'll need to modify the workflow to add `bibtex` compilation between the two `pdflatex` runs
- For complex documents, you may need to add additional compilation passes

### Workflow doesn't trigger
- Make sure you're pushing changes to `.tex` files
- Check that you're pushing to the `main` branch or creating a pull request
- Verify the workflow file is in `.github/workflows/` and has the correct syntax

## Contributing

1. Fork the repository
2. Make your changes in a new branch
3. Submit a pull request
4. Wait for the automated compilation to complete
5. Download and review the compiled PDF from the PR's workflow run