# LangGraph Research Assistant

A Python-based AI research assistant that uses **LangGraph** to coordinate topic refinement, question generation, DuckDuckGo search, findings analysis, iterative decision-making, and report generation. This repository also includes a **Streamlit interface** for interacting with the project.

## Streamlit Interface

![Streamlit Research Assistant interface](screenshots/streamlit-app.png)

> Screenshot: add a real screenshot of the running Streamlit application to `screenshots/streamlit-app.png` to display it here on GitHub.

## Features

- Refines a research topic using an LLM.
- Generates focused research questions.
- Retrieves results through DuckDuckGo search (`ddgs`).
- Extracts key findings from search snippets.
- Uses a LangGraph conditional edge to repeat research or generate a report.
- Produces a short report with a summary, findings, conclusion, and basic metadata.
- Demonstrates structured graph state with a `TypedDict`.

## Workflow

```text
START
  |
  v
Input Processor
  |
  v
Question Generator <----------+
  |                          |
  v                          |
DuckDuckGo Search             |
  |                          |
  v                          |
Analyzer                      |
  |                          |
  v                          |
Conditional Router -- Retry -+
  |
  | Finish
  v
Report Generator
  |
  v
END
```

**Note:** The code's `quality_score` is a simple count-based heuristic, not a measure of source reliability or factual accuracy. The graph stores findings in its invocation state; it does not implement persistent memory across sessions.

## Tech Stack

- Python
- LangGraph
- LangChain OpenAI integration (`langchain-openai`)
- DuckDuckGo Search (`ddgs`)
- Streamlit
- python-dotenv (optional, for local `.env` loading)

## Installation (Windows / VS Code)

```powershell
# Clone the repository
 git clone https://github.com/MernaMansour/LangGraph_RA.git
 cd LangGraph_RA

# Create and activate a Python 3.12 virtual environment
py -3.12 -m venv venv
.\venv\Scripts\Activate.ps1

# Install required dependencies
python -m pip install -r requirements.txt
```

If PowerShell blocks virtual environment activation, run `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` and try again.

## API Configuration

Set environment variables for your **own supported OpenAI-compatible provider**. For local development, create a `.env` file (excluded from Git):

```dotenv
OPENAI_API_KEY=YOUR_API_KEY
OPENAI_API_BASE=YOUR_PROVIDER_BASE_URL
```

The current research assistant script uses a provider-specific model ID (`gpt-5.6-luna`). Change the model setting to an ID available from your provider. The KodeKloud lab proxy and its model aliases are not guaranteed to work on a local machine.

> If your Python script expects environment variables, ensure it loads `.env` using `from dotenv import load_dotenv` and `load_dotenv()`, or configure the variables in PowerShell before starting the app. Never commit real API keys.

## Run the Project

Launch the Streamlit application:

```powershell
python -m streamlit run streamlit_app.py
```

Open `http://localhost:8501` in your browser.

To run the command-line research workflow, execute the Python file containing your LangGraph workflow (adjust the filename to match the file in this repository):

```powershell
python research_assistant.py
```

Stop the Streamlit server by pressing **Ctrl + C** in the terminal.

## Project Structure

```text
LangGraph_RA/
├── streamlit_app.py
├── research_assistant.py       # Rename to your actual workflow filename if different
├── requirements.txt
├── README.md
├── .gitignore
├── .env.example
└── screenshots/
    └── streamlit-app.png
```

## Limitations & Future Improvements

- Search snippets are not independently fact-checked.
- Add source URLs and inline citations to generated reports.
- Replace count-based quality scoring with source and evidence evaluation.
- Add robust handling for rate limits, empty search results, and API failures.
- Add persistent checkpointing if cross-session memory is required.

## Author

Merna Mansour
