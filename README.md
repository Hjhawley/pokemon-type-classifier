# Pokémon Type Classifier (Sprite-Based CNN)

## Overview

This project builds a machine learning model that predicts a Pokémon’s type(s) based solely on its sprite image.

The task is framed as a **multi-label classification problem**, since Pokémon can have one or two types. The model outputs probabilities for all possible types and selects the top two predictions.

---

## Dataset

The dataset is automatically generated using the PokéAPI:

- Pokémon sprites are downloaded and saved locally
- Type labels are retrieved and stored in a JSON file
- Each Pokémon is associated with one or two types

If a Pokémon has only one type, it is duplicated so all samples are treated consistently as two-label outputs.

---

## Data Preprocessing

Images are processed as follows:

- Resized to 96×96 pixels
- Converted from BGR → RGB
- Normalized to [0, 1]

Labels are encoded using **multi-hot vectors**, where each type corresponds to an index.

The dataset is split into training and validation sets using an 80/20 split.

---

## Model Architecture

The model is a convolutional neural network (CNN) built with TensorFlow/Keras:

- Data augmentation (random horizontal flip)
- 3 convolutional blocks:
  - Conv2D + BatchNorm + MaxPooling
- Fully connected layer (Dense 256 + Dropout)
- Output layer:
  - Sigmoid activation for multi-label classification

The model predicts probabilities for each Pokémon type.

---

## Training

- Loss function: Binary cross-entropy
- Optimizer: Adam
- Metrics:
  - Standard accuracy
  - Custom **"at least one correct" accuracy**

### Custom Metric

Because Pokémon can have two types, a prediction is considered correct if **at least one of the true types appears in the top two predictions**.

This provides a more realistic evaluation than strict exact-match accuracy.

---

## Prediction Pipeline

After training:

1. Images are passed through the model
2. Probabilities are generated for all types
3. Top two predictions are selected
4. Results are saved to a CSV file
5. An HTML page is generated to visualize predictions

This allows easy inspection of model performance. 

---

## Results

The model demonstrates basic ability to classify Pokémon types from visual features, but performance is limited by:

- Small image size (96×96)
- High visual similarity between some types
- Limited dataset complexity

The model performs better when predicting at least one correct type than when predicting both.

---

## What I Learned

- How to build a full ML pipeline (data → model → evaluation → visualization)
- How to handle **multi-label classification problems**
- Why evaluation metrics must match the problem (custom metric was necessary)
- That image-based classification has real limitations when visual features are ambiguous

---

## Limitations

- Model relies only on sprite images (no metadata)
- Very small dataset compared to a typical image classification task

---

## Future Improvements

- Use larger and more detailed images
- Apply transfer learning (pretrained CNNs)
- Add data augmentation beyond horizontal flips
- Incorporate additional features (e.g., Pokédex data)

---

## Tech Stack

- Python
- TensorFlow / Keras
- NumPy
- OpenCV
- PokéAPI
