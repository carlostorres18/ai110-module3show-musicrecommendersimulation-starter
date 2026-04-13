# 🎵 Music Recommender Simulation

## Project Summary

In this project you will build and explain a small music recommender system.

Your goal is to:

- Represent songs and a user "taste profile" as data
- Design a scoring rule that turns that data into recommendations
- Evaluate what your system gets right and wrong
- Reflect on how this mirrors real world AI recommenders

My version scores each song by comparing it to a user's favorite genre, favorite mood, and target energy level. Songs that match genre and mood and are close to the target energy receive higher scores. The system ranks all songs by score and returns the top recommendations with short explanations.

---

## How The System Works

Explain your design in plain language.

Some prompts to answer:

- What features does each `Song` use in your system
  - For example: genre, mood, energy, tempo
  - Each Song has: id, title, artist, genre, mood, energy, tempo_bpm, valence, danceability, acousticness

- What information does your `UserProfile` store
  - It stores the following:
    - favorite_genre: the user's preferred genre
    - favorite_mood: the user's preferred mood
    - target_energy: the energy level they want (as a float)
    - likes_acoustic: whether they prefer acoustic songs (True/False)

- How does your `Recommender` compute a score for each song
  - The way that it works is the following: 
    - 1. Computes how well one song matches the user.
    - 2. It then produces a comparable numeric value (between 0 - 1)
    - 3. Lastly it then lets you tune feature importance with weights

- How do you choose which songs to recommend
  - 1. Score each song against the user profile
  - 2. Then it ranks each songs by final score

You can include a simple diagram or bullet list if helpful.

### Finalized Algorithm Recipe

1. Load the song catalog from `data/songs.csv`.
2. Represent each song with features: genre, mood, and energy (plus other metadata fields).
3. Represent the user with a taste profile: favorite genre, favorite mood, target energy, and acoustic preference.
4. For each song, compute a match score:
  - Add a genre match component (higher if genres match).
  - Add a mood match component (higher if moods match).
  - Add an energy similarity component based on how close song energy is to target energy.
  - Optionally apply feature weights so some signals matter more than others.
5. Normalize or keep the score on a comparable 0 to 1 scale.
6. Rank songs from highest score to lowest score.
7. Return the top `k` songs as recommendations.
8. Generate a short explanation for each recommendation using the strongest matching features.

![Terminal Screenshot](Terminal%20Screenshot.png)



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

## Experiments You Tried

Use this section to document the experiments you ran. For example:

- What happened when you changed the weight on genre from 2.0 to 0.5
- What happened when you added tempo or valence to the score
- How did your system behave for different types of users

### Testing Different User Profiles

I ran the recommender against four contrasting user profiles to understand how it handles different taste preferences:

**Gym Hero vs. Happy Chill Pop:**
The "Gym Hero" profile (pop, intense, high energy 0.93) correctly got "Gym Hero" as the top recommendation. By contrast, the "Happy Chill Pop" profile (pop, happy, medium energy 0.76) correctly got "Sunrise City" (happy, pop) at #1. However, the surprise: "Gym Hero" appeared at position #3 in the Happy Chill Pop recommendations, even though it's labeled "intense" (not happy). Why? Because the energy is only 0.17 away from target, earning 3.32 energy points. When added to the 1.0 genre match (it's pop), it totals 4.32 points—enough to beat out "Rooftop Lights" (indie pop, 0.76 energy, 4.5 points). **The takeaway:** Energy proximity dominates mood preference. A song can be the "wrong mood" but still rank high if the energy is close enough.

**Late Night Coding vs. Weekend Acoustic:**
"Late Night Coding" (lofi, focused, low energy 0.40) returned excellent recommendations—all lofi or low-energy, all with strong focus/chill vibes. "Weekend Acoustic" (folk, relaxed, low energy 0.35), however, should find relaxed acoustic songs, but instead got "Coffee Shop Stories" (jazz, relaxed, 0.37 energy) at #1 and "Paper Crown" (folk, nostalgic, 0.33 energy) at #2. Why did the jazz song rank first? It's 0.02 away from target energy (0.37 vs 0.35), while Paper Crown is 0.02 away on the other side (0.33 vs 0.35)—both were equally close, but mood match for jazz (relaxed) broke the tie. **The biggest shock:** The `likes_acoustic` preference was never used in scoring, so genuinely acoustic folk songs didn't get any bonus points. The system recommended jazz, lofi, and ambient based purely on energy and mood, ignoring the acoustic texture entirely.

### Adversarial User Profile Testing

The following outputs demonstrate how the recommender handles three distinct adversarial user profiles designed to test edge cases and potential scoring weaknesses:

![Profile Output 1](Profile%20Output1.png)

![Profile Output 2](Profile%20output%202.png)

![Profile Output 3](Profile%20output%203.png)

---

## Limitations and Risks

Summarize some limitations of your recommender.

Examples:

- It only works on a tiny catalog
- It does not understand lyrics or language
- It might over favor one genre or mood

You will go deeper on this in your model card.

Potential bias note: This system might over-prioritize genre, ignoring strong mood or energy matches from songs outside the user's favorite genre.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this


---

## 7. `model_card_template.md`

Combines reflection and model card framing from the Module 3 guidance. :contentReference[oaicite:2]{index=2}  

```markdown
# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

**MusicMatcher 1.0**

---

## 2. Goal / Task  

This recommender suggests songs based on what a user wants to hear right now. It takes three things a user cares about—favorite genre, favorite mood, and desired energy level—and finds the best songs from our catalog. It's built for classroom learning, not real users.

---

## 3. Data Used  

We have **18 songs** in our catalog. They cover many genres: pop, rock, lofi, jazz, hip hop, folk, and more. Each song has features like energy level, mood (happy, chill, intense, etc.), and acoustic-ness. The dataset is balanced enough for most user tastes, but it could use more songs in some genres like classical and metal. We did not add or remove songs from the starter set.

---

## 4. Algorithm Summary  

The recommender scores each song using three rules:
- **Genre**: If the song's genre matches what the user likes, it gets +1.0 points.
- **Mood**: If the song's mood matches, it gets +1.5 points.
- **Energy**: The closer a song's energy level is to what the user wants, the more points it gets—up to +4.0 if it's a perfect match.

Songs are ranked by their total score, and we show the top 5. Energy matters the most because it has the highest point value.

---

## 5. Observed Behavior / Biases  

**Energy dominates the score.** A song with the right genre but wrong energy will often rank lower than a song with the wrong genre but matching energy. This means the recommender gets stuck recommending songs in narrow energy ranges. For example, someone who likes relaxing music might never find upbeat acoustic songs, even if they love acousticness. The `acousticness` feature exists in the data but the scoring formula completely ignores it. This is a major limitation.

---

## 6. Evaluation Process  

We tested the recommender with different user profiles:

- **Gym Hero** (pop, intense, 0.93 energy): Correctly recommended high-energy songs, but picked rock songs over other pop songs because energy was so close.
- **Happy Chill Pop** (pop, happy, 0.76 energy): Got pop songs, but energy proximity was more important than mood—some songs ranked high just because their energy was close enough.
- **Late Night Coding** (lofi, focused, 0.40 energy): Worked great because all three preferences aligned.
- **Weekend Acoustic** (folk, relaxed, 0.35 energy): Got jazz and lofi songs instead of folk because the algorithm doesn't consider acousticness.

The tests showed the algorithm works when preferences align, but struggles when users want specific secondary features like acoustic-ness.

---

## 7. Intended Use and Non-Intended Use  

**Intended Use:**
- Classroom projects to learn how recommenders work.
- A simple tool to explore what scores different user profiles get.
- Teaching the tradeoffs between fairness and simplicity.

**Non-Intended Use:**
- Do not use this for real music streaming apps—it's too simple.
- Do not rely on it for mission-critical decisions.
- Do not assume it works fairly for all music tastes.

---

## 8. Ideas for Improvement  

1. **Add acousticness to the formula.** Right now we calculate it but ignore it. Adding it as "+0.5 points for high acousticness" would help users who care about instrument vs. electronic music.

2. **Lower the energy weight.** Change energy from +4.0 to +2.5 so genre and mood matter more. This would give users more diverse recommendations instead of getting stuck in one energy range.

3. **Add more songs.** We only have 18 songs. Adding 50-100 more would let us test with more user profiles and reduce the chance of recommending the "wrong" song just because it's the best available.  ---

## 9. Personal Reflection  

Working on this project taught me that recommender systems seem simple on the surface but have real tradeoffs. The energy weight dominates so much that I realized scoring formulas need careful balance. If one feature is worth 4x more than another, that's what users will experience. I also learned that "missing data" isn't just about the songs in the catalog, it's about features we collect but then ignore. The acousticness data was sitting there the whole time, and the algorithms didn't use it. That's a reminder to either collect what you'll use or explain why you're not using it. This changed how I think about apps on my phone: every recommendation is probably weighted toward something I don't even know about.  
