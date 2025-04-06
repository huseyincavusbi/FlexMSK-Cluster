# FlexMSK-Cluster

## Objective

This Jupyter Notebook performs an unsupervised multi-omics analysis using mutation and copy number alteration (CNA) data from the MSK-IMPACT 2017 cohort available on cBioPortal. 

## Data

*   **Source:** [MSK-IMPACT Clinical Sequencing Cohort (MSK, Nat Med 2017)](https://www.cbioportal.org/study/summary?id=msk_impact_2017) via cBioPortal.
*   **Data Types Used:**
    *   Somatic Mutations (`data_mutations.txt`)
    *   Copy Number Alterations (`data_cna.txt`)
    *   Clinical Sample Information (`data_clinical_sample.txt`)

## Methodology

1.  **Data Acquisition & Preprocessing:** Data is fetched from cBioPortal and preprocessed using the `flexynesis` framework. 
2.  **Train/Test Split:** Data is split into training and testing sets.
3.  **Feature Selection:** Features (genes) within mutation and CNA data are selected based on variance and Laplacian scores (keeping top 90%, min 500 features).
4.  **Unsupervised Learning:** A Variational Autoencoder (VAE) with no target variable is trained on the training set. Hyperparameter optimization (`flexynesis.HyperparameterTuning`) is used to find the best model configuration based on hyperparameter optimization over 25 iterations.
5.  **Embedding Generation:** The trained VAE is used to generate low-dimensional latent space embeddings for the test set samples.
6.  **Clustering:** Louvain community detection and K-Means clustering are applied to the test set embeddings.
7.  **Visualization:** PCA, UMAP, and t-SNE are used to visualize the embeddings and cluster assignments in 2D space.
8.  **Cluster Characterization:** The identified Louvain clusters are analyzed for associations with clinical variables using statistical tests (Chi-squared, Kruskal-Wallis, ANOVA) and visualizations (countplots, boxplots, stacked bar plots). Variables analyzed include:
    *   Primary Tumor Site
    *   Tumor Mutational Burden (TMB)
    *   Detailed Cancer Type
    *   Sample Type (Primary/Metastasis)
    *   Tumor Purity

## Key Libraries

*   `flexynesis`
*   `pandas`
*   `numpy`
*   `torch`
*   `matplotlib`
*   `seaborn`
*   `scipy`
*   `scikit-learn`
*   `umap-learn`

## How to Run

1.  Ensure the required libraries are installed.
2.  Execute the notebook cells sequentially. The notebook will:
    *   Download data from cBioPortal into a `msk_impact_2017` directory.
    *   Process and split the data, saving results into an `msk_impact` directory (with `train` and `test` subdirectories).
    *   Train the VAE model.
    *   Perform clustering, visualization, and statistical analysis.

## Results

The notebook outputs:

*   A trained VAE model optimized for reconstructing the input multi-omics data.
*   Latent space embeddings for the test samples.
*   Cluster assignments for test samples based on Louvain and K-Means algorithms.
*   Visualizations (PCA, UMAP, t-SNE plots) of the embeddings colored by cluster labels.
*   Statistical analysis results and plots showing the association between identified clusters and variables like primary site, TMB, sample type, and tumor purity.
*   Saved CSV files containing the split training and testing data (`msk_impact/train/`, `msk_impact/test/`).

# Acknowledgements

This project was inspired by the BIMSB Computational Genomics 2025 course (taught by Dr. Bora Uyar, https://github.com/BIMSBbioinfo/compgen_course_2025_module3). Thank you to all the instructors and contributors.

This project utilizes the `flexynesis` framework: [https://github.com/BIMSBbioinfo/flexynesis]

Uyar, B., Savchyn, T., Wurmus, R., Sarigun, A., Shaik, M. M., Franke, V., & Akalin, A. (2024). Flexynesis: A deep learning framework for bulk multi-omics data integration for precision oncology and beyond. *bioRxiv*, 2024.07.16.603606. https://doi.org/10.1101/2024.07.16.603606

## License

This project is licensed under the MIT LICENSE - see the [LICENSE](LICENSE) file for details.
