# 🛠️ Environment Setup Guide

## Quick Setup Checklist

- [ ] AWS Account created
- [ ] AWS CLI installed and configured
- [ ] Python 3.8+ installed
- [ ] Required Python packages installed
- [ ] IDE/Editor setup (VS Code recommended)

## 1. AWS Account Setup

### Create AWS Account
1. Go to [aws.amazon.com](https://aws.amazon.com)
2. Click "Create an AWS Account"
3. Follow the registration process
4. **Important**: Most services have free tier limits

## 2. AWS CLI Setup

### Install & Configure
```bash
# Install AWS CLI
pip install awscli

# Configure credentials
aws configure
# AWS Access Key ID: [Your Access Key]
# AWS Secret Access Key: [Your Secret Key]
# Default region name: us-east-1
# Default output format: json
```

## 3. Python Environment

```bash
# Create virtual environment
python -m venv aws-ai-env
source aws-ai-env/bin/activate  # Linux/Mac
# aws-ai-env\Scripts\activate  # Windows

# Install packages
pip install boto3 pandas numpy matplotlib jupyter
```

## 4. Test Setup

```python
# test_setup.py
import boto3

def test_connection():
    try:
        sts = boto3.client('sts')
        identity = sts.get_caller_identity()
        print(f"✅ Connected! Account: {identity['Account']}")
        return True
    except Exception as e:
        print(f"❌ Failed: {e}")
        return False

if __name__ == "__main__":
    test_connection()
```

## 🚨 Free Tier Limits
- **Rekognition**: 5,000 images/month
- **Textract**: 1,000 pages/month  
- **Comprehend**: 50,000 units/month
- **SageMaker**: 250 hours/month
- **Bedrock**: Pay-per-use (varies by model)

✅ **Ready?** Start with [Tier 3 Fundamentals](../tier3/01-getting-started.md)