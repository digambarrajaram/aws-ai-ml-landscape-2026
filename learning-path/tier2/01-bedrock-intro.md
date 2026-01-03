# 🟡 Introduction to Amazon Bedrock

> **Harness the power of foundation models without building from scratch**

## 🎯 Learning Objectives

By the end of this module, you'll be able to:
- Understand foundation models and their capabilities
- Set up Amazon Bedrock access
- Make your first Bedrock API calls
- Build a simple chatbot with Claude

## 🧠 What is Amazon Bedrock?

Amazon Bedrock provides **access to high-performing foundation models** from leading AI companies through a single API:

### Available Models
| Provider | Model | Best For |
|----------|-------|----------|
| **Anthropic** | Claude 3 | Reasoning, analysis, coding |
| **Amazon** | Titan | Text generation, embeddings |
| **Meta** | Llama 2 | Open-source alternative |
| **Cohere** | Command | Business applications |
| **AI21 Labs** | Jurassic | Multilingual tasks |

## 🛠️ Setup: Request Model Access

### Step 1: Enable Bedrock Access
1. Go to AWS Console → Bedrock
2. Navigate to "Model access"
3. Request access to Claude 3 Sonnet
4. Wait for approval (usually instant)

### Step 2: Verify Access
```python
import boto3
import json

bedrock = boto3.client('bedrock-runtime', region_name='us-east-1')

# List available models
bedrock_models = boto3.client('bedrock')
response = bedrock_models.list_foundation_models()

print("Available models:")
for model in response['modelSummaries']:
    print(f"- {model['modelId']}")
```

## 🚀 Your First Bedrock API Call

### Simple Text Generation
```python
import boto3
import json

def chat_with_claude(message):
    bedrock = boto3.client('bedrock-runtime', region_name='us-east-1')
    
    body = json.dumps({
        "anthropic_version": "bedrock-2023-05-31",
        "max_tokens": 1000,
        "messages": [
            {
                "role": "user",
                "content": message
            }
        ]
    })
    
    response = bedrock.invoke_model(
        modelId='anthropic.claude-3-sonnet-20240229-v1:0',
        body=body
    )
    
    result = json.loads(response['body'].read())
    return result['content'][0]['text']

# Test it
response = chat_with_claude("Explain AWS Bedrock in simple terms")
print(response)
```

### Expected Output
```
AWS Bedrock is like having access to a library of pre-trained AI assistants. 
Instead of building your own AI from scratch, you can use models like Claude, 
Titan, or Llama through simple API calls. It's perfect for adding conversational 
AI, text generation, or analysis capabilities to your applications without the 
complexity of training your own models.
```

## 💡 Key Bedrock Concepts

### 1. **Foundation Models**
- Pre-trained on massive datasets
- General-purpose capabilities
- Fine-tunable for specific tasks

### 2. **Prompt Engineering**
- Craft inputs to get desired outputs
- Include context and examples
- Iterate and refine prompts

### 3. **Model Parameters**
- **Temperature**: Creativity (0-1)
- **Max Tokens**: Response length
- **Top P**: Nucleus sampling

## 🧪 Hands-On Labs

### Lab 1: Smart Content Generator
```python
def generate_content(topic, content_type, audience):
    prompt = f"""
    Create a {content_type} about {topic} for {audience}.
    
    Requirements:
    - Engaging and informative
    - Appropriate tone for the audience
    - Include key points and actionable insights
    - Keep it concise but comprehensive
    """
    
    return chat_with_claude(prompt)

# Examples
blog_post = generate_content("AWS AI services", "blog post", "developers")
social_media = generate_content("cloud security", "social media post", "business executives")
tutorial = generate_content("Python basics", "tutorial outline", "beginners")
```

