

🌍 Global-MMLU-Lite Sample Notebook

### A practical multilingual benchmark workflow for evaluating Large Language Models with Google Gemini

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Google Colab](https://img.shields.io/badge/Run%20in-Google%20Colab-F9AB00?logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![Pandas](https://img.shields.io/badge/Data-Pandas-150458?logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Gemini API](https://img.shields.io/badge/LLM-Gemini%20API-8E75B2)](https://ai.google.dev/)
[![License](https://img.shields.io/badge/License-Check%20Dataset%20Terms-lightgrey)](#license-and-data-usage)

**A reproducible sample workflow for testing multilingual question answering, measuring accuracy, and exporting benchmark results.**

</div>

---

## 📌 Project Overview

**Global-MMLU-Lite** is described as a human-verified benchmark covering **23 languages**, with **400 samples per language**. The samples are divided between culturally sensitive and general-knowledge questions.

This repository provides a Google Colab notebook that demonstrates how to evaluate a Gemini model using development and test Parquet files. It is designed as a compact starting point for exploring model performance across languages and question categories.

> **Important:** This repository contains a sample evaluation notebook. It does not include the benchmark dataset files. You must obtain the authorized Parquet files separately and upload them before running the evaluation.

## ✨ Key Features

- 🌐 **Multilingual workflow** — run the evaluation for any language for which you have both required dataset files.
- 🧠 **Gemini integration** — send multiple-choice questions to a Gemini model through the Google AI API.
- 🧪 **Few-shot prompting** — use examples from the development split to guide the model.
- 🎯 **Answer extraction** — parse the model response into an A/B/C/D choice.
- 📊 **Accuracy measurement** — calculate overall accuracy and category-specific accuracy using the labels in the dataset.
- 📁 **Excel export** — save detailed predictions and a summary to an `.xlsx` workbook.
- 🔐 **API-key handling** — read the Gemini API key from Google Colab Secrets rather than hard-coding it in the notebook.

## 🗂️ Repository Structure

```text
Global-MMLU-Lite-Sample-Notebook/
│
├── MMLU_Benchmark.ipynb   # Main Google Colab evaluation notebook
└── README.md              # Project documentation
```

## ⚙️ Requirements

The notebook installs the following Python packages:

- `pandas`
- `pyarrow`
- `requests`
- `openpyxl`

It also uses Google Colab utilities for file uploads and secret management.

## 🚀 Getting Started

### 1. Open the notebook

Open [`MMLU_Benchmark.ipynb`](./MMLU_Benchmark.ipynb) in GitHub and select **Open in Colab** if available. You can also download the notebook and upload it to [Google Colab](https://colab.research.google.com/).

### 2. Configure your Gemini API key

1. Create or access a Gemini API key through [Google AI Studio](https://aistudio.google.com/apikey).
2. In Google Colab, open the **Secrets** panel (key icon in the left sidebar).
3. Add a secret named:

   ```text
   GOOGLE_AI_API_KEY
   ```

4. Paste your API key as the secret value.
5. Enable notebook access for this secret.

**Security:** Never write your API key directly into a code cell, commit it to GitHub, or share it publicly.

### 3. Prepare the dataset files

For English, the notebook expects these two files:

```text
Global-MMLU-Lite-en-dev.parquet
Global-MMLU-Lite-en-test.parquet
```

For another language, replace `en` with that language's dataset code. For example:

```text
Global-MMLU-Lite-es-dev.parquet
Global-MMLU-Lite-es-test.parquet
```

Upload both matching files when the notebook asks you to select files.

Each Parquet file must contain these columns:

| Column | Purpose |
|---|---|
| `subject` | Subject or topic of the question |
| `question` | Multiple-choice question |
| `option_a` | Option A |
| `option_b` | Option B |
| `option_c` | Option C |
| `option_d` | Option D |
| `answer` | Correct answer |
| `cultural_sensitivity_label` | Optional category label used for category-level reporting |

### 4. Run the notebook

Run the notebook cells from top to bottom:

1. Install and import libraries.
2. Load the Gemini API key from Colab Secrets.
3. Upload the development and test Parquet files.
4. Load and validate the dataset.
5. Connect to the Gemini API.
6. Prepare prompts and extract model answers.
7. Run the English benchmark or the optional multilingual benchmark.
8. Calculate accuracy.
9. Export the results to Excel.

## 🧪 Evaluation Configuration

The notebook uses the following initial settings:

```python
MODEL_NAME = "gemini-2.5-flash"
LANGUAGE = "en"
SAMPLE_SIZE = 10
```

- `MODEL_NAME` selects the Gemini model used for evaluation.
- `LANGUAGE` selects the language dataset files.
- `SAMPLE_SIZE` controls how many test examples are evaluated in the initial run.

Start with a small sample to confirm that your API key and dataset files work. Increase the sample size only after the first run succeeds. API usage may be subject to quotas or charges.

## 📈 Evaluation Metrics

The notebook calculates:

- **Overall accuracy** — proportion of evaluated questions answered correctly.
- **CS accuracy** — accuracy for records whose category label is `CS`.
- **CA accuracy** — accuracy for records whose category label is `CA`.

The category labels are read directly from `cultural_sensitivity_label`. Refer to the dataset documentation for the authoritative meaning of each label.

## 📦 Output

After a successful run, the notebook creates:

```text
MMLU_Benchmark_Results.xlsx
```

The workbook contains two sheets:

| Sheet | Contents |
|---|---|
| `Detailed Results` | Question, model response, predicted answer, correct answer, correctness, and category label |
| `Summary` | Language, evaluated sample count, overall accuracy, CS accuracy, and CA accuracy |

The notebook then prompts you to download the Excel file.

## 🌏 Optional Multilingual Evaluation

The notebook includes a function for evaluating multiple languages:

```python
multilingual_summary, multilingual_results = run_multilingual_benchmark(
    languages=("en", "es", "fr"),
    sample_size=5
)

display(multilingual_summary)
```

Only include languages for which you have uploaded **both** development and test Parquet files. The example above is illustrative; available language codes depend on the dataset files you have.

## ⚠️ Notes and Limitations

- Dataset files are not included in this repository; obtain them separately and follow their access and usage terms.
- The notebook no longer relies on Kaggle or KaggleHub for dataset downloads.
- The multilingual function skips languages whose required files are missing.
- The current notebook starts with a small sample size; results from a small sample should not be treated as a definitive benchmark score.
- Model outputs can be inconsistent or difficult to parse. Review detailed predictions when interpreting results.
- API quotas, latency, and costs depend on your Google AI account and selected model.
- For reproducible comparisons, record the model name, dataset version, language, sample size, and run date.

## 🔒 Responsible Use

Use the benchmark data only under its applicable license and access terms. Protect API credentials, avoid publishing private dataset content, and report evaluation settings transparently.

## 🤝 Contributions

Suggestions, bug reports, and improvements are welcome. Before submitting a change:

1. Keep the notebook executable from top to bottom.
2. Do not commit API keys or private dataset files.
3. Document any changes to evaluation logic or metrics.
4. Include enough information for another researcher to reproduce the experiment.

## 📄 License and Data Usage

No license information is established by this README. Before redistributing this notebook or any benchmark data, check the repository owner’s intended software license and the benchmark dataset’s own license and usage terms.

---

**Built for transparent and reproducible multilingual LLM evaluation.**
