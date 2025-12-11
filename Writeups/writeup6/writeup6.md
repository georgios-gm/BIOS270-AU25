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
  What models, algorithms, or computational steps do you plan to run? Are there steps that depends on output of other steps?

- **Scalability and efficiency**  
  How will you ensure your pipeline runs efficiently on your dataset size, format, number of samples?


### 5. Machine Learning

Brainstorm an ML task that can be performed on your data

- **Task definition**  
  What is the supervised or unsupervised learning problem that appropriate for your data?

- **Feature representation**  
  How will you convert raw data into numerical form suitable for modeling?

- **Model selection**  
  Which model(s) will you apply and why?

- **Generalization strategy** (for supervised learning)  
  How will you ensure your model performs well on unseen data?

- **Evaluation metrics**  
  What metrics will you track? Why are they appropriate for your task?
