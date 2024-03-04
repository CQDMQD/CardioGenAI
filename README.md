<h1 align="center">CardioGenAI: A Machine Learning-Based Framework for Re-Engineering Drugs for Reduced Cardiotoxicity</h1>

[![](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/gregory-kyro/CardioGenAI/blob/main/LICENSE)

### Summary
Drug-induced cardiotoxicity is a major health concern which can lead to serious adverse effects including life-threatening cardiac arrhythmias via the blockade of cardiac ion channels such as hERG, NaV1.5 and CaV1.2. It is therefore of tremendous interest to develop advanced methods to identify cardiotoxic compounds in early stages of drug development, as well as to optimize commercially available drugs for reduced cardiac ion channel activity. In this work, we present CardioGenAI, a machine learning-based framework for re-engineering both developmental and marketed drugs for reduced cardiotoxicity while preserving their pharmacological activity. The framework incorporates novel state-of-the-art discriminative models for predicting hERG, NaV1.5 and CaV1.2 channel activity, which can also serve independently as effective components of a virtual screening pipeline. We applied the complete framework to pimozide, an FDA-approved antipsychotic agent that demonstrates high affinity to the hERG channel, and generated 100 refined candidates. Remarkably, among the candidates is fluspirilene, a compound which is of the same class of drugs (diphenylmethanes) as pimozide and therefore has similar pharmacological activity, yet exhibits over 700-fold weaker binding to hERG. We have made all of our software open-source to facilitate implementation.

![cgaib](https://github.com/gregory-kyro/CardioGenAI/assets/98780179/56396b7d-b9b7-415b-b4b4-a66c142bfc9f)

## Technical Overview of the Framework
The CardioGenAI framework combines generative and discriminative ML models to re-engineer cardiotoxic compounds for reduced cardiac ion channel activity while preserving their pharmacological action. An autoregressive transformer is trained on a dataset that we previously curated which contains approximately 5 million unique and valid SMILES strings derived from ChEMBL 33, GuacaMol v1, MOSES, and BindingDB datasets. The model is trained autoregressively, receiving a sequence of SMILES tokens as context as well as the corresponding molecular scaffold and ADMET properties, and iteratively predicting each subsequent token in the sequence. Once trained, this model is able to generate valid molecules conditioned on a specified molecular scaffold along with a set of ADMET properties. For an input cardiotoxic compound, the generation is conditioned on the scaffold and ADMET properties of this compound. Each generated compound is subject to filtering based on activity against hERG, NaV1.5 and CaV1.2 channels. Depending on the desired activity against each channel, the framework employs either classification models to include predicted non-blockers or regression models to include compounds within a specified range of predicted pIC50 values. Both the classification and regression models utilize the same architecture, and are trained using three feature representations of each molecule: a feature vector that is extracted from a bidirectional transformer trained on SMILES strings, a molecular fingerprint, and a graph. For each molecule in the filtered generated ensemble and the input cardiotoxic molecule, a feature vector is constructed from the 209 chemical descriptors available through the RDKit Descriptors module. The redundant descriptors are then removed according to pairwise mutual information calculated for every possible pair of descriptors. Cosine similarity is then calculated between the processed descriptor vector of the input molecule and the descriptor vectors of every generated molecule to identify the molecules most chemically similar to the input molecule, but with desired activity against each of the cardiac ion channels.

## Installation and Setup
Follow these instructions to install and set up CardioGenAI on your local machine:

### Cloning the Repository
Clone the CardioGenAI repository to your local environment using the following command:
git clone https://github.com/gregory-kyro/CardioGenAI.git

After cloning, navigate to the CardioGenAI project directory:
cd CardioGenAI

### Downloading Necessary Files
Some essential files are not hosted directly in the GitHub repository due to their size. Please download the following files from the provided Google Drive links:

- Autoregressive_Transformer_parameters.pt <https://drive.google.com/file/d/1oj2OkjRNX3rYN9xv0GKkANjWKf1ebLLN/view?usp=sharing>
- prepared_transformer_data.csv
- raw_transformer_data.csv
- train_hERG.h5

