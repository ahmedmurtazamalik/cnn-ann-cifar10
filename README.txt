================================================================================
CIFAR-10 Image Classification using ANN, CNN, and Hybrid Architectures
Assignment 2 - CS 4045: Deep Learning for Perception
================================================================================

Student Information:
-------------------
Name: Ahmed Murtaza Malik
Roll Number: i22-0985
Section: CS-A
Date: October 23, 2025

================================================================================
CONTENTS OF THIS SUBMISSION
================================================================================

This submission contains the following files and directories:

├── notebooks/
│   └── i220985_A2.ipynb       
│
├── checkpoints/
│   ├── ANN_best.pth      # Trained ANN model weights
│   ├── CNN_best.pth      # Trained CNN model weights
│   └── Hybrid_best.pth   # Trained Hybrid model weights
│
├── report.pdf             
└── README.txt             # This file

================================================================================
SYSTEM REQUIREMENTS
================================================================================

Hardware Used:
-------------
- CPU: AMD Ryzen 5-2600X
- GPU: NVIDIA MSI GeForce GTX 1060 3GB
- RAM: 16GB (recommended minimum: 8GB)

Software Requirements:
---------------------
- Python: 3.8 or higher
- PyTorch: 2.0 or higher (with CUDA support for GPU)
- torchvision: 0.15 or higher
- NumPy: 1.21 or higher
- Matplotlib: 3.5 or higher
- Pillow: 9.0 or higher

To install all dependencies, run:
    pip install torch torchvision numpy matplotlib pillow

Optional (for notebook execution):
    pip install jupyter notebook

================================================================================
REPRODUCIBILITY SETTINGS
================================================================================

Random Seeds (CRITICAL for reproducibility):
--------------------------------------------
All experiments use the following random seeds:

- Python random seed: 42
- NumPy random seed: 42
- PyTorch random seed: 42
- PyTorch CUDA seed: 42

These are set at the beginning of each notebook to ensure reproducible results.

Dataset:
-------
CIFAR-10 will be automatically downloaded by torchvision when you run the 
notebooks for the first time. The dataset will be saved in a './data' directory.

- Training images: 50,000 (40,000 train / 10,000 validation with 80/20 split)
- Test images: 10,000
- Image size: 32×32×3 (RGB)
- Classes: 10 (airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck)

================================================================================
HOW TO RUN THE NOTEBOOKS
================================================================================

Option 1: Running Locally
-------------------------
1. Ensure all dependencies are installed (see Software Requirements above)

2. Navigate to the notebooks/ directory:
   cd notebooks/

3. Start Jupyter Notebook:
   jupyter notebook

4. Open any of the three notebooks:
   - ANN.ipynb
   - CNN.ipynb
   - Hybrid.ipynb

5. Run all cells sequentially from top to bottom:
   - Click "Cell" → "Run All" in the Jupyter menu
   - OR press Shift+Enter to run cells one by one

6. The notebook will:
   - Download CIFAR-10 dataset (if not already present)
   - Define the model architecture
   - Train the model (or load pre-trained weights)
   - Evaluate on test set
   - Display results and visualizations

Option 2: Running on Google Colab
---------------------------------
1. Upload the notebook to Google Drive

2. Open with Google Colab

3. Enable GPU:
   - Click "Runtime" → "Change runtime type"
   - Select "GPU" from Hardware accelerator dropdown
   - Click "Save"

4. Upload checkpoint files if you want to skip training:
   - Use the file upload feature in Colab
   - Adjust file paths in the notebook accordingly

5. Run all cells sequentially

Note: Training times will vary based on hardware. On Google Colab's free GPU,
expect similar or faster training times compared to GTX 1060.

================================================================================
REPRODUCING FINAL RESULTS
================================================================================

To reproduce the exact test accuracies reported in the paper:

METHOD 1: Load Pre-trained Checkpoints (RECOMMENDED - Fast)
----------------------------------------------------------
1. Open the notebook

2. Look for the section titled "8. Final Evaluation"

3. Ensure the checkpoint path is correct:
   - Default: '../checkpoints/[model]_best.pth'
   - Adjust if needed based on your directory structure

4. Run the evaluation cells

5. Expected output:
   - ANN Test Accuracy: 55.93%
   - CNN Test Accuracy: 90.84%
   - Hybrid Test Accuracy: 91.13%

METHOD 2: Train from Scratch (SLOW - Requires GPU)
-------------------------------------------------
1. Open the notebook

2. Run all cells from the beginning

3. Training times (on GTX 1060 3GB):
   - ANN: ~30 minutes (50 epochs)
   - CNN: ~39 minutes (50 epochs)
   - Hybrid: ~39 minutes (50 epochs)

4. Models will automatically save checkpoints during training

5. Final evaluation will run automatically at the end

Note: Due to randomness in training (even with fixed seeds), you may observe
slight variations (±0.5%) in final accuracy when training from scratch.

================================================================================
MODEL ARCHITECTURES SUMMARY
================================================================================

ANN (Artificial Neural Network):
--------------------------------
- Input: 32×32×3 images flattened to 3,072-dimensional vectors
- Architecture: 4 fully-connected layers (2048→1024→512→256→10)
- Parameters: 9,058,058
- Features: BatchNorm, ReLU activation, 50% Dropout
- Test Accuracy: 55.93%

CNN (Convolutional Neural Network):
-----------------------------------
- Input: 32×32×3 images (spatial structure preserved)
- Architecture: 3 convolutional blocks (64→128→256 filters)
- Each block: 2×Conv3×3 + BatchNorm + ReLU + MaxPool2×2
- Classifier: Flatten → FC(512) → FC(10)
- Parameters: 3,251,018
- Test Accuracy: 90.84%

Hybrid (CNN Feature Extractor + ANN Classifier):
------------------------------------------------
- Feature Extractor: Same 3 CNN blocks as above
- Classifier: 2-layer ANN (4096→512→256→10)
- Parameters: 3,380,298
- Test Accuracy: 91.13% (BEST)

================================================================================
TRAINING CONFIGURATION
================================================================================

Common Hyperparameters (All Models):
------------------------------------
- Optimizer: Adam
- Learning Rate: 0.001
- Weight Decay: 1e-4
- Batch Size: 128
- Max Epochs: 50
- Early Stopping: Patience = 10 epochs
- Dropout: 0.5
- Loss Function: CrossEntropyLoss

Data Preprocessing:
------------------
- Normalization: Mean=(0.4914, 0.4822, 0.4465), Std=(0.2470, 0.2435, 0.2616)
- Training Augmentation:
  * RandomCrop(32, padding=4)
  * RandomHorizontalFlip(p=0.5)
- Validation/Test: Only normalization (no augmentation)

Learning Rate Schedule:
----------------------
- Adaptive scheduler enabled
- Reduces learning rate when validation loss plateaus