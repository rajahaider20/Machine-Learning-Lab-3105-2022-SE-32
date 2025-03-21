# Survival Prediction Post-HCT Using Machine Learning  

## 📌 University Details  
**The University of Azad Jammu and Kashmir, Muzaffarabad**  
**Department of Software Engineering**  
**Bachelor of Science in Software Engineering (2022-2026)**  

## 📚 Course Details  
- **Course Title:** Machine Learning  
- **Course Code:** SE-3105  
- **Instructor:** Engr. Ahmed Khawaja  
- **Semester:** Fall 2024  
- **Session:** 2022 – 2026  

## 📝 Submission Details  
- **Submitted By:**  
- **Roll Number:** 2022-SE-23, 32, 26  
- **Degree Program:** BS Software Engineering  
- **Submitted To:** Engr. Ahmed Khawaja  
- **Date:** February 28, 2025  

## 📖 Overview  
This study focuses on **Event-Free Survival (EFS) prediction** for patients undergoing **Hematopoietic Cell Transplantation (HCT)** using a **Neural Network-based model** implemented in PyTorch.  

The project involves:  
✅ **Data Preprocessing**  
✅ **Model Training**  
✅ **Prediction Generation**  
✅ **Submission File Creation**  

## 📂 Project Files  
The implementation is divided into two key files:  

### 1. **Training File:** `train2.ipynb`  
- Trains a **PyTorch-based Artificial Neural Network (ANN)** model.  
- Saves the model as `efs_model.pth`.  
- Stores preprocessing objects for inference.  

### 2. **Testing & Submission File:** `submission-hct.ipynb`  
- Loads the pre-trained ANN model.  
- Preprocesses test data using saved objects.  
- Generates and saves survival predictions.  

## 📊 Dataset  
- **Source:** Provided dataset (`train.csv`, `test.csv`)  
- **Size:** Large-scale dataset  
- **Target Variable:** `efs` (Binary: `0 = event`, `1 = survival`)  

### **Key Features:**  
- **Medical:** `prim_disease_hct`, `hla_match_b_low`, `prod_type`, `gvhd_proph`, `comorbidity_score`, `karnofsky_score`  
- **Demographic:** `age_at_hct`, `donor_age`, `donor_related`, `sex_match`, `year_hct`  
- **Health Indicators:** `obesity`, `prior_tumor`  

## 🚀 Project Workflow  

### 1️⃣ **Data Loading**  
- Train and test datasets are loaded using `pandas`:  

```python  
import pandas as pd  

train_file_path = "train.csv"  
df = pd.read_csv(train_file_path)  

test_file_path = "test.csv"  
df_test = pd.read_csv(test_file_path)  
```

### 2️⃣ **Setup and Preprocessing**  
- Load dataset (`train.csv`) from Kaggle.  
- Select relevant features and the target variable (`efs`).  
- Identify numerical and categorical columns.  
- Handle missing values:  
  - **Numerical:** Median imputation  
  - **Categorical:** Most frequent value imputation  
- Encode categorical variables using One-Hot Encoding.  
- Standardize numerical features using `StandardScaler`.  
- Save preprocessing objects (Imputer, Encoder, Scaler) for inference.  

### 3️⃣ **Prepare Data for Training**  
- Split the dataset into:  
  - **Train (80%)**  
  - **Validation (20%)**  
- Convert data into PyTorch tensors.  
- Use `DataLoader` to create batches for training and validation.  

### 4️⃣ **Define the Neural Network**  
Construct a feedforward neural network with:  
- **3 layers (128 → 64 → 1)**  
- **ReLU activations**  
- **Dropout (30%) for regularization**  
- **Sigmoid activation for binary classification**  
- **Binary Cross-Entropy Loss (BCELoss)** as the loss function.  
- **Adam optimizer** with a learning rate of `0.00001`.  

```python  
import torch  
import torch.nn as nn  

class EFSModel(nn.Module):  
    def __init__(self, input_size):  
        super(EFSModel, self).__init__()  
        self.model = nn.Sequential(  
            nn.Linear(input_size, 128),  
            nn.ReLU(),  
            nn.Dropout(0.3),  
            nn.Linear(128, 64),  
            nn.ReLU(),  
            nn.Dropout(0.3),  
            nn.Linear(64, 1),  
            nn.Sigmoid()  
        )  

    def forward(self, x):  
        return self.model(x)  
```

### 5️⃣ **Train the Model**  
- Train for **100 epochs**.  
- Perform batch training using mini-batches (`batch_size=32`).  
- Compute **training and validation loss** at each epoch.  
- Save the best-performing model based on validation loss (`efs_model.pth`).  

```python  
import torch.optim as optim  

num_epochs = 100  
best_val_loss = float("inf")  
criterion = nn.BCELoss()  
optimizer = optim.Adam(model.parameters(), lr=0.00001)  

for epoch in range(num_epochs):  
    model.train()  
    train_loss = 0.0  
    for X_batch, y_batch in train_loader:  
        X_batch, y_batch = X_batch.to(device), y_batch.to(device)  
        optimizer.zero_grad()  
        outputs = model(X_batch)  
        loss = criterion(outputs, y_batch)  
        loss.backward()  
        optimizer.step()  
        train_loss += loss.item()  
    
    train_loss /= len(train_loader)  
    
    # Validation  
    model.eval()  
    val_loss = 0.0  
    with torch.no_grad():  
        for X_batch, y_batch in val_loader:  
            X_batch, y_batch = X_batch.to(device), y_batch.to(device)  
            outputs = model(X_batch)  
            loss = criterion(outputs, y_batch)  
            val_loss += loss.item()  
    
    val_loss /= len(val_loader)  
    
    print(f"Epoch {epoch+1}/{num_epochs} - Train Loss: {train_loss:.4f} - Val Loss: {val_loss:.4f}")  
    
    # Save best model  
    if val_loss < best_val_loss:  
        best_val_loss = val_loss  
        torch.save(model.state_dict(), "efs_model.pth")  
        print("Model saved!")  
```

## 🚀 How to Use  
### **Train the Model**  
Run the script:  

```bash  
python train_efs_model.py  
```

## 📁 Files in This Repository  
- `train_efs_model.py` → The main script for training the model.  
- `README.md` → This file with workflow details.  
- `/preprocessor/` → Folder containing saved preprocessing objects (imputer, encoder, scaler).  
- `efs_model.pth` → Saved PyTorch model (after training).  

## 📌 Future Improvements  
✅ Implement hyperparameter tuning.  
✅ Experiment with different architectures.  
✅ Deploy the model for real-world inference.  

