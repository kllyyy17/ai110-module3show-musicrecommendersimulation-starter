# 🎵 Music Recommender Simulation

## Project Summary

In this project you will build and explain a small music recommender system.

Your goal is to:

- Represent songs and a user "taste profile" as data
- Design a scoring rule that turns that data into recommendations
- Evaluate what your system gets right and wrong
- Reflect on how this mirrors real world AI recommenders

Replace this paragraph with your own summary of what your version does.

---

## How The System Works

Real recommenders (Spotify, etc.) learn from millions of users' behavior — plays, skips, saves —
to predict what you'll like. My version is smaller and **content-based**: it compares each song's
measurable attributes to a user's stated taste, with no history or other users. It prioritizes
*closeness of fit* (values near the user's target, not just high) plus a bonus for matching genre
and mood.

- **`Song` features:** `genre`, `mood`, `energy`, `valence`, `danceability`, `acousticness`,
  `tempo_bpm` (normalized 0–1), plus `title`/`artist` for display.
- **`UserProfile` features:** `preferred_genre` and `preferred_mood`, target values for the five
  numeric features, and per-feature **weights**.

### Algorithm Recipe

Each song accumulates points (higher = better match):

- **Genre match:** +2.0 · **Mood match:** +1.0 (mood is half of genre since it overlaps energy/valence)
- **Numeric similarity** rewards closeness: `weight × (1 − |target − value|)`, with weights
  energy 2.0, acousticness 1.5, valence/danceability/tempo 1.0 each.

```
score = 2.0·[genre match] + 1.0·[mood match]
      + 2.0·(1−|Δenergy|) + 1.5·(1−|Δacousticness|)
      + 1.0·(1−|Δvalence|) + 1.0·(1−|Δdanceability|) + 1.0·(1−|Δtempo|)
```

**Ranking:** score every song → sort descending → return top *k* (k=5) with a short explanation.

### Potential biases I expect

- **Genre over-prioritization** — genre (+2.0) is the heaviest term, so a great cross-genre match
  on mood/energy (e.g., ambient/jazz for a lofi fan) can get buried.
- **No diversity** — with no history it may return five near-identical songs.
- **Redundant mood signal** — mood restates energy + valence, so "vibe" is partly double-counted.
- **Tiny hand-labeled catalog** — scores are only meaningful across 20 songs whose values I assigned,
  so any labeling bias propagates directly.

---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows

2. Install dependencies

```bash
pip install -r requirements.txt
```

3. Run the app:

```bash
python -m src.main
```

### Running Tests

Run the starter tests with:

```bash
pytest
```

You can add more tests in `tests/test_recommender.py`.

---

## Sample Recommendation Output

Paste a sample of your recommender's output here as a text block so a reader can see what it produces:

```
# e.g.:
# User profile: genre=indie, mood=chill, energy=low
# Recommendations:
#   1. ...
#   2. ...
#   3. ...
```

**Screenshot or video** *(optional)*: <!-- Insert a screenshot or demo video link here -->

---

## Experiments You Tried

Use this section to document the experiments you ran. For example:

- What happened when you changed the weight on genre from 2.0 to 0.5
- What happened when you added tempo or valence to the score
- How did your system behave for different types of users

---

## Limitations and Risks

Summarize some limitations of your recommender.

Examples:

- It only works on a tiny catalog
- It does not understand lyrics or language
- It might over favor one genre or mood

You will go deeper on this in your model card.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this



