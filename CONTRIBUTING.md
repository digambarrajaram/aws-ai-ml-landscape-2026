# 🤝 Contributing to AWS AI/ML Landscape 2026

We welcome contributions from the community! This guide will help you get started with contributing to this project.

## 🎯 How to Contribute

### 1. **Types of Contributions**

We welcome several types of contributions:

- **📚 Documentation**: Improve existing docs, add new guides, fix typos
- **💻 Code Examples**: Add new examples, improve existing ones
- **🏗️ Architecture Patterns**: Share new reference architectures
- **🐛 Bug Reports**: Report issues with examples or documentation
- **💡 Feature Requests**: Suggest new content or improvements
- **🔍 Reviews**: Review pull requests and provide feedback

### 2. **Getting Started**

#### Fork and Clone
```bash
# Fork the repository on GitHub, then clone your fork
git clone https://github.com/YOUR-USERNAME/aws-ai-ml-landscape-2026.git
cd aws-ai-ml-landscape-2026

# Add upstream remote
git remote add upstream https://github.com/original-owner/aws-ai-ml-landscape-2026.git
```

#### Set Up Development Environment
```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements-dev.txt

# Install pre-commit hooks
pre-commit install
```

### 3. **Contribution Workflow**

#### Step 1: Create a Branch
```bash
# Sync with upstream
git fetch upstream
git checkout main
git merge upstream/main

# Create feature branch
git checkout -b feature/your-feature-name
```

#### Step 2: Make Changes
- Follow our [Style Guide](#style-guide)
- Add tests for code examples
- Update documentation as needed
- Test your changes locally

#### Step 3: Commit Changes
```bash
# Stage your changes
git add .

# Commit with descriptive message
git commit -m "Add: New Bedrock RAG example with vector database integration"
```

#### Step 4: Push and Create PR
```bash
# Push to your fork
git push origin feature/your-feature-name

# Create pull request on GitHub
```

## 📝 Style Guide

### **Documentation Style**
- Use clear, concise language
- Include practical examples
- Add diagrams where helpful (Mermaid preferred)
- Follow existing formatting patterns
- Include resource links and references

### **Code Style**
- Follow PEP 8 for Python code
- Include docstrings for functions and classes
- Add type hints where appropriate
- Include error handling
- Add comments for complex logic

### **Example Structure**
```python
"""
Module description and purpose.
"""

import boto3
from typing import Dict, List, Optional

class ExampleClass:
    """
    Brief description of the class.
    
    Attributes:
        attribute_name: Description of attribute
    """
    
    def __init__(self):
        """Initialize the class with required clients."""
        self.client = boto3.client('service-name')
    
    def example_method(self, param: str) -> Dict:
        """
        Brief description of what the method does.
        
        Args:
            param: Description of parameter
            
        Returns:
            Dictionary containing the result
            
        Raises:
            ValueError: When parameter is invalid
        """
        try:
            # Implementation here
            result = self.client.some_operation(Parameter=param)
            return result
        except Exception as e:
            logger.error(f"Operation failed: {e}")
            raise
```

### **Markdown Style**
- Use consistent heading levels
- Include table of contents for long documents
- Use code blocks with language specification
- Include emoji for visual appeal (but don't overuse)
- Link to relevant AWS documentation

## 🏗️ Content Guidelines

### **Adding New Examples**

When adding new code examples:

1. **Choose the Right Tier**
   - Tier 3: Ready-to-use AI APIs
   - Tier 2: Foundation models and GenAI
   - Tier 1: Custom ML with SageMaker

2. **Include Complete Examples**
   - Working code that can be run
   - Error handling and logging
   - Configuration examples
   - Usage instructions

3. **Add Supporting Documentation**
   - README with setup instructions
   - Requirements file
   - Architecture diagram if complex
   - Cost estimation

### **Adding Architecture Patterns**

For new reference architectures:

1. **Provide Visual Diagrams**
   - Use Mermaid for consistency
   - Show data flow clearly
   - Include all AWS services

2. **Include Implementation Guide**
   - Step-by-step instructions
   - CloudFormation/CDK templates
   - Configuration details
   - Security considerations

3. **Add Use Case Context**
   - When to use this pattern
   - Benefits and trade-offs
   - Scaling considerations
   - Cost implications

## 🧪 Testing Guidelines

### **Code Examples**
- All code examples should be tested
- Include unit tests where appropriate
- Test with actual AWS services when possible
- Use mocking for expensive operations

### **Documentation**
- Check all links work
- Verify code examples run
- Test installation instructions
- Validate architecture diagrams

## 📋 Pull Request Guidelines

### **PR Title Format**
Use one of these prefixes:
- `Add:` for new content
- `Update:` for improvements to existing content
- `Fix:` for bug fixes
- `Docs:` for documentation-only changes
- `Refactor:` for code restructuring

### **PR Description Template**
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] New example/content
- [ ] Bug fix
- [ ] Documentation update
- [ ] Architecture pattern
- [ ] Performance improvement

## Testing
- [ ] Code examples tested locally
- [ ] Documentation reviewed
- [ ] Links verified
- [ ] Architecture validated

## Checklist
- [ ] Follows style guide
- [ ] Includes appropriate documentation
- [ ] Tests added/updated
- [ ] No breaking changes
```

### **Review Process**
1. Automated checks must pass
2. At least one maintainer review required
3. Address feedback promptly
4. Squash commits before merge

## 🏷️ Issue Guidelines

### **Bug Reports**
Use the bug report template:
```markdown
**Describe the bug**
Clear description of the issue

**To Reproduce**
Steps to reproduce the behavior

**Expected behavior**
What you expected to happen

**Environment**
- AWS Region:
- Python version:
- Dependencies:

**Additional context**
Any other relevant information
```

### **Feature Requests**
Use the feature request template:
```markdown
**Is your feature request related to a problem?**
Description of the problem

**Describe the solution you'd like**
Clear description of desired feature

**Describe alternatives considered**
Alternative solutions considered

**Additional context**
Any other relevant information
```

## 🎖️ Recognition

Contributors will be recognized in several ways:

- **Contributors List**: Added to README
- **Release Notes**: Mentioned in release notes
- **Social Media**: Highlighted on project social media
- **Conference Talks**: Invited to present contributions

## 📞 Getting Help

If you need help with contributing:

- **GitHub Discussions**: Ask questions in discussions
- **Issues**: Create an issue with the `question` label
- **Discord**: Join our community Discord server
- **Office Hours**: Attend weekly contributor office hours

## 📄 Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold this code.

## 📜 License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to the AWS AI/ML Landscape 2026! 🚀