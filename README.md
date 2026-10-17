# ResearchMind 🔬

**ResearchMind is a multi-agent AI research assistant that searches the web, extracts relevant source content, generates a structured research report, and critiques the report for quality.** It includes a Streamlit interface and a command-line pipeline.

## Overview

ResearchMind uses a four-stage workflow to turn a research topic into a readable report:

1. **Search Agent** — uses Tavily to find relevant web results, including titles, URLs, and snippets.
2. **Reader Agent** — selects a relevant URL from the search results and extracts readable page content.
3. **Writer Chain** — combines the search results and extracted content into a report with an introduction, key findings, conclusion, and source URLs.
4. **Critic Chain** — reviews the generated report and returns a score out of 10, strengths, areas to improve, and a one-line verdict.

The Streamlit app presents the workflow and displays the research output, the final report, and critic feedback. The report can also be downloaded as a Markdown file.

## Features

- Topic-driven web research
- Tavily-powered search with up to five results per query
- Web-page text extraction using Requests and Beautiful Soup
- LLM-generated research reports with a consistent structure
- Separate critique stage for report quality feedback
- Streamlit UI with a dark, custom-styled interface
- Markdown report download
- Command-line entry point for running the pipeline without the UI

## Architecture

```text
Research topic
      |
      v
  Search Agent  -----> Tavily web search
      |
      v
  Reader Agent  -----> Fetch and extract a relevant web page
      |
      v
  Writer Chain  -----> Structured research report
      |
      v
  Critic Chain  -----> Score, strengths, improvements, verdict
      |
      v
  Results in Streamlit UI or terminal
```

## Tech Stack

- **Language:** Python
- **LLM orchestration:** LangChain
- **Language model:** OpenAI `gpt-4o-mini`
- **Web search:** Tavily
- **Web extraction:** Requests, Beautiful Soup, lxml
- **Interface:** Streamlit
- **Configuration:** `python-dotenv`

## Project Structure

```text
ResearchMind/
├── agents.py          # Search and reader agents; writer and critic chains
├── app.py             # Streamlit user interface
├── pipeline.py        # Command-line research pipeline
├── tools.py           # Tavily search and web-page scraping tools
├── requirements.txt   # Python dependencies
├── .gitignore         # Excludes environment secrets
└── README.md
```

## Requirements

- Python 3.10 or newer
- An OpenAI API key
- A Tavily API key
- Internet access for search and page retrieval

API usage may incur charges according to your provider plans and current pricing.

## Setup

### 1. Clone the repository

```bash
git clone <repository-url>
cd <repository-directory>
```

Replace the repository URL and directory with the repository you created for this project.

### 2. Create and activate a virtual environment

**Windows (PowerShell):**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure API keys

Create a file named `.env` in the project root with the following entries:

```env
OPENAI_API_KEY=your_openai_api_key
TAVILY_API_KEY=your_tavily_api_key
```

Replace the example values with your own keys. Keep `.env` private and never commit API keys to version control. The repository's `.gitignore` excludes `.env`.

## Run ResearchMind

### Streamlit web app

```bash
streamlit run app.py
```

Open the local address printed by Streamlit, enter a research topic, and select **Run Research Pipeline**. The app displays the generated report and critic feedback, and provides a Markdown download for the report.

### Command-line pipeline

```bash
python pipeline.py
```

Enter a research topic when prompted. The pipeline runs the search, reading, writing, and critique stages and prints their outputs in the terminal.

## Report Format

The writer is prompted to produce these sections:

- **Introduction**
- **Key Findings** — at least three explained points
- **Conclusion**
- **Sources** — URLs identified in the research material

The critic returns:

- Score out of 10
- Strengths
- Areas to improve
- One-line verdict

## Configuration

The language model is configured in `agents.py` as `gpt-4o-mini` with temperature `0`. The Tavily search tool in `tools.py` requests up to five results per search. The scraper removes common non-content elements such as scripts, styles, navigation, and footers, then limits extracted text to 3,000 characters.

## Limitations

- The quality and recency of the report depend on the search results, accessible web pages, and model output.
- Some websites block automated requests or render their content with JavaScript, so scraping may return incomplete text or fail.
- The reader stage is prompted to select a relevant URL from the search output; it does not systematically scrape every result.
- The critic provides an LLM-generated assessment, not an independent factual verification of every claim.
- Always check important findings and cited sources before relying on the report for academic, professional, medical, legal, or financial decisions.
- API access and usage are subject to the relevant OpenAI and Tavily accounts and plans.

## Security

- Store API keys in `.env` or another secure secret-management mechanism.
- Do not share `.env` or publish API keys in source files, screenshots, logs, or commits.
- Treat generated reports as research assistance; verify important claims against the original sources.

## License

No license file is currently included. Add a license before distributing or reusing the project if you want to specify the terms under which others may use it.