### Lab 2: Document Analyzer
```python
def analyze_document(document_text, analysis_type):
    prompts = {
        "summary": f"Summarize this document in 3-4 key points:\n\n{document_text}",
        "sentiment": f"Analyze the sentiment and tone of this text:\n\n{document_text}",
        "action_items": f"Extract action items and next steps from:\n\n{document_text}",
        "questions": f"Generate 5 thoughtful questions about:\n\n{document_text}"
    }
    
    return chat_with_claude(prompts[analysis_type])

# Test with meeting notes
meeting_notes = """
Team discussed Q4 roadmap. Key decisions:
1. Prioritize mobile app development
2. Hire 2 additional developers by December
3. Launch beta testing in January
4. Marketing campaign starts in February
Budget approved: $150K for development, $50K for marketing.
Next meeting: November 15th to review progress.
"""

summary = analyze_document(meeting_notes, "summary")
actions = analyze_document(meeting_notes, "action_items")
```

### Lab 3: Interactive Chatbot
```python
import streamlit as st

def create_chatbot():
    st.title("🤖 Claude-Powered Assistant")
    
    # Initialize chat history
    if "messages" not in st.session_state:
        st.session_state.messages = []
    
    # Display chat history
    for message in st.session_state.messages:
        with st.chat_message(message["role"]):
            st.markdown(message["content"])
    
    # Chat input
    if prompt := st.chat_input("What would you like to know?"):
        # Add user message
        st.session_state.messages.append({"role": "user", "content": prompt})
        with st.chat_message("user"):
            st.markdown(prompt)
        
        # Get AI response
        with st.chat_message("assistant"):
            response = chat_with_claude(prompt)
            st.markdown(response)
            st.session_state.messages.append({"role": "assistant", "content": response})

# Run the chatbot
create_chatbot()
```

## 🎯 Advanced Prompt Engineering

### Technique 1: Few-Shot Learning
```python
def classify_email(email_content):
    prompt = f"""
    Classify the following email as: urgent, normal, or spam
    
    Examples:
    Email: "CONGRATULATIONS! You've won $1M! Click here now!"
    Classification: spam
    
    Email: "Server down - production impact - need immediate attention"
    Classification: urgent
    
    Email: "Weekly team meeting moved to Thursday 2pm"
    Classification: normal
    
    Email: "{email_content}"
    Classification:
    """
    
    return chat_with_claude(prompt)
```

### Technique 2: Chain of Thought
```python
def solve_problem(problem):
    prompt = f"""
    Solve this step by step:
    
    Problem: {problem}
    
    Let me think through this:
    1. First, I need to understand what's being asked
    2. Then identify the key information
    3. Apply the appropriate method or formula
    4. Calculate the result
    5. Verify the answer makes sense
    
    Solution:
    """
    
    return chat_with_claude(prompt)
```

## 📊 Cost Management

### Pricing Model
- **Pay per token** (input + output)
- **Claude 3 Sonnet**: ~$3 per 1M input tokens, ~$15 per 1M output tokens
- **Titan Text**: ~$0.50 per 1M input tokens, ~$0.65 per 1M output tokens

### Cost Optimization Tips
```python
def optimize_costs():
    tips = [
        "Use shorter prompts when possible",
        "Set appropriate max_tokens limits",
        "Cache responses for repeated queries",
        "Choose the right model for your task",
        "Monitor usage with CloudWatch"
    ]
    return tips
```

## ✅ Knowledge Check

1. **What's the main advantage of Bedrock over building custom models?**
   - Pre-trained capabilities
   - No infrastructure management
   - Multiple model options
   - Faster time to market

2. **Which model would you choose for:**
   - Complex reasoning tasks? → **Claude 3**
   - Cost-effective text generation? → **Titan**
   - Open-source requirements? → **Llama 2**

3. **What's prompt engineering?**
   - Crafting inputs to get desired AI outputs
   - Including context and examples
   - Iterative refinement process

## 🚀 Next Steps

Continue your Bedrock journey:
- [Module 2: Working with Claude & Titan](./02-foundation-models.md)
- [Module 3: Prompt Engineering Best Practices](./03-prompt-engineering.md)
- [Module 4: Fine-tuning Foundation Models](./04-fine-tuning.md)

Or explore:
- [RAG Implementation Guide](./05-rag-patterns.md)
- [Production Bedrock Apps](../../examples/tier2/)

---

🎉 **Great job!** You've unlocked the power of foundation models. Ready to build intelligent applications?