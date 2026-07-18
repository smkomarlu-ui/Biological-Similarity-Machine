# Biological Similarity Engine — Master Pipeline 🧪

A NumPy-based data pipeline that normalizes, splits, encodes, and analyzes feature data to compute pairwise similarity/distance metrics — built as a foundation for biological similarity analysis (e.g., comparing feature vectors representing samples, sequences, or measurements).

## What It Does

The pipeline runs five stages over a dataset of feature vectors:

1. **Normalization** — Z-score normalizes each feature column (mean 0, standard deviation 1)
2. **Shuffle & Split** — Randomly shuffles the dataset and splits it into train (70%), validation (15%), and test (15%) sets
3. **One-Hot Encoding** — Converts integer class labels into one-hot vectors
4. **Pairwise Distance & Similarity** — Computes a full pairwise Euclidean distance matrix and a pairwise cosine similarity matrix across all samples
5. **Batch Variance Analysis** — Splits the training set into batches of 128 samples, computes per-feature variance within each batch, and flags the batch with the highest average variance (potential instability/outlier batch)

## Usage

```bash
python biological_similarity_engine.py
```

The script runs end-to-end on generation and prints summary statistics for each stage — no arguments needed.

### Example Output

```
0.000000
(700, 30)
(150, 30)
(1000, 4)
5.923421
0.012845
3
```

Output corresponds to, in order: normalized feature mean (sanity check, ~0), training set shape, test set shape, one-hot label shape, mean pairwise distance, mean cosine similarity, and the index of the least stable batch.

## Pipeline Details

### 1. Normalization
```python
X_norm = (X - X.mean(axis=0)) / X.std(axis=0)
```
Standardizes each of the 30 features independently across all 1000 samples.

### 2. Shuffle & Split
Uses a random permutation of indices to create non-overlapping train/validation/test sets:

| Split | Size | Indices |
|-------|------|---------|
| Train | 700 | `indices[:700]` |
| Validation | 150 | `indices[700:850]` |
| Test | 150 | `indices[850:]` |

### 3. One-Hot Encoding
Converts integer labels (0–3, representing 4 classes) into one-hot vectors using `np.eye(4)[y]`.

### 4. Pairwise Distance & Cosine Similarity
- **Euclidean distance matrix** — computed via broadcasting (`X_norm[:, None, :] - X_norm[None, :, :]`) to get all pairwise distances in one vectorized operation
- **Cosine similarity matrix** — samples are first unit-normalized, then the similarity matrix is the dot product of unit vectors with their transpose

Both produce a symmetric 1000×1000 matrix.

### 5. Batch Variance / Instability Detection
Training data is split into batches of 128 samples. Per-feature variance is computed within each batch, then averaged across features to get one variance score per batch. The batch with the highest mean variance is flagged as `unstable_batch`.

## Requirements

```
numpy
```

Install with:
```bash
pip install numpy
```

## Notes

- A fixed random seed (`np.random.seed(0)`) is used for reproducibility — re-running the script will always generate the same synthetic data and results.
- The current version generates random synthetic data (`np.random.normal`) as a placeholder — swap out the `X` and `y` generation step with real biological feature data (e.g., sequence embeddings, expression profiles, or measurement vectors) to use this on actual datasets.
- The pairwise distance and similarity matrices scale as O(n²) in memory and compute — this works fine for ~1000 samples but will need batching or approximate methods (e.g., FAISS, ball trees) for much larger datasets.

## Project Structure

```
biological-similarity-engine/
├── biological_similarity_engine.py
├── README.md
└── LICENSE
```

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add some feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
