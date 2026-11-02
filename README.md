# Breaking Bad Transcript Analysis

A data analysis pipeline that scrapes *Breaking Bad* episode transcripts, structures them into character-level dialogue, and uses NLP, statistical correlation, and network analysis to study sentiment, character importance, and how characters talk about one another.

## Project description

This project collects full episode transcripts from [Forever Dreaming](https://transcripts.foreverdreaming.org/viewforum.php?f=165) with Python (`requests` + BeautifulSoup), then turns unstructured script text into a table of episode, character, and line. Pandas is used to query, clean, and filter that table (character aliases such as Walt/Walter, episode numbering, main-cast subsets). TextBlob sentiment scores each line; line counts per episode are correlated to show which characters rise and fall together; a NetworkX graph links characters when they mention each other in dialogue; word clouds summarize vocabulary for the show and for each top speaker.

## What it analyzes

- **Sentiment by character** — average TextBlob polarity for the main cast (Walter, Jesse, Skyler, Hank, Marie, Walter Jr, plus scene direction).
- **Sentiment over time** — mean polarity by episode.
- **Character importance correlation** — heatmap of how often top characters speak in the same episodes (Pearson correlation of line counts).
- **Mention network** — undirected graph of co-mentions among the most frequent speakers.
- **Word clouds** — frequent words across all dialogue and per top character, with English stopwords plus filler words (`yeah`, `right`, `okay`) removed.

## Pipeline

1. **Scrape** the forum index, follow each episode link, and extract transcript HTML.
2. **Parse** speaker labels (`Name: line`) into a pandas DataFrame.
3. **Normalize** names and map titles like `1x01` to episode numbers.
4. **Score** each line’s sentiment polarity.
5. **Aggregate and visualize** sentiment, correlations, a mention graph, and word clouds.

## Setup

```bash
pip install -r requirements.txt
```

NLTK tokenizers and stopwords download on first run (`punkt`, `stopwords`).

## Run

```bash
python breaking_bad_analysis.py
```

The script fetches live pages, so you need network access. Matplotlib windows will open for each plot.

## Notes

- Episode numbering in the script covers seasons 1–3 (7 episodes each). Later-season titles that do not match `SxEE` in that map get episode number `-1`.
- “Scene” is treated as a speaker when it appears as a label in the transcript.
- Scraping another site’s transcripts may be subject to that site’s terms of use; this project is for personal analysis and learning.
