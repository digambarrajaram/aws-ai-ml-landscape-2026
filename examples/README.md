# 🛠️ Essential Code Examples

> **Minimal, production-ready examples for AWS AI/ML services**

## 🟢 Tier 3: AI APIs

### Smart Photo Analyzer
```python
import boto3

class PhotoAnalyzer:
    def __init__(self):
        self.rekognition = boto3.client('rekognition')
    
    def analyze(self, bucket: str, key: str):
        return self.rekognition.detect_labels(
            Image={'S3Object': {'Bucket': bucket, 'Name': key}},
            MaxLabels=5
        )
```

### Document Processor
```python
import boto3

class DocumentProcessor:
    def __init__(self):
        self.textract = boto3.client('textract')
        self.comprehend = boto3.client('comprehend')
    
    def process(self, bucket: str, key: str):
        # Extract text
        response = self.textract.detect_document_text(
            Document={'S3Object': {'Bucket': bucket, 'Name': key}}
        )
        
        text = ' '.join([
            block['Text'] for block in response['Blocks']
            if block['BlockType'] == 'LINE'
        ])
        
        # Analyze sentiment
        sentiment = self.comprehend.detect_sentiment(
            Text=text[:5000], LanguageCode='en'
        )
        
        return {'text': text, 'sentiment': sentiment}
```

## 🟡 Tier 2: Foundation Models

### RAG Chatbot
```python
import boto3
import json

class RAGChatbot:
    def __init__(self):
        self.bedrock = boto3.client('bedrock-runtime')
    
    def chat(self, query: str, context: str = ""):
        prompt = f"Context: {context}\n\nQuestion: {query}\n\nAnswer:"
        
        response = self.bedrock.invoke_model(
            modelId='anthropic.claude-3-sonnet-20240229-v1:0',
            body=json.dumps({
                "anthropic_version": "bedrock-2023-05-31",
                "max_tokens": 500,
                "messages": [{"role": "user", "content": prompt}]
            })
        )
        
        return json.loads(response['body'].read())['content'][0]['text']
```

### Content Generator
```python
import boto3
import json

class ContentGenerator:
    def __init__(self):
        self.bedrock = boto3.client('bedrock-runtime')
    
    def generate_content(self, topic: str, content_type: str = "blog"):
        prompt = f"Write a {content_type} about {topic}. Keep it concise and engaging."
        
        response = self.bedrock.invoke_model(
            modelId='amazon.titan-text-express-v1',
            body=json.dumps({
                "inputText": prompt,
                "textGenerationConfig": {
                    "maxTokenCount": 1000,
                    "temperature": 0.7
                }
            })
        )
        
        return json.loads(response['body'].read())['results'][0]['outputText']
```

## 🔴 Tier 1: Custom ML

### SageMaker Training
```python
import boto3
from sagemaker.sklearn.estimator import SKLearn
from sagemaker import get_execution_role

class MLTrainer:
    def __init__(self):
        self.role = get_execution_role()
    
    def train_model(self, train_data_path: str):
        estimator = SKLearn(
            entry_point='train.py',
            role=self.role,
            instance_type='ml.m5.large',
            framework_version='0.23-1'
        )
        
        estimator.fit({'training': train_data_path})
        return estimator.model_data
```

### Model Deployment
```python
from sagemaker.sklearn.model import SKLearnModel

class ModelDeployer:
    def __init__(self):
        self.role = get_execution_role()
    
    def deploy(self, model_data: str):
        model = SKLearnModel(
            model_data=model_data,
            role=self.role,
            entry_point='inference.py',
            framework_version='0.23-1'
        )
        
        predictor = model.deploy(
            initial_instance_count=1,
            instance_type='ml.m5.large'
        )
        
        return predictor.endpoint_name
```

## 🔗 Multi-Tier Integration

### Complete AI Pipeline
```python
import boto3
import json

class AIProcessor:
    def __init__(self):
        self.textract = boto3.client('textract')
        self.comprehend = boto3.client('comprehend')
        self.bedrock = boto3.client('bedrock-runtime')
        self.sagemaker = boto3.client('sagemaker-runtime')
    
    def process_document(self, bucket: str, key: str):
        # Tier 3: Extract text
        response = self.textract.detect_document_text(
            Document={'S3Object': {'Bucket': bucket, 'Name': key}}
        )
        
        text = ' '.join([
            block['Text'] for block in response['Blocks']
            if block['BlockType'] == 'LINE'
        ])
        
        # Tier 3: Analyze sentiment
        sentiment = self.comprehend.detect_sentiment(
            Text=text[:5000], LanguageCode='en'
        )
        
        # Tier 2: Generate insights
        insights_response = self.bedrock.invoke_model(
            modelId='anthropic.claude-3-sonnet-20240229-v1:0',
            body=json.dumps({
                "anthropic_version": "bedrock-2023-05-31",
                "max_tokens": 500,
                "messages": [{
                    "role": "user", 
                    "content": f"Analyze this text: {text[:1000]}"
                }]
            })
        )
        
        insights = json.loads(insights_response['body'].read())['content'][0]['text']
        
        return {
            'text': text,
            'sentiment': sentiment,
            'insights': insights
        }
```

---

🚀 **Ready to build?** Choose an example and start implementing!