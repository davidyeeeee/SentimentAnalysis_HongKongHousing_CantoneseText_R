LIHKG Forum Sentiment Analysis (R + GPT API)

This project performs sentiment analysis on Hong Kong LIHKG forum thread titles using a large language model (GPT-4o-mini) via an OpenAI-compatible API. It is designed to be flexible, supporting any API endpoint that follows the OpenAI Chat Completion format.

Why GPT instead of traditional R sentiment packages?

Traditional R sentiment analysis packages (e.g., syuzhet, sentimentr, tidytext + Bing/NRC dictionaries) rely on predefined word lists and simple rules. They struggle with:

- Cantonese and mixed language: LIHKG users frequently write in Cantonese vernacular, mixing English, emojis, and internet slang. Rule-based methods fail to recognize even basic expressions like "好正" or "FF".

- Sarcasm and irony: Hong Kong online discourse is often highly ironic (e.g., "真係多謝政府" meaning the opposite). Lexicon-based approaches cannot detect tone reversal.

- Context-dependent polarity: The same word can be positive or negative depending on the topic (e.g., "跌" in "樓價跌" is negative for homeowners but positive for buyers). GPT captures such nuance by understanding the full sentence.

- Out-of-vocabulary terms: New slang appears constantly. GPT's pre-trained knowledge generalizes well without needing an updated dictionary.

In short, GPT provides context-aware, multi-lingual, and irony-sensitive sentiment analysis that closely matches human judgment – essential for credible computational social science research on Cantonese forums.

---

Features

- Batch sentiment scoring of text data (CSV input/output)
- Supports any OpenAI-compatible API (official, third-party proxy, local server)
- Fully configurable via environment variables – no hard-coded keys
- Cantonese-aware system prompt included (adjustable)
- Automatic rate-limiting delay and error handling
- Preserves original data, appends sentiment score column

Requirements

- R (>= 4.0)
- R packages: httr, jsonlite, readr, stringr
- An API key from an OpenAI-compatible service (e.g., OpenAI, Azure, Shubiaobiao, LocalAI)

Installation

1. Clone the repository:
   git clone https://github.com/YOUR_USERNAME/sentiment-analysis-lihkg.git
   cd sentiment-analysis-lihkg

2. Install required R packages (run in R console):
   install.packages(c("httr", "jsonlite", "readr", "stringr"))

3. Set up your API key and other configurations (see below).

---

Configuration

All settings are read from environment variables. You can set them in your R session, in an .Renviron file, or via your shell.

Variable; Description; Default

OPENAI_API_KEY; Your API key (required); (none)

OPENAI_API_BASE; API endpoint URL; https://api.openai.com/v1

OPENAI_MODEL; Model name; gpt-4o-mini

OPENAI_TEMPERATURE; Sampling temperature (0-1); 0

REQUEST_DELAY; Seconds between requests; 0.3

DATA_DIR; Directory for input/output; ./data

INPUT_FILE; Path to input CSV; {DATA_DIR}/threads.csv

OUTPUT_FILE; Path to output CSV; {DATA_DIR}/sentiment_output.csv

TEXT_COLUMN; Column name containing text; title

SYSTEM_PROMPT; Custom system prompt (Chinese by default); (see script)

Example (using an official OpenAI key):
   Sys.setenv(OPENAI_API_KEY = "sk-...")
   Sys.setenv(OPENAI_MODEL = "gpt-4o-mini")

Example (using a third-party proxy like "Shubiaobiao"):
   Sys.setenv(OPENAI_API_KEY = "your-proxy-key")
   Sys.setenv(OPENAI_API_BASE = "https://api.shubiaobiao.com/v1")
   Sys.setenv(OPENAI_MODEL = "gpt-4o-mini")

---

Usage

1. Place your input CSV file (with a text column, e.g., "title") inside the data/ folder (or adjust INPUT_FILE accordingly).

2. Run the script from the terminal:
   Rscript sentiment_analysis.R
   Or source it in R:
   source("sentiment_analysis.R")

3. The output CSV will be saved with an additional column "sentiment_score" (range -1 to 1, where -1 = very negative, 0 = neutral, 1 = very positive).

Output Example

title                                            sentiment_score
樓價跌到阿媽都唔認得                                -0.85
政府撤辣好消息                                     0.72
乜嘢係納米樓？                                     0.05

---

Notes

- The system prompt is written in Chinese to better capture Cantonese expressions and irony. You can override it with SYSTEM_PROMPT if you prefer English.

- The script adds a 0.3-second delay between API calls to avoid rate limiting. Adjust REQUEST_DELAY as needed.

- If you process large batches, consider using a local model (via Ollama or LocalAI) to reduce costs.

---

AI Use Declaration

This project was developed with the assistance of an AI language model (OpenAI's GPT-4o / ChatGPT). The AI was used for the following purposes:

- Generating initial code snippets and function structures
- Debugging and suggesting improvements
- Drafting the project README and configuration instructions

All AI-generated code and text have been reviewed, tested, and modified as necessary by the human author to ensure correctness, safety, and compliance with the project's academic goals. The final work remains the original intellectual contribution of the author, with the AI acting as a coding assistant and writing aid.

License

MIT
