# DemocraticTopicRankAggregator

A lightweight Python utility for aggregating individual student topic rankings into a single, consolidated ranked list. Ideal for courses, workshops, or surveys where you need to surface the most preferred topics across a group.

---

## Overview

`TopicRankAggregator` takes a set of student rankings (each an ordered list of topic preferences) and produces a single aggregated ranking. It works by assigning **weighted scores** to each rank position — higher ranks receive higher weights — and resolves ties using **lexicographical comparison of rank counts** for deterministic, reproducible results.

---

## Features

- Aggregates rankings from any number of students across any number of topics
- Dynamic weight calculation based on total topic count
- Deterministic tie-breaking via rank count comparison
- Clean, class-based design for easy extension and testing

---

## Project Structure

```
.
├── topic_rank_aggregator.ipynb   # Main Jupyter notebook
├── requirements.txt              # Python dependencies (if any)
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.7+
- Jupyter Notebook or JupyterLab

No external libraries are required beyond Python's standard library (`collections.defaultdict`).

### Installation

```bash
git clone https://github.com/your-username/topic-rank-aggregator.git
cd topic-rank-aggregator
jupyter notebook topic_rank_aggregator.ipynb
```

---

## Usage

Define your topics and student rankings, then run the aggregator:

```python
topic_id_to_title = {
    1: "Machine Learning Fundamentals",
    2: "Data Visualization",
    3: "Natural Language Processing",
    # ... up to N topics
}

student_rankings = [
    [3, 1, 2, ...],   # Student 1's ranking (most to least preferred)
    [1, 3, 2, ...],   # Student 2's ranking
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
