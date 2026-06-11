import numpy as np

# BIOLOGICAL SIMILARITY ENGINE MASTER PIPELINE:
# Complete Part 1 (Normalization), Part 2 (Shuffle & Split), Part 3 (One-hot encode), Part 4 (Pairwise Euclidean distance & Cosine similarity matrices), and Part 5 (128-sized batch variances).
# Complete the pipeline computations:
np.random.seed(0)
X = np.random.normal(0, 1, size=(1000, 30))
y = np.random.randint(0, 4, size=1000)

# Part 1
X_norm = (X - X.mean(axis=0)) / X.std(axis=0)

# Part 2
indices = np.random.permutation(len(X))
train_idx = indices[:700]
X_train = X_norm[train_idx]
val_idx = indices[700:850]
X_val = X_norm[val_idx]
test_idx = indices[850:]
X_test = X_norm[test_idx]

# Part 3
onehot = np.eye(4)[y]

# Part 4
dist = np.linalg.norm(X_norm[:,None,:] - X_norm[None,:,:], axis=2)
X_unit = X_norm/np.linalg.norm(X_norm, axis=1, keepdims=True)
cosine = X_unit @ X_unit.T

# Part 5
batches = np.array_split(X_train, len(X_train)//128)
vars_ = np.array([b.var(axis=0) for b in batches])
unstable_batch = np.argmax(vars_.mean(axis=1)) 

print(f"{X_norm.mean(axis=0).sum():.6f}")
print(X_train.shape)
print(X_test.shape)
print(onehot.shape)
print(f"{dist.mean():.6f}")
print(f"{cosine.mean():.6f}")
print(unstable_batch)
