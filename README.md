# Explainer-Explainer

Generate plausible causal factors for a binary outcome using a simple, interpretable approach: fit one logistic regression per feature, normalize to log-relative-odds, and aggregate contributions in a naive Bayes-style model. The included Jupyter notebook walks through the method, shows visuals, and provides a reusable class you can apply to your own data.

## What’s in this repo
- `explainer.ipynb` — the main notebook with narrative, code, visuals, and a reusable `NaiveLogOddsExplainer` class.
- `requirements.txt` — Python dependencies for running the notebook.
- `LICENSE` — License for the project.

## Method at a glance
- Fit `LogisticRegression(y ~ x_j)` for each feature `x_j` separately.
- Interpret the coefficient as a log-relative-odds contribution (after standardization for numeric features).
- Sum per-feature contributions to obtain an overall log-odds score, then convert to probability using the observed base rate.
- Visualize global effect sizes and per-sample top positive/negative contributors.

Assumptions and notes:
- Outcome `y` is binary (0/1).
- Features should be numeric (one-hot encode categoricals first).
- Conditional independence between features given `y` is assumed (naive Bayes). This is for explanation and exploration, not causal proof.

## Quick start
These steps assume macOS with zsh and Python 3.10+ installed.

### 1) Create and activate a virtual environment
```zsh
# From the repository root
python3 -m venv .venv
source .venv/bin/activate

# (Optional) upgrade pip
python -m pip install --upgrade pip
```

### 2) Install dependencies
```zsh
pip install -r requirements.txt
```

### 3a) Run the notebook in VS Code (recommended)
1. Open the folder in VS Code: `File > Open Folder...` and select this repo.
2. Install the VS Code extensions: `Python` and `Jupyter` (if prompted).
3. Select the interpreter: press `Cmd+Shift+P`, run `Python: Select Interpreter`, and choose `.venv`.
4. Open `explainer.ipynb` in VS Code.
5. Click `Run All` or execute cells step-by-step.

If VS Code asks to install the `ipykernel`, accept the prompt or run:
```zsh
python -m ipykernel install --user --name explainer-explainer --display-name "Python (explainer-explainer)"
```

### 3b) Run the notebook in your browser (Jupyter Lab/Notebook)
```zsh
# Jupyter Lab (preferred)
jupyter lab
# OR classic Notebook
jupyter notebook
```
Then open `explainer.ipynb` from the Jupyter file browser.

## Using the explainer on your data
In the notebook, see the "Use on your data" cell for a minimal template. In short:
- Prepare `X_user` as a `pandas.DataFrame` of numeric features (one-hot encode categoricals).
- Prepare `y_user` as a 0/1 array-like target.
- Fit the explainer and get probabilities: `explainer = NaiveLogOddsExplainer().fit(X_user, y_user)` then `p_user = explainer.predict_proba(X_user)`.
- Inspect contributors for a row: `explainer.explain_row(X_user, idx, top_k=3)`.

## Troubleshooting
- Kernel mismatch in VS Code: ensure the selected interpreter points to `.venv`.
- Missing packages: run `pip install -r requirements.txt` inside the activated `.venv`.
- Plot rendering issues: ensure you ran the setup/import cell first in the notebook.

## License
This project is licensed under the terms of the LICENSE file in this repository.
