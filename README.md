# 🍳 Recipe Agent

An AI agent that looks at a photo of your ingredients and instantly finds you a recipe you can actually cook with what you have — no manual ingredient typing required.

Built with **LangChain**, a **Groq**-hosted LLM, and the **Tavily** search API, the agent takes an image, identifies the ingredients in it using vision, then autonomously searches the web for a matching recipe and returns it.

## How it works

1. You point the app at an image (via a file picker, or by typing a path).
2. The image is base64-encoded and sent to the LLM as part of a multimodal message.
3. A LangChain agent — running on Groq's `qwen3-32b` model — is given a system prompt instructing it to act as "the best chef in the world," identify the ingredients visible in the photo, and find a recipe that only needs those ingredients.
4. The agent calls a custom `web_search` tool (backed by the **Tavily** search API) to look up a matching recipe online.
5. The final recipe is printed to the console.

## Tech stack

| Layer | Technology |
|---|---|
| Agent framework | [LangChain](https://www.langchain.com/) (`create_agent`, tool calling, multimodal messages) |
| LLM inference | [Groq](https://groq.com/) via `langchain-groq` |
| Web search tool | [Tavily](https://tavily.com/) search API |
| Image input | Python `tkinter` file dialog + Base64 data URLs |
| Config / secrets | `python-dotenv` |
| Language | Python 3.12+ |
| Dependency management | [uv](https://github.com/astral-sh/uv) |

## Project structure

```
recipe_agent/
├── main.py             # Entry point — builds and runs the agent
├── photo_decoder.py    # Image selection + base64 encoding for the vision prompt
├── prompt.py           # System prompt and user prompt for the chef agent
├── tools.py            # Tavily-backed web_search tool exposed to the agent
├── pyproject.toml      # Project metadata and dependencies
└── .env.example        # Required environment variables
```

## Getting started

### Prerequisites

- Python 3.12+
- A [Groq API key](https://console.groq.com/keys)
- A [Tavily API key](https://tavily.com/)

### Installation

```bash
git clone https://github.com/baakasta/recipe_agent.git
cd recipe_agent

# using uv (recommended, since the repo ships a uv.lock)
uv sync

# or with pip
pip install -e .
```

### Configuration

Copy the example environment file and add your API keys:

```bash
cp .env.example .env
```

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
```

### Usage

```bash
uv run main.py
# or: python main.py
```

A file picker will open — select a photo of your ingredients (`.png`, `.jpg`, `.jpeg`, `.webp`). If no display is available, you'll be prompted to paste an image path in the terminal instead. The agent will then identify the ingredients and print a recipe suggestion to the console.

