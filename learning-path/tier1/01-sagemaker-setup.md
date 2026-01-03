# 🔴 SageMaker Studio Setup

> **Build custom ML models with full control and maximum performance**

## 🎯 Learning Objectives

By the end of this module, you'll be able to:
- Set up SageMaker Studio environment
- Understand SageMaker's ML lifecycle components
- Create your first notebook instance
- Run a simple ML training job

## 🧠 What is Amazon SageMaker?

Amazon SageMaker is a **fully managed machine learning platform** that provides every component you need for the complete ML lifecycle:

### Core Components
| Component | Purpose | When to Use |
|-----------|---------|-------------|
| **Studio** | Integrated ML IDE | Development and experimentation |
| **Training** | Distributed model training | Large datasets, complex models |
| **Inference** | Model deployment | Real-time and batch predictions |
| **Pipelines** | ML workflow automation | Production ML workflows |
| **Feature Store** | Feature management | Reusable feature engineering |

## 🛠️ Setting Up SageMaker Studio

### Step 1: Create SageMaker Domain
```python
import boto3

sagemaker = boto3.client('sagemaker')

# Create domain (one-time setup)
response = sagemaker.create_domain(
    DomainName='ml-development-domain',
    AuthMode='IAM',
    DefaultUserSettings={
        'ExecutionRole': 'arn:aws:iam::YOUR_ACCOUNT:role/SageMakerExecutionRole'
    },
    VpcId='vpc-12345678',  # Your VPC ID
    SubnetIds=['subnet-12345678']  # Your subnet ID
)

print(f"Domain created: {response['DomainArn']}")
```

### Step 2: Create User Profile
```python
# Create user profile
user_response = sagemaker.create_user_profile(
    DomainId=response['DomainId'],
    UserProfileName='ml-developer',
    UserSettings={
        'ExecutionRole': 'arn:aws:iam::YOUR_ACCOUNT:role/SageMakerExecutionRole'
    }
)

print(f"User profile created: {user_response['UserProfileArn']}")
```

### Step 3: Launch Studio
1. Go to AWS Console → SageMaker
2. Click "Studio" in left navigation
3. Select your domain and user profile
4. Click "Launch Studio"

## 🚀 Your First SageMaker Notebook

### Create New Notebook
```python
# In SageMaker Studio notebook
import sagemaker
import pandas as pd
import numpy as np
from sagemaker import get_execution_role

# Initialize SageMaker session
sagemaker_session = sagemaker.Session()
role = get_execution_role()
bucket = sagemaker_session.default_bucket()

print(f"SageMaker role: {role}")
print(f"Default bucket: {bucket}")
```

### Simple ML Example: Boston Housing
```python
# Load and prepare data
from sklearn.datasets import load_boston
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

# Load dataset
boston = load_boston()
X, y = boston.data, boston.target

# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Scale features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

print(f"Training data shape: {X_train_scaled.shape}")
print(f"Test data shape: {X_test_scaled.shape}")
```

### Train Model with SageMaker
```python
from sagemaker.sklearn.estimator import SKLearn

# Create estimator
sklearn_estimator = SKLearn(
    entry_point='train.py',
    role=role,
    instance_type='ml.m5.large',
    framework_version='0.23-1',
    py_version='py3',
    script_mode=True
)

# Upload data to S3
train_input = sagemaker_session.upload_data(
    path='train_data.csv',
    bucket=bucket,
    key_prefix='housing-data'
)

# Start training
sklearn_estimator.fit({'train': train_input})
```

### Training Script (train.py)
```python
import argparse
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error
import joblib
import os

def model_fn(model_dir):
    """Load model for inference"""
    model = joblib.load(os.path.join(model_dir, "model.joblib"))
    return model

def train():
    parser = argparse.ArgumentParser()
    parser.add_argument('--model-dir', type=str, default=os.environ['SM_MODEL_DIR'])
    parser.add_argument('--train', type=str, default=os.environ['SM_CHANNEL_TRAIN'])
    
    args = parser.parse_args()
    
    # Load data
    train_data = pd.read_csv(os.path.join(args.train, 'train_data.csv'))
    X_train = train_data.drop('target', axis=1)
    y_train = train_data['target']
    
    # Train model
    model = RandomForestRegressor(n_estimators=100, random_state=42)
    model.fit(X_train, y_train)
    
    # Save model
    joblib.dump(model, os.path.join(args.model_dir, "model.joblib"))
    
    print("Model training completed!")

if __name__ == '__main__':
    train()
```

## 🧪 Hands-On Labs

### Lab 1: Data Exploration
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Explore the dataset
def explore_data(X, y, feature_names):
    df = pd.DataFrame(X, columns=feature_names)
    df['target'] = y
    
    # Basic statistics
    print("Dataset Info:")
    print(f"Shape: {df.shape}")
    print(f"Missing values: {df.isnull().sum().sum()}")
    
    # Correlation heatmap
    plt.figure(figsize=(12, 8))
    sns.heatmap(df.corr(), annot=True, cmap='coolwarm', center=0)
    plt.title('Feature Correlation Matrix')
    plt.show()
    
    # Target distribution
    plt.figure(figsize=(8, 6))
    plt.hist(y, bins=30, alpha=0.7)
    plt.xlabel('House Price')
    plt.ylabel('Frequency')
    plt.title('Target Variable Distribution')
    plt.show()

explore_data(X, y, boston.feature_names)
```

### Lab 2: Model Comparison
```python
from sklearn.linear_model import LinearRegression
from sklearn.ensemble import RandomForestRegressor, GradientBoostingRegressor
from sklearn.metrics import mean_squared_error, r2_score

