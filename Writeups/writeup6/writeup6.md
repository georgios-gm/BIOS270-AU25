# Project 2: Machine Learning Project Proposal


### 1. Project Overview

- **Overarching goal**
  This project aims to develop a machine learning model that can predict the the identities and properties of its spatial neighbors in human lung tissue, using Xenium spatial transcriptomics data as ground truth, given a cell's gene expression profile. Ultimately, the goal is to be able to infer spatial context from dissociated single-cell datasets. 
  
- **Rationale**
  Spatial context is critical for understanding how cells interact, especially in complex organs like the lung where structure–function relationships and microenvironments strongly influence cell fate and disease progression.
  New spatial transcriptomics platforms provide both high-resultion spatial coordinates of distinct cell populations and gene expression. However, they are expensive and do not provide whole-transcript throughput. Additionally, the datasets are more limited compared to single cell and single nuclear RNA sequencing data. By learning a mapping from a cell's expression profile to its local neighborhood, this project can assign a spatial dimension to single-cell datasets that will aid in our understanding of cell-cell interactions and improve downstream modeling. 

- **Specific aims**

  - **Aim 1**
    Curate a spatially-annoted single cell dataset from Xenium human lung data.
    This aim will develop the computational to transform each Xenium cell’s transcriptomic profile to a structured representation of its local spatial neighborhood amenable to machine learning and other downstream tasks.

    **Aim 2**
    Train and evaluate machine learning models that predict local cellular neighborhoods from single-cell transcriptomic profiles.
    This aim will leverage the representations developed in Aim 1, including the definitions of different cellular populationad and neighborhoods to develop supervised machine learning models that can predict cell neighborhood composition. 


### 2. Data

- **Dataset description**
  - **Source** For the Xenium spaital transcriptomics dataset of human lung, we plan to leverage published data from Vannan et al. ([Paper](https://doi.org/10.1038/s41588-025-02080-x)). For the single-cell data used at inference, we plan to leverage the Human Lung Cell Atlas ([Paper](https://doi.org/10.1038/s41591-023-02327-2)).
  - **Size** The Xenium data contains 1.6 million cells from 35 unique lungs, and the HLCA contains 2.4 million cells from 486 individuals.
  - **Format** The Xenium data is available in .csv format, and the HLCA is available in .h5ad format. 

- **Data suitability**
  - The current AnnData format is appropriate for single-cell work, but will require modification for downstream ML tasks. 
  - For ML tasks, the ideal preprocessing would involve quality control, normalization, dimentionality reduction, and the construction of an appropriate format for the cellular neighborhoods. 

- **Storage and data management**
  - The data will be stored on our lab server with local copies for development. 
  - The data will be shared through GCP with collaborators, as needed. 


### 3. Environment


- **Coding environment**
  - The primary coding environment will be on the local machine for exploratory analysis and rapid prototyping. Then, we plan to transition to the HPC cluster for preprocessing and training that uses the entire dataset. 

- **Dependencies**
  - Key packages, libraries, or tools required for our analysis include scanpy, anndata, scikit-learn, and pytorch. 

- **Reproducibility**
  - To ensure reproducibility, we plan to use GitHub for version control, a environment.yml file to ensure dependency consistency, and experimental logging for the model hyperparameters. 


### 4. Pipeline

- **Algorithms and methods**  
  Here is the general pipeline and associated methods that this project will employ:
  - Data loading and QC: Load Xenium data and remove low-quality cells with insufficient transcripts or rarely expressed genes. Perform batch correction, if needed. 
  - Cell-type annotation: Map Xenium cells to HLCA-defined cell types to ensure consistency between the different datasets.
  - Neighborhood definition: Computer the Euclidian distance to to other cells for each cell and identify the nearest 25 neighbors. Then, create a neighbor cell-type composition vector that contains the proportion of each cell type in within that 25-rich neighborhood.
  - Features: Generate a gene expression vector for each cell with appropriate normalization and potentially dimensionality reduction. 

- **Scalability and efficiency**  
  Depending on the computational constraints, we may consider reducing the gene expression dimensionality. Additionally, we will leverage appropriate batching when training our model and avoid ever needed to access the full dataset. 


### 5. Machine Learning

- **Task definition**  
  This task is a supervised task where the input is the gene expression vector and the output is a prediction of the proportion of each defined cell population with the the define 25-cell neighborhood. 

- **Feature representation**  
  To encode the single-cell gene expression, we intend to try using normalized counts, the top PCs from PCA, or a more advanced method such as the hidden layer of a self-supervised autoencoder. 

- **Model selection**  
  For our baseline models, we will use traditional machine learning models such as random forest and XGBoost. Then, we will employ deep neural networks and consider representation learning if the Xenium dataset proves too small for the model to learn meaningful representations of the gene expression patters that hit at adjacent cell-cell communication. 

- **Generalization strategy** (for supervised learning)  
  First, in the model training, we will employ proper test/train/val split and regularization to avoid overfitting. Then, we will assess the model on an unseen Xenium dataset performed by a different lab to ensure that the model maintains its accuracy. 

- **Evaluation metrics**  
  We will consider the cosine similarity and correlation between the predicted and observed cellular compositions. Additionally, we will compare with ground-truth biologically-validated method for cell-cell interactions that leverage ligand-receptor pairs. 

