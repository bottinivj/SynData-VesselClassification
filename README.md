# SynData-VesselClassification

#DS_OG_5K https://drive.google.com/file/d/1WkEASKXEFAzOd1rytX5tFv_-OvdrswRN/view?usp=sharing

#DS_OG_54K https://drive.google.com/file/d/1o_kEn8CcjdjMas2sCc_Jm26nGjqJSG5u/view?usp=sharing

This repository contains the code, experiment pipelines, and supporting materials for my master's dissertation on synthetic data generation for maritime vessel classification.

The study investigates the impact of classical data augmentation and deep generative models on image classification performance under limited labeled data conditions. The repository includes generation pipelines, dataset construction scripts, filtering and super-resolution workflows, image quality metrics, and classification experiments using YOLO12 and ResNet512.

## Repository Structure

- `DDPM/` — code to generate synthetic datasets with DDPM
- `DCGAN/` — code to generate synthetic datasets with DCGAN
- `VAE/` — code to generate synthetic datasets with VAE
- `AUG1&2/` — classical data augmentation pipelines
- `ESRGAN-X4/` — super-resolution pipeline
- `FILTER/` — filtering pipeline based on generated datasets
- `METRICS/` — image generation quality metrics
- `CLASS-YOLO12/` — classification scripts using YOLO12
- `CLASS-RESNET512/` — classification scripts using ResNet512
- `DATASET-SEEDS.ipynb` — notebook to create seeded dataset splits with multiple synthetic proportions

## Replication Pipeline

To replicate the study, follow the steps below.

### 1. Generate the synthetic base datasets

Start with the original dataset `DS_OG_54K`.

Run the code in the following folders:

- `DDPM/`
- `DCGAN/`
- `VAE/`

The outputs of these pipelines should be the following synthetic datasets:

- `DDPM-F64`
- `DCGAN-F64`
- `VAE-F64`

These datasets are the synthetic image pools later used to build the experimental training sets.

### 2. Create the seeded datasets for DDPM, DCGAN, and VAE

To create the datasets used in the classification experiments, run the notebook `DATASET-SEEDS.ipynb` separately for each method:

- DDPM
- DCGAN
- VAE

The notebook will:

- split `DS_OG_5K`
- apply the 5 predefined random seeds
- recursively add the selected synthetic proportions from:
  - `DDPM-F64`
  - `DCGAN-F64`
  - `VAE-F64`

For each generative method, this process creates datasets for all seeds and proportions.
In total, **45 datasets** are generated for each method.

### 3. Create the AUG1 and AUG2 datasets

To generate the classical augmentation datasets, run the code in the `AUG1&2/` folder.

Inside the code, select one pipeline at a time:

- `AUG1`
- `AUG2`

The script will generate datasets for:

- the 5 predefined seeds
- all evaluated augmentation proportions

For each augmentation pipeline, this results in **45 datasets**.

### 4. Run the classification experiments

To run the classification stage:

1. Create one folder for each method.
2. Place the corresponding 45 datasets in that folder.
3. Place the 45 classification scripts for either:
   - `YOLO12`, or
   - `RESNET512`
4. Run each script individually.

By doing this, it is possible to reproduce all classification results reported in the study.

### 5. Apply super-resolution if desired

If you want to perform resolution enhancement, run the code in the `ESRGAN-X4/` folder on any dataset.

This will generate a new higher-resolution dataset.

After creating the super-resolved dataset, repeat:

- **Step 2** to create the seeded datasets
- **Step 4** to run the classification experiments

> **Important:** when using higher-resolution datasets, make sure to update the resize settings in the preprocessing and classification pipelines to the correct image size.

### 6. Apply filtering if desired

To filter a generated dataset, run the code in the `FILTER/` folder on any desired dataset.

After filtering, apply the output dataset to any of the generative-model pipelines and then repeat:

- **Step 2** to create the seeded datasets
- **Step 4** to rerun the classification experiments

This makes it possible to evaluate the effect of dataset filtering on downstream classification performance.

### 7. Compute image generation metrics

To compute image quality metrics for any generated dataset, run the code in the `METRICS/` folder.

These scripts can be applied to any dataset to obtain the generation-quality results used in the study.

## Notes

- The full experimental workflow depends on consistent dataset naming and directory organization.
- Seeded dataset creation and classification execution must be repeated independently for each method.
- When using filtered or super-resolved datasets, the downstream steps remain the same, but preprocessing parameters may need to be updated accordingly.

## Citation

If you use this repository, please cite:

@inproceedings{jardim2025enhancing,
  title={Enhancing Vessel Classification with Synthetic Data},
  author={Jardim, Vinicius B and de Figueiredo, Felipe AP and Mafra, Samuel B},
  booktitle={2025 Computing, Communications and IoT Applications (ComComAp)},
  pages={154--159},
  year={2025},
  organization={IEEE}
}

## Contact

For questions about the code or replication details, please open an issue in this repository.
