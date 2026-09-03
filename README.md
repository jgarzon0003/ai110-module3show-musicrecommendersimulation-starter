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

Explain your design in plain language.

Some prompts to answer:

- What features does each `Song` use in your system
  - It will focus on energy and mood of the song
- What information does your `UserProfile` store
  It will store favGenre, favMood, targetEnergy
- How does your `Recommender` compute a score for each song

  **Algorithm Recipe (finalized):**

  | Component | Points | Rule |
  |---|---|---|
  | Genre match | +2.0 | `song.genre == user.favorite_genre` |
  | Mood match | +1.0 | `song.mood == user.favorite_mood` |
  | Energy similarity | up to +1.5 | `1.5 * (1 - abs(song.energy - user.target_energy))` — full credit when energy exactly matches the target, scaling down the further apart they are |
  | Acousticness bonus | +0.5 | only if `user.likes_acoustic` is True and `song.acousticness >= 0.6` |

  Max possible score is 5.0. Genre is weighted twice as heavily as mood because it's the strongest explicit signal a user gives — switching genres tends to break a listening session more than switching moods. Energy uses a continuous similarity curve instead of an all-or-nothing bonus so a song that's close to the target energy still scores well even if it's not a perfect match. Acousticness is a small, optional tie-breaker that only applies to users who opted in.

- How do you choose which songs to recommend
  - Every song in the catalog is scored against the `UserProfile` using the recipe above, then sorted by score from highest to lowest
  - The top `k` songs are returned as the recommendations (`k` defaults to 5)

**Potential biases to watch for:**

- Because genre carries the most points (2.0 vs 1.0 for mood), the system may over-prioritize genre matches and bury a song that's a near-perfect mood/energy fit but in a different genre.
- Exact-match scoring for genre and mood means adjacent or related tastes (e.g. "indie pop" vs "pop", "chill" vs "relaxed") get zero credit even though a listener might enjoy both — the recipe can't recognize similarity between categories, only identity.
- The acousticness bonus only rewards users who explicitly like acoustic songs; it doesn't penalize non-acoustic songs for users who dislike acoustic, so there's an asymmetry in how that preference is applied.

You can include a simple diagram or bullet list if helpful.

---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows
   ```

2. Install dependencies:

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

```
Loading songs from data/songs.csv...
Loaded songs: 18

Top Recommendations
========================================

1. Sunrise City by Neon Echo
   Score: 4.47 / 5.00
   Reasons:
     - genre matches
     - mood matches
     - energy similarity (1.47 pts)

2. Gym Hero by Max Pulse
   Score: 3.30 / 5.00
   Reasons:
     - genre matches
     - energy similarity (1.30 pts)

3. Rooftop Lights by Indigo Parade
   Score: 2.44 / 5.00
   Reasons:
     - mood matches
     - energy similarity (1.44 pts)

4. Night Drive Loop by Neon Echo
   Score: 1.42 / 5.00
   Reasons:
     - energy similarity (1.42 pts)

5. Concrete Dreams by MC Solstice
   Score: 1.38 / 5.00
   Reasons:
     - energy similarity (1.38 pts)
```

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



