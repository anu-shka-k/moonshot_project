# Moonshot Project — Blog to Temporal JSON Converter for Medical Case Blogs

---

## Overview

This repository contains tools to convert medical case-study blog posts (currently focused on cardiac arrest cases) into a standardized **temporal JSON** format. The goal is to make narrative patient case data machine-readable so it can be used for analysis, precedent retrieval, and LLM-based downstream tasks.

The main script (`create_temporal_json.py`) scrapes blog content, extracts publication metadata and images, sends the text to a Groq/Llama model for structured extraction, and saves the resulting temporal JSON files to a local output directory.

---

## Features

* Extracts main content, published date, and images from blog URLs.
* Calls the Groq API (Llama-3.3 70B in the current setup) to parse and return structured timelines.
* Produces a temporal JSON per case with fields such as `time`, `activity`, `symptom`, `diagnosis`, `treatment`, and nested `notes`.
* Handles multiple case studies in a single blog post and sorts events chronologically.

---

## Requirements

* Python 3.8+
* `pip` for installing dependencies

Suggested Python packages (install with `pip`):

```bash
pip install requests beautifulsoup4 groq
```

---

## Configuration

1. **GROQ API Key**: The script expects a Groq API key in the environment variable `GROQ_API_KEY`. Set it in your shell before running:

```bash
export GROQ_API_KEY="your_groq_api_key_here"
```

On Windows PowerShell:

```powershell
$env:GROQ_API_KEY = "your_groq_api_key_here"
```

2. **Input links CSV**: By default the script reads `src\data\links\test.csv`. Ensure this CSV exists and has a `URL` column with blog links.

3. **Output directory**: Default output path is `src\data\Jsons\temporal`. The script will write a JSON file per processed blog.

---

## Usage

Run the main script from the repository root:

```bash
python create_temporal_json.py
```

Example workflow:

1. Populate `src/data/links/test.csv` with blog URLs (header `URL`).
2. Ensure `GROQ_API_KEY` is set.
3. Run the script. JSON files will be written to the output directory and printed to the console.

---

## Output Format

Each output file is a pretty-printed JSON object with keys similar to:

```json
{
  "metadata": { "url": "...", "datetime": "..." },
  "timeline": [ /* array of ordered events */ ],
  "imageurl": [ /* list of image URLs */ ]
}
```

Each timeline entry contains at least:

* `ordinal` (event number)
* `time` (explicit time description — must not be empty)
* `activity`, `symptom`, `diagnosis`, `treatment`, `notes`

---

## Notes & Recommendations

* The quality of extracted timelines depends on the blog text and the LLM output. Curating, prompt engineering and validating a subset of outputs manually is recommended.

---

## Future Work

* Expand beyond cardiac arrest to other clinical conditions.
* Add unit tests and schema validation for output JSONs.
* Integrate into a retrieval system (RAG) or graph DB for precedent search and analytics.
* Create a small web UI for batch uploads and result inspection.

---

## Contributing

Contributions are welcome. Please open an issue or submit a pull request with clear descriptions of the change.

---

## License & Contact

Specify your license here (e.g., MIT) and provide a contact email or GitHub handle for questions.

---
