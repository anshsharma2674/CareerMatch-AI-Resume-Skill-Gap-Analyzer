# CareerMatch AI — Resume & Skill Gap Analyzer

CareerMatch AI is a static, browser-based portfolio project for comparing resume text with a target job description. It identifies recognizable skills, calculates a transparent coverage score, suggests learning activities, and creates a draft summary for the user to verify. All matching runs locally in the browser; the baseline is deterministic keyword matching, not a trained machine learning model.

## Features

- Upload PDF or TXT resumes (5 MB limit), or paste text.
- PDF text extraction using pinned PDF.js 3.11.174; OCR is not included.
- Sample role descriptions and a fictional Python Developer Intern demo.
- 50+ grouped sample roles across technology, business, creative, finance, education, healthcare, engineering, service, and legal fields.
- Skill matching by a small, explainable taxonomy with common aliases.
- Skill match formula: matched unique required skills ÷ unique recognized required skills × 100.
- Separate keyword coverage, resume word count, and detected bullet count.
- Skill gap learning suggestions, a short roadmap, and a draft summary based on provided resume text.
- Download a plain text analysis report and copy the draft summary.
- Optional local history storing role, score, and date, with a clear-history control.
- Responsive layout, keyboard focus indicators, reduced-motion support, and accessible form labels.
- Persisted dark/light themes that follow the system preference on first visit.

## Screenshots

No screenshots are included yet. To add them, capture the landing page, analyzer, and completed fictional demo on desktop and mobile. Save images under `screenshots/` and link them here using repository-relative paths, for example `![Demo analysis](screenshots/demo-analysis.png)`.

## Technology stack

- HTML5, CSS3, and vanilla JavaScript (ES6+)
- PDF.js 3.11.174, loaded from cdnjs for PDF parsing
- Browser File APIs and optional `localStorage`

There is no build step, backend, API key, analytics, or server requirement for core analysis. PDF parsing requires the PDF.js CDN to be reachable. If it is unavailable, paste resume text manually.

## Project structure

```text
.
├── index.html    # Application, styles, skill taxonomy, and analysis logic
├── README.md     # Project and usage documentation
├── TESTING.md    # Manual verification cases and recorded results
├── LICENSE       # MIT license with owner/year placeholders
└── .gitignore    # Local and sensitive artifacts
```

## Run locally

Open `index.html` in a current browser. No installation is required. For reliable PDF.js worker loading, use a local static file server if your browser blocks workers on `file://`; for example, the VS Code Live Server extension. The app itself has no server-side component. You may also use GitHub Pages.

## Publish with GitHub Pages

1. Create a GitHub repository and add these project files. Replace the copyright placeholders in `LICENSE` first.
2. Push the files to the repository's default branch. Keep `index.html` at the repository root.
3. In the repository, open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**, select the default branch and `/ (root)`, then save.
5. After GitHub Pages finishes deploying, open the Pages URL shown in repository settings. The app uses relative local assets (none required) and works from a repository subpath.

The public Pages site will serve the static files. PDF parsing additionally loads PDF.js from cdnjs, so users without access to that CDN can still analyze pasted text and TXT files.

## Use the application

1. Upload a PDF/TXT or paste resume text.
2. Enter a job title and paste a job description, or select one of the sample roles (which fills a sample description).
3. Choose whether to save a short summary in browser history; it is off by default.
4. Select **Analyze match**. Review matched skills, skills absent from the resume text, additional skills, the gap suggestions, roadmap, and draft summary.
5. Download a TXT report or copy the draft summary. Verify any generated text before using it.

Use the theme control in the header to switch between light and dark appearance. On first visit, the app follows the operating system's color preference. A manual choice is saved as a browser preference; it does not save resume or job-description text.

**Example demo workflow:** choose **Try fictional demo**. A fictional Alex Morgan resume and Python Developer Intern description fill the form and are sent through the same skill detector and scoring function as user input. The score depends on the skills recognized in that supplied sample and the taxonomy in `index.html`.

## How matching works

The app normalizes case, whitespace, and Unicode form, then uses boundary-aware matching for each canonical skill and its aliases. A recognized skill is counted once, regardless of how many times it occurs. Required skills are the unique taxonomy entries found in the supplied job description. Matched skills are required skills also found in resume text.

```text
Skill match = (matched required skills / unique recognized required skills) × 100
```

For example, if a job description contains Python, SQL, and Git and resume text contains Python and Git, the score is 2 ÷ 3 × 100 = 67% (rounded to the nearest whole percent). If no taxonomy skills are found in the job description, the app asks for more detail instead of displaying a misleading 0%. A separate keyword coverage metric is the share of distinct, non-stopword job-description terms found in resume text; it is not the skill score.

Priority labels on gaps follow the order of skills detected in the job description and divide the list into high, medium, and explore groups. They are simple planning cues, not a claim about employer priorities. Missing from resume text does not mean the candidate lacks that skill.

## Architecture

The single `index.html` includes interface markup, responsive styles, the skill taxonomy, parsing, normalization, skill detection, scoring, roadmap and summary generation, reporting, and optional local history. File parsing and all text analysis take place in the page. User-controlled strings inserted into result markup are HTML-escaped; ordinary form text uses text controls.

## Privacy and security

Resume and job description text are not uploaded to a service or stored by default. PDF.js library code and its worker are fetched from cdnjs, but the resume document is processed locally. No analytics or tracking are enabled. Optional history stores only role title, score, and date in browser local storage. Local storage is not encrypted; do not enable it on a shared device when that is unsuitable. Clear saved history from the analyzer, or remove this site's local storage through browser settings. Downloaded reports are saved by the browser to the location you choose.

## Limitations

- This is keyword/taxonomy matching, not semantic NLP or a trained ML model. It can miss synonyms, infer no context, and may count words without understanding proficiency.
- The taxonomy is intentionally small and may not cover domain-specific requirements.
- PDF extraction requires selectable text and an available PDF.js CDN. Scanned PDFs need OCR elsewhere or pasted text; OCR is not implemented here.
- The generated summary uses simple rules and supplied text. It can omit context and must be checked for accuracy before use.
- No ATS compatibility, interview outcome, or hiring result is predicted.
- Browser behavior and third-party CDN availability may vary.

## Future improvements

- Expand and version the taxonomy with user-visible aliases and tests.
- Add optional semantic similarity using embeddings, with clear disclosure and consent.
- Add a real NLP pipeline for entity and context extraction.
- Add an optional secure backend integration for model-assisted feedback, with explicit consent, retention controls, and secret keys kept server-side.
- Add OCR as an explicitly selected feature, with clear privacy and processing disclosure.

## Contributing

Open an issue describing the change or submit a pull request. Keep matching explainable, avoid fabricated candidate facts, preserve local-first defaults, and document any new data processing. Run the manual cases in `TESTING.md` before submitting.

## License

MIT. Replace the clearly marked copyright holder and year placeholders in `LICENSE` before publishing.
