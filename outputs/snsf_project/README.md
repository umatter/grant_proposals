# SNSF Project Funding - Research Plan

## Overview

This folder contains the LaTeX template and supporting files for an SNSF Project Funding proposal.

## Files

| File | Description |
|------|-------------|
| `main.tex` | Main LaTeX document (research plan) |
| `ref.bib` | Bibliography file (BibTeX format) |
| `fig/` | Folder for figures (PDF, PNG, JPG) |

## Requirements

### SNSF Format Requirements

- **Page limit**: 15 pages / 60,000 characters (17 pages / 68,000 for collaborative)
- **Font**: Minimum 10pt (template uses 11pt Times)
- **Spacing**: Minimum 1.5 line spacing
- **Includes**: Title, summary, footnotes, figures, tables, TOC
- **Excludes**: Bibliography (not counted)

### LaTeX Requirements

To compile this document, you need:
- A LaTeX distribution (TeX Live, MiKTeX, or MacTeX)
- BibTeX for bibliography
- Standard packages (included in most distributions)

## Compilation

### Command Line

```bash
# Compile with bibliography
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex

# Or use latexmk for automatic compilation
latexmk -pdf main.tex
```

### VS Code

1. Install the LaTeX Workshop extension
2. Open `main.tex`
3. Use Ctrl+Alt+B (or Cmd+Alt+B on Mac) to build

### Overleaf

1. Create a new project
2. Upload all files from this folder
3. Set `main.tex` as the main document
4. Compile

## Output File Naming

The final PDF should be named:
```
SciencePart_[LastName].pdf
```

Example: `SciencePart_Matter.pdf`

## Checklist Before Submission

### Content
- [ ] All 5 sections complete (Summary, 2.1-2.5)
- [ ] Research questions clearly stated
- [ ] Methods justified against alternatives
- [ ] Risks identified with mitigation
- [ ] Timeline with milestones included
- [ ] Budget aligned with activities

### Format
- [ ] Within page/character limits
- [ ] PDF not write-protected
- [ ] Correct filename
- [ ] All figures render correctly
- [ ] Bibliography complete
- [ ] No broken references

### SNSF Requirements
- [ ] Language: English (for economics, STEM)
- [ ] CV achievements: Max 4,350 characters
- [ ] Budget limits respected
- [ ] All supporting documents ready

## Tips

### Character Counting

To count characters (with spaces) in your LaTeX document:
```bash
# Extract text and count
pdftotext main.pdf - | wc -m
```

Or use the `texcount` utility:
```bash
texcount main.tex
```

### Figure Preparation

- Use PDF format for vector graphics (diagrams, charts)
- Use PNG for screenshots (300 dpi minimum)
- Keep figures in the `fig/` subfolder
- Include schedule/Gantt chart as a figure

### Bibliography

- Use DOIs where available
- Include recent works (last 5 years)
- Balance seminal and current references
- Check for consistency in citation format

## Support

For questions about SNSF requirements, see:
- [SNSF Project Funding](https://www.snf.ch/en/funding/projects)
- [Research Plan Requirements](https://snf.ch/en/257qx6wxjo72ODHy/page/funding/documents-downloads/regulations-requirements-for-the-research-plan)
