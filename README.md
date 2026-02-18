# One-Way ANOVA Tool (with Tukey HSD + Boxplots)

A lightweight, dependency-free **One-Way ANOVA** web app you can host on **GitHub Pages** and embed in **Canvas** via an `<iframe>`.

## Features

- **One-way ANOVA** (k groups, unequal sample sizes supported)
- Full **ANOVA table**: SS, df, MS, F, p-value
- **Effect sizes**: η² and ω²
- **Group descriptives**: n, mean, variance, SD
- **Boxplots** (per group)
  - 1.5·IQR whiskers
  - outliers plotted as points
- Post-hoc comparisons:
  - **Tukey HSD / Tukey–Kramer** (recommended after ANOVA)
  - Optional **pooled t-tests** with Holm/Bonferroni/none

## Files

- `index.html` — the full app (single-file; includes all CSS + JS)

## Input Formats

The app supports two paste-in formats:

### 1) Column mode (each column is a group)
- Rows are observations
- Commas, tabs, or spaces are accepted
- A header row is optional (recommended)

Example:

```csv
GroupA,GroupB,GroupC
12,10,9
11,12,8
13,9,10
10,11,9
```
### 2) Label mode (two columns: Group, Value)

Example:
Group,Value
A,12
A,11
B,10
B,12
C,9
Empty cells are ignored. Non-numeric values in the data region are ignored.
## How Tukey HSD Is Computed (Important)

This tool implements **Tukey HSD / Tukey–Kramer** using:

- **Pooled within-group mean square:** `MSW`
- **Error degrees of freedom:** `dfW`

### Tukey–Kramer standard error (unequal n)

SE<sub>ij</sub> = √[ (MSW / 2) ( 1/n<sub>i</sub> + 1/n<sub>j</sub> ) ]

### Test statistic

q<sub>ij</sub> = | x̄<sub>i</sub> − x̄<sub>j</sub> | / SE<sub>ij</sub>

Because the studentized range distribution Q(k, df) is not built into vanilla JavaScript, the app estimates:

- Tukey critical value q<sub>α</sub>
- Tukey-adjusted p-values

…via a seeded Monte Carlo simulation of Q(k, df).

You can adjust:

- **Sim reps** (more reps → smoother p-values / more stable q<sub>α</sub>)
- **Seed** (reproducibility)

Recommended default:

- **60,000 reps** for general classroom use


## Running Locally

You can open `index.html` directly in a browser.

Optionally, serve it locally (recommended for testing) with Python:

```
python -m http.server 8000
```
Then open:
```
http://localhost:8000
```
## Deploying on GitHub Pages

1. Create a GitHub repository (e.g., `anova-tool`).
2. Add `index.html` at the **repository root**.
3. Navigate to **Settings → Pages**.
4. Under **Build and deployment**:
   - **Source:** Deploy from a branch  
   - **Branch:** `main` (or `master`)  
   - **Folder:** `/(root)`
5. Click **Save**.

Your site will be published at:

https://YOUR-USERNAME.github.io/YOUR-REPO/

---

## Embedding in Canvas

Use an `<iframe>` inside a Canvas page:

```html
<iframe
  src="https://YOUR-USERNAME.github.io/YOUR-REPO/"
  width="100%"
  height="900"
  style="border:0;border-radius:12px;overflow:hidden;"
  loading="lazy"
></iframe>
```
## Canvas Embedding Notes

If Canvas strips `<iframe>` elements from a normal Page, try:

- Adding the tool as an **External Tool (LTI)**
- Embedding it in a location where Canvas allows raw HTML  
  (availability depends on your institution’s Canvas settings)

---

## Assumptions & Notes

This tool implements a **classical one-way ANOVA** workflow and assumes:

- Independent observations  
- Approximately normal residuals  
- Roughly equal group variances (homoscedasticity)

If you expect substantial heteroscedasticity (unequal variances), consider using **Welch’s ANOVA** (not included in this version).

---

## License

This project is licensed under the MIT License.

### MIT License

Copyright (c) 2026 Angie Schirck

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
