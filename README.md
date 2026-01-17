# The-Digital-Stoic-Library

This repository features a comprehensive pipeline for creating a digital philological edition of Epictetus and Marcus Aurelius. It includes Python scripts for API data acquisition, Gemini 3 Pro-powered translation, and CLTK-based lexical tagging. The final output is an interactive, web-based library with dynamic lemma-highlighting readers.

## I. METHODOLOGY OVERVIEW

The goal of this project is to create a technically rigorous, digital-first edition of the core Stoic corpus. By integrating Large Language Models (LLMs) with computational linguistics, we move beyond static translations into an interactive database where English text is live-mapped to Greek technical lemmas.

### Key Methodological Pillars:

* **Structured Acquisition**: Extracting standardized `perseus-grc2` editions via the Scaife Viewer CTS API to ensure scholarly consistency.
* **Context-Aware LLM Translation**: Utilizing **Gemini 3 Pro** with a sliding context window (150 characters of leading/trailing Greek) to maintain narrative and thematic continuity.
* **Philological Lemmatization**: Using the **Classical Language Toolkit (CLTK)** to identify the root dictionary form (lemma) of technical terms, ensuring that all inflected forms of a word are captured in thematic analysis.
* **Automated Sanitization**: Implementing a regex-based cleaning layer to eliminate structural artifacts and "token leaks" common in high-reasoning LLM outputs.

---

## II. SCRIPT MANIFEST

To recapitulate the project, the following scripts must be executed in the order listed below.

### Phase 1: Data Acquisition

* **`fetch_enchiridion.py`**: Requests the Greek text of the *Enchiridion* from the Scaife API and saves it as `enchiridion_greek_source.json`.
* **`fetch_meditations.py`**: Requests the Greek text of the *Meditations* and saves it as `meditations_greek_source.json`.
* **`fetch_discourses.py`**: Requests the Greek text of the *Discourses* from the Scaife API and saves it as `discourses_greek_source.json`.

### Phase 2: Translation and Commentary

* **`translate_enchiridion.py`**: Generates an instructional, imperative translation and Stoic commentary.
* **`translate_meditations.py`**: Generates a reflective, personal translation and commentary using high-level reasoning (`ThinkingConfig`).
* **`translate_discourses.py`**: Generates a conversational, dialectic translation and pedagogical commentary.

### Phase 3: Lexical Tagging

* **`enchiridion_lexical_tagging.py`**: Applies the Master Stoic Lexicon to the *Enchiridion* using CLTK lemmatization.
* **`meditations_lexical_tagging.py`**: Applies the Master Stoic Lexicon to the *Meditations*, with specific logic to handle word collisions (e.g., *horme* vs. *aphorme*).
* **`discourses_lexical_tagging.py`**: Applies the Master Stoic Lexicon to the *Discourses*, optimized for the larger volume of text and frequent cross-references.

### Phase 4: Data Cleaning

* **`clean_enchiridion.py`**: Strips Markdown artifacts and repairs JSON leaks in the *Enchiridion* database.
* **`clean_meditations.py`**: Cleans the *Meditations* database and standardizes chapter reference formatting (e.g., "Book 1.1").
* **`clean_discourses.py`**: Cleans the *Discourses* database and standardizes the multi-book reference structure (e.g., "1.1", "4.1").

---

## III. DIRECTIONS FOR RUNNING LOCALLY

### 1. Installation

Install the required Python environment:

```bash
pip install requests google-genai cltk

```

*Note: CLTK will download Ancient Greek models on the first run of the tagging scripts.*

### 2. API Configuration

You must provide a Google API Key to run the translation scripts:

```bash
export GOOGLE_API_KEY='your_api_key_here'

```

### 3. Execution Sequence

Run the scripts in this specific order to ensure data integrity:

1. Run all `fetch_*.py` scripts.
2. Run all `translate_*.py` scripts.
3. Run all `*_lexical_tagging.py` scripts.
4. Run all `clean_*.py` scripts.

### 4. Viewing the Interactive Library

The project results are viewed through a web browser using the dedicated HTML readers. Because the HTML files use the `fetch()` command to load JSON data, you must host them on a local server.

**Steps:**

1. Open a terminal in the project directory.
2. Start the Python server: `python3 -m http.server 9000`
3. Open your browser to: `http://localhost:9000`
4. Select `index.html`, `Meditations_Reader.html`, `Enchiridion_Reader.html`, or `Discourses_Reader.html` to begin.

---
