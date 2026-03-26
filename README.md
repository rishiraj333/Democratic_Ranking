# Democratic_Ranking

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rishiraj333/Democratic_Ranking/blob/main/Project_Topic_Ranking.ipynb)

A lightweight Python utility for aggregating individual student topic rankings into a single, consolidated group ranking. Designed for courses or workshops where you need to surface the most preferred topics across a group — fairly and reproducibly.

---

## Overview

`TopicRankAggregator` takes a set of student preference rankings (each an ordered list of all topic IDs, from most to least preferred) and produces a single aggregated ranking with scores. It assigns **weighted scores** to each rank position — rank 1 receives the highest weight, rank `n` the lowest — and resolves ties using **lexicographical comparison of rank count distributions** for deterministic results.

---

## How It Works

1. **Weight Assignment** — For `n` topics, rank position `k` receives weight `n - k + 1`. So for 10 topics: rank 1 → 10 pts, rank 2 → 9 pts, …, rank 10 → 1 pt.
2. **Score Aggregation** — Weights are summed across all students for each topic to produce a `total_score`.
3. **Tie-Breaking** — Topics with equal total scores are compared lexicographically by their rank count distributions (rank 1 count, then rank 2 count, etc.) for a stable, reproducible result.
4. **Output** — A list of dicts ordered by rank, each containing `rank`, `topic_id`, `title`, and `total_score`.

---

## Installation

No external libraries required — only Python's standard library (`collections.defaultdict`).

```bash
git clone https://github.com/rishiraj333/Democratic_Ranking.git
cd Democratic_Ranking
jupyter notebook Project_Topic_Ranking.ipynb
```

---

## Usage

```python
from collections import defaultdict

# 1. Define your topics
topic_id_to_title = {
    1: "Action Recognition using Vision Transformers",
    2: "Diabetic Retinopathy Grading",
    3: "Human Faces Generation with Diffusion Model",
    # ... up to n topics
}

# 2. Provide student rankings (each list = all topic IDs, best → worst)
student_rankings = [
    [7, 4, 10, 5, 1, 8, 9, 3, 2, 6],  # Student 1
    [7, 1, 10, 6, 5, 8, 4, 9, 2, 3],  # Student 2
    # ...
]

# 3. Aggregate
aggregator = TopicRankAggregator(topic_id_to_title)
final_ranking = aggregator.aggregate(student_rankings)

for item in final_ranking:
    print(f"Rank {item['rank']}: {item['title']} (Score: {item['total_score']})")
```

### Example Output

Using 10 topics and 5 student rankings (see notebook for full inputs):

```
Rank 1:  Prompt Tuning Foundation Vision Language Models for Better Downstream Performance (Score: 49)
Rank 2:  Action Recognition using Vision Transformers (Score: 39)
Rank 3:  Identity-Specific Face Generation with Diffusion Model (Score: 35)
Rank 4:  Video Anomaly Detection by Weakly Supervised Methods (Score: 33)
Rank 5:  Human Sentiment Analysis (Score: 29)
Rank 6:  Training Deep Learning Models for Industrial Waste Classification (Score: 23)
Rank 7:  Training Vision Transformers for Knee Abnormality Classification (Score: 20)
Rank 8:  Human Faces Generation with Diffusion Model (Score: 19)
Rank 9:  Melanoma Classification (Score: 18)
Rank 10: Diabetic Retinopathy Grading (Score: 10)
```

---

## API Reference

### `TopicRankAggregator(topic_id_to_title)`

| Parameter | Type | Description |
|-----------|------|-------------|
| `topic_id_to_title` | `dict` | Maps integer topic IDs to their string titles. |

### `.aggregate(student_rankings) → list[dict]`

| Parameter | Type | Description |
|-----------|------|-------------|
| `student_rankings` | `list[list[int]]` | Each inner list contains **all** topic IDs ranked from best to worst. Every student must rank every topic exactly once. |

**Returns:** A list of dicts sorted by descending rank, each with keys:
- `rank` — Final position (1 = most preferred)
- `topic_id` — Original topic ID
- `title` — Topic title string
- `total_score` — Aggregated weighted score

**Raises:**
- `ValueError` if any student's ranking doesn't include all topics, or contains duplicates.

---

## Scalability Notes

| Scale | Recommendation |
|-------|---------------|
| Small–medium (≤ hundreds of students, tens of topics) | Works out of the box — no changes needed. |
| Large inputs | Load `topic_id_to_title` and `student_rankings` from CSV, JSON, or a database instead of hardcoding. |
| Very large / distributed | Adapt aggregation logic for a framework like Apache Spark. |

---

## Reproducibility

- Input formats are documented with clear examples in the notebook.
- Rankings use integer IDs (not strings) to avoid ambiguity.
- Tie-breaking is fully deterministic — same inputs always produce the same output.
- Track changes with Git; pin any new dependencies in `requirements.txt`.

---

## License

[MIT](LICENSE)    [1, 3, 2, ...],   # Student 2's ranking
    # ...
]

aggregator = TopicRankAggregator(topic_id_to_title)
result = aggregator.aggregate(student_rankings)
print(result)  # Ordered list of topic titles
```

The notebook ships with **10 predefined topics** and **5 example student rankings** to demonstrate the full workflow end-to-end.

---

## How It Works

1. **Weight Assignment** — Each rank position is assigned a weight. For `n` topics, rank 1 receives weight `n`, rank 2 receives `n-1`, and so on down to rank `n` receiving weight `1`.
2. **Score Aggregation** — Weights are summed across all students for each topic.
3. **Tie-Breaking** — When two topics share the same total score, their rank count distributions are compared lexicographically to produce a stable, deterministic result.
4. **Sorting** — Topics are sorted in descending order of total score to produce the final consolidated ranking.

---

## Scalability Notes

- **Small–medium datasets** (hundreds of students, tens of topics): works out of the box with no changes.
- **Large datasets**: consider loading `topic_id_to_title` and `student_rankings` from CSV/JSON files or a database rather than hardcoding them.
- **Very large datasets**: for truly massive scale, the aggregation logic can be adapted to run on distributed frameworks like Apache Spark.

---

## Reproducibility

- Input formats (`topic_id_to_title`, `student_rankings`) are documented in the notebook with clear examples.
- Track changes with Git to maintain a full history of code evolution.
- Pin any additional dependencies in `requirements.txt`.

---

## Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

## License

[MIT](LICENSE)