def compare_models(X_train, X_test, y_train, y_test):
    models = {
        'Linear Regression': LinearRegression(),
        'Random Forest': RandomForestRegressor(n_estimators=100, random_state=42),
        'Gradient Boosting': GradientBoostingRegressor(n_estimators=100, random_state=42)
    }
    
    results = {}
    
    for name, model in models.items():
        # Train model
        model.fit(X_train, y_train)
        
        # Make predictions
        y_pred = model.predict(X_test)
        
        # Calculate metrics
        mse = mean_squared_error(y_test, y_pred)
        r2 = r2_score(y_test, y_pred)
        
        results[name] = {'MSE': mse, 'R2': r2}
        
        print(f"{name}:")
        print(f"  MSE: {mse:.2f}")
        print(f"  R2 Score: {r2:.3f}")
        print()
    
    return results

# Compare models
model_results = compare_models(X_train_scaled, X_test_scaled, y_train, y_test)
```

### Lab 3: SageMaker Experiments
```python
from sagemaker.experiments import Experiment, Trial

# Create experiment
experiment = Experiment.create(
    experiment_name='housing-price-prediction',
    description='Compare different algorithms for housing price prediction'
)

# Run trial
with Trial.create(trial_name='random-forest-trial', experiment_name=experiment.experiment_name) as trial:
    # Log parameters
    trial.log_parameter('algorithm', 'RandomForest')
    trial.log_parameter('n_estimators', 100)
    trial.log_parameter('max_depth', 10)
    
    # Train model
    rf_model = RandomForestRegressor(n_estimators=100, max_depth=10, random_state=42)
    rf_model.fit(X_train_scaled, y_train)
    
    # Evaluate
    y_pred = rf_model.predict(X_test_scaled)
    mse = mean_squared_error(y_test, y_pred)
    r2 = r2_score(y_test, y_pred)
    
    # Log metrics
    trial.log_metric('mse', mse)
    trial.log_metric('r2_score', r2)
    
    print(f"Trial completed - MSE: {mse:.2f}, R2: {r2:.3f}")
```

## 📊 Understanding SageMaker Costs

### Instance Types & Pricing
| Instance Type | vCPUs | Memory | Use Case | Cost/Hour |
|---------------|-------|--------|----------|-----------|
| **ml.t3.medium** | 2 | 4 GB | Development | ~$0.05 |
| **ml.m5.large** | 2 | 8 GB | Small training | ~$0.10 |
| **ml.m5.xlarge** | 4 | 16 GB | Medium training | ~$0.20 |
| **ml.p3.2xlarge** | 8 | 61 GB + GPU | Deep learning | ~$3.00 |

### Cost Optimization Tips
```python
def optimize_sagemaker_costs():
    tips = [
        "Use Spot instances for training (up to 90% savings)",
        "Stop notebook instances when not in use",
        "Use appropriate instance sizes",
        "Leverage managed endpoints for inference",
        "Monitor usage with CloudWatch"
    ]
    return tips

# Example: Using Spot instances
sklearn_estimator = SKLearn(
    entry_point='train.py',
    role=role,
    instance_type='ml.m5.large',
    use_spot_instances=True,  # Enable Spot instances
    max_wait=3600,  # Maximum wait time
    max_run=1800,   # Maximum training time
    framework_version='0.23-1'
)
```

## 🔧 SageMaker Best Practices

### 1. **Data Management**
```python
# Use S3 for data storage
def upload_data_to_s3(local_path, s3_path):
    sagemaker_session.upload_data(
        path=local_path,
        bucket=bucket,
        key_prefix=s3_path
    )
    return f"s3://{bucket}/{s3_path}"

# Organize data structure
data_structure = {
    'train': 'data/train/',
    'validation': 'data/validation/',
    'test': 'data/test/'
}
```

### 2. **Model Versioning**
```python
# Use Model Registry
from sagemaker.model import Model

model = Model(
    image_uri=sklearn_estimator.image_uri,
    model_data=sklearn_estimator.model_data,
    role=role
)

# Register model
model_package = model.register(
    content_types=["text/csv"],
    response_types=["text/csv"],
    inference_instances=["ml.t2.medium", "ml.m5.large"],
    transform_instances=["ml.m5.large"],
    model_package_group_name="housing-price-models"
)
```

### 3. **Monitoring & Logging**
```python
# Enable CloudWatch logging
import logging

logger = logging.getLogger(__name__)
logger.setLevel(logging.INFO)

# Log training progress
def log_training_progress(epoch, loss, accuracy):
    logger.info(f"Epoch {epoch}: Loss={loss:.4f}, Accuracy={accuracy:.4f}")
```

## ✅ Knowledge Check

1. **What are the main components of SageMaker?**
   - Studio (IDE)
   - Training (distributed training)
   - Inference (model deployment)
   - Pipelines (workflow automation)

2. **When should you use SageMaker over other tiers?**
   - Need custom algorithms
   - Large-scale training
   - Full control over ML pipeline
   - Complex feature engineering

3. **How can you optimize SageMaker costs?**
   - Use Spot instances
   - Right-size instances
   - Stop unused resources
   - Monitor usage

## 🚀 Next Steps

Continue your SageMaker journey:
- [Module 2: Data Preparation & Feature Engineering](./02-data-prep.md)
- [Module 3: Model Training Basics](./03-training-basics.md)
- [Module 4: Distributed Training](./04-distributed-training.md)

Or explore:
- [SageMaker Pipelines](./08-mlops-pipelines.md)
- [Production Examples](../../examples/tier1/)

---

🎉 **Excellent!** You've set up your SageMaker environment and trained your first model. Ready to build production ML systems?