# Discovering Rare Genomic Subtypes via Unsupervised Learning

This project focuses on discovering rare genomic subtypes using unsupervised learning and deep learning approaches. The goal is to identify novel patterns in gene expression data that may reveal clinically relevant subtypes across and within different cancer types.

---

## My Approach

### 1. General Clustering (Pan-Cancer)
**Goal:**  
Look at all samples together, regardless of cancer type.

**Purpose:**  
- Identify broad clusters that might cut across cancer types.  
- Possibly find patterns where a sample behaves more like another cancer type (cross-cancer subtypes).

**Approach:**  
1. Use autoencoders to reduce dimensionality of the 20k+ genes.  
2. Feed the compressed latent representation into clustering algorithms such as:
   - K-Means  
   - DBSCAN  
   - Hierarchical clustering  

**Output:**  
- General clusters of samples sharing similar gene expression patterns.  
- Visualize clusters with t-SNE or UMAP in 2D.

---

### 2. Cancer-Specific Clustering
**Goal:**  
Focus on one cancer type at a time (e.g., BRCA, LUAD) to find rare subtypes.

**Purpose:**  
- Detect smaller, potentially rare subgroups within each cancer type that might have clinical relevance (e.g., treatment response, survival).

**Approach:**  
1. Filter dataset for samples of one cancer type.  
2. Apply the same autoencoder → clustering pipeline.  
3. Compare clusters to known clinical data if available.

**Output:**  
- Subtype clusters within a single cancer type.  
- Rare groups potentially hidden in pan-cancer clustering.

**Why this is useful:**  
- Combining pan-cancer and cancer-specific clustering helps understand global patterns and detect subtle rare subtypes.  
- Clusters can be interpreted by examining gene loadings in the latent space.

---

## Deep Learning Model Architecture (PyTorch)

**Autoencoder Architecture:**

Input Layer (20531 genes)
│
▼
Dense Layer (1024 neurons, ReLU)
▼
Dense Layer (512 neurons, ReLU)
▼
Dense Layer (128 neurons, ReLU) ← Bottleneck / Latent Representation
▼
Dense Layer (512 neurons, ReLU)
▼
Dense Layer (1024 neurons, ReLU)
▼
Output Layer (20531 neurons, Linear)

**Details:**  
- **Encoder:** Compresses 20k+ features → 128 features.  
- **Bottleneck (Latent Space):** Captures key patterns across all genes for clustering.  
- **Decoder:** Reconstructs input to help model learn meaningful structure.  
- **Activations:** ReLU for hidden layers, Linear for output (gene expression is continuous).  
- **Loss Function:** Mean Squared Error (MSE) between input and reconstructed output.

---

### 3. Optional Enhancements
- **Denoising Autoencoder:** Add small Gaussian noise to inputs to learn robust latent representations.  
- **Variational Autoencoder (VAE):** Adds probabilistic interpretation; useful for complex distributions in gene expression.  
- **Dropout / BatchNorm:** Stabilize training in high-dimensional data.  
- **Deeper / Wider Networks:** Depending on resources, increase layers or neurons.

---

### Flow for Clustering
1. Train the autoencoder on the full dataset (all cancer types).  
2. Extract latent vectors from the bottleneck layer.  
3. Apply clustering:
   - **Pan-cancer:** All samples together  
   - **Per-cancer:** Filter samples by cancer type and cluster in latent space  
4. Visualize clusters using t-SNE or UMAP.

---

## References
- Autoencoders for dimensionality reduction  
- t-SNE / UMAP for visualization  
- K-Means, DBSCAN, Hierarchical clustering

---

This project provides a structured pipeline for discovering rare genomic subtypes across and within cancer types, combining deep learning and unsupervised clustering for interpretable results.
