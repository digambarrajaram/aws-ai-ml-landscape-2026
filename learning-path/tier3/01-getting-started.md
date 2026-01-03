# 🟢 Getting Started with AWS AI Services

> **Learn to build AI-powered apps without ML expertise**

## 🎯 Learning Objectives

By the end of this module, you'll be able to:
- Set up AWS AI services
- Make your first API calls
- Build a simple AI application
- Understand pricing and limits

## 📚 What You'll Learn

### AWS AI Services Overview
AWS Tier 3 services provide **ready-to-use AI capabilities** through simple API calls:

| Service | Purpose | Use Cases |
|---------|---------|-----------|
| **Rekognition** | Computer Vision | Object detection, face recognition |
| **Textract** | Document Analysis | PDF text extraction, form processing |
| **Comprehend** | Text Analysis | Sentiment, entities, key phrases |
| **Polly** | Text-to-Speech | Voice applications, accessibility |
| **Transcribe** | Speech-to-Text | Meeting transcripts, voice commands |
| **Translate** | Language Translation | Multi-language apps, content localization |

## 🛠️ Hands-On Lab: Your First AI API Call

### Step 1: Setup
```python
import boto3
import json

# Initialize the client
rekognition = boto3.client('rekognition', region_name='us-east-1')
```

### Step 2: Detect Objects in Image
```python
def detect_objects(image_path):
    with open(image_path, 'rb') as image:
        response = rekognition.detect_labels(
            Image={'Bytes': image.read()},
            MaxLabels=10,
            MinConfidence=75
        )
    
    print("Detected objects:")
    for label in response['Labels']:
        print(f"- {label['Name']}: {label['Confidence']:.1f}%")

# Test with your image
detect_objects('test-image.jpg')
```

### Step 3: Expected Output
```
Detected objects:
- Person: 99.2%
- Clothing: 95.8%
- Smile: 87.3%
- Happy: 82.1%
```

## 💡 Key Concepts

### 1. **No ML Knowledge Required**
- Pre-trained models handle complexity
- Simple REST API interface
- JSON request/response format

### 2. **Pay-Per-Use Pricing**
- No upfront costs
- Scale automatically
- Free tier for learning

### 3. **Enterprise Ready**
- High availability
- Security built-in
- Compliance certifications

## 🧪 Practice Exercises

### Exercise 1: Text Analysis
```python
import boto3

comprehend = boto3.client('comprehend')

def analyze_sentiment(text):
    response = comprehend.detect_sentiment(
        Text=text,
        LanguageCode='en'
    )
    
    sentiment = response['Sentiment']
    confidence = response['SentimentScore'][sentiment]
    
    return f"Sentiment: {sentiment} ({confidence:.1%} confidence)"

# Try it
text = "I love using AWS AI services! They make development so much easier."
print(analyze_sentiment(text))
```

### Exercise 2: Document Text Extraction
```python
import boto3

textract = boto3.client('textract')

def extract_text(bucket_name, document_name):
    response = textract.detect_document_text(
        Document={
            'S3Object': {
                'Bucket': bucket_name,
                'Name': document_name
            }
        }
    )
    
    text = ""
    for item in response["Blocks"]:
        if item["BlockType"] == "LINE":
            text += item["Text"] + "\n"
    
    return text

# Extract text from PDF in S3
extracted = extract_text('my-bucket', 'document.pdf')
print(extracted)
```

## 🎯 Mini Project: Photo Analyzer App

Build a simple app that analyzes uploaded photos:

```python
import boto3
import streamlit as st
from PIL import Image

def analyze_photo(image_bytes):
    rekognition = boto3.client('rekognition')
    
    # Detect objects
    labels_response = rekognition.detect_labels(
        Image={'Bytes': image_bytes},
        MaxLabels=10
    )
    
    # Detect faces
    faces_response = rekognition.detect_faces(
        Image={'Bytes': image_bytes},
        Attributes=['ALL']
    )
    
    return labels_response, faces_response

# Streamlit UI
st.title("📸 AI Photo Analyzer")
uploaded_file = st.file_uploader("Choose an image...", type=['jpg', 'jpeg', 'png'])

if uploaded_file:
    image = Image.open(uploaded_file)
    st.image(image, caption='Uploaded Image')
    
    # Analyze
    image_bytes = uploaded_file.getvalue()
    labels, faces = analyze_photo(image_bytes)
    
    # Display results
    st.subheader("🏷️ Detected Objects")
    for label in labels['Labels']:
        st.write(f"- {label['Name']}: {label['Confidence']:.1f}%")
    
    st.subheader("👥 Face Analysis")
    st.write(f"Found {len(faces['FaceDetails'])} face(s)")
```

## 📊 Cost Estimation

### Free Tier Limits (Monthly)
- **Rekognition**: 5,000 images
- **Textract**: 1,000 pages
- **Comprehend**: 50,000 units
- **Polly**: 5 million characters
- **Transcribe**: 60 minutes
- **Translate**: 2 million characters

### Beyond Free Tier
- **Rekognition**: $0.001 per image
- **Textract**: $0.0015 per page
- **Comprehend**: $0.0001 per unit

💡 **Tip**: Start with free tier, monitor usage in AWS Console

## ✅ Knowledge Check

1. **What makes Tier 3 services different from custom ML?**
   - Pre-trained models
   - No ML expertise required
   - Simple API interface
   - Pay-per-use pricing

2. **Which service would you use for:**
   - Extracting text from invoices? → **Textract**
   - Analyzing customer feedback sentiment? → **Comprehend**
   - Building a voice assistant? → **Polly + Transcribe**
   - Moderating user-uploaded images? → **Rekognition**

## 🚀 Next Steps

Ready for more? Continue to:
- [Module 2: Rekognition Deep Dive](./02-rekognition-basics.md)
- [Module 3: Document Processing with Textract](./03-textract-basics.md)

Or jump to:
- [Tier 2: Foundation Models](../tier2/01-bedrock-intro.md) (if you want GenAI)
- [Full Project Examples](../../examples/tier3/)

---

🎉 **Congratulations!** You've made your first AI API calls. You're now ready to build AI-powered applications!