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
