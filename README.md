# Credit-Card-Fraud-Detection-using-Machine-Learning

## 📌 Project Overview
Credit card fraud is a major concern for financial institutions and customers worldwide. With the increasing volume of digital transactions, traditional fraud detection methods (manual reviews, static rules) often struggle to keep pace.  

This project demonstrates how to use **Azure Machine Learning Studio** to build, train, and deploy a **classification model** that can detect fraudulent transactions in real time.  

The solution leverages **Automated Machine Learning (AutoML)** to automatically find the best-performing algorithm, deploys it as a predictive web service, and tests it with sample transactions.

---

## 🛠️ Tech Stack
- **Azure Machine Learning Studio** (AutoML, Workspace, Datasets, Compute)
- **Azure Blob Storage**
- **Azure Compute Instance / Cluster**
- **JupyterLab**
- **Python (for testing endpoint)**

---

## 🎯 Key Features
- Import and preprocess historical credit card transaction data  
- Use **AutoML** for classification to detect fraud vs. legitimate transactions  
- Deploy the best model as a **real-time endpoint**  
- Test predictions using JupyterLab with REST API calls  
- Ensure scalability with Azure Compute resources  
- Share results on LinkedIn / Community for professional visibility  

---

## 📂 Resources:
┣ 📄 CreditCard.csv # Dataset used for training
┣ 📄 deploy_test.ipynb # Jupyter notebook for testing endpoint
┗ 📄 requirements.txt # Python dependencies




---

## 🚀 Steps to Reproduce

### 1️⃣ Create Azure Machine Learning Workspace
- Log in to [Azure Portal](https://portal.azure.com)  
- Search for **Azure Machine Learning** → Create a new workspace  
- Configure Subscription, Resource Group, and Workspace Name  

### 2️⃣ Launch Azure Machine Learning Studio
- Go to the created workspace  
- Open **Studio Web URL**  

### 3️⃣ Create Compute Instance
- Navigate to **Compute → +New**  
- VM type: CPU (Standard_DS11_v2 recommended)  
- Start and confirm instance is **Running**  

### 4️⃣ Upload Dataset
- Download dataset: `CreditCard.csv`  
- In Studio → **Data → +Create** → Upload local file  
- Configure as **Tabular dataset**  

### 5️⃣ Run AutoML Experiment
- Navigate to **Automated ML → +New Automated ML Job**  
- Configure:  
  - Job Name: `detectfraud`  
  - Task: `Classification`  
  - Target Column: `Class` (0 = Legit, 1 = Fraud)  
- AutoML runs multiple models and selects the best  

### 6️⃣ Deploy Model
- Select best model → **Deploy → Real-time endpoint**  
- Configure endpoint name: `frauddetection1`  
- Instance Count: 2  

### 7️⃣ Test Endpoint
Use Python in JupyterLab to test endpoint:

```python
import urllib.request, json, os, ssl

def allowSelfSignedHttps(allowed):
    if allowed and not os.environ.get('PYTHONHTTPSVERIFY', ''):
        ssl._create_default_https_context = ssl._create_unverified_context

allowSelfSignedHttps(True)

data = {
  "input_data": {
    "columns": ["Time","V1","V2","V3","V4","V5","V6","V7","V8","V9","V10","V11","V12",
                "V13","V14","V15","V16","V17","V18","V19","V20","V21","V22","V23","V24",
                "V25","V26","V27","V28","Amount"],
    "data": [[0,-1.35,-0.07,2.53,1.37,-0.33,0.46,0.23,0.09,0.36,0.09,-0.55,-0.61,-0.99,
              -0.31,1.46,-0.47,0.20,0.02,0.40,0.25,-0.01,0.27,-0.11,0.06,0.12,-0.18,
              0.13,-0.02,149.62]]
  }
}

body = str.encode(json.dumps(data))

url = '<YOUR_ENDPOINT_URL>'
api_key = '<YOUR_API_KEY>'

headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

req = urllib.request.Request(url, body, headers)

response = urllib.request.urlopen(req)
result = response.read()
print(result)
**Result → 0 (Legit) or 1 (Fraud)
