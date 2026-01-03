# 🎨 Diagram Assets Guide

## 📁 Diagram Files Location

All architecture diagrams are stored in `/assets/diagrams/` and referenced throughout the documentation.

## 🛠️ Creating Diagrams

### Recommended Tools

1. **Lucidchart** (Professional)
   - AWS architecture templates
   - Official AWS icons
   - Collaborative editing
   - Export as PNG/SVG

2. **Draw.io (diagrams.net)** (Free)
   - AWS icon library
   - Web-based editor
   - GitHub integration
   - Multiple export formats

### Diagram Standards

#### Color Scheme
- **Tier 1 (Custom ML)**: `#FF6B6B` (Red)
- **Tier 2 (Foundation Models)**: `#FFD93D` (Yellow)
- **Tier 3 (AI APIs)**: `#6BCF7F` (Green)
- **AWS Services**: Official AWS colors
- **Data Flow**: `#4ECDC4` (Teal arrows)

#### Icon Guidelines
- Use official AWS service icons
- Consistent sizing (64x64px for services)
- Clear labels for all components
- Logical flow direction (left-to-right, top-to-bottom)

## 📋 Current Diagram Inventory

### Architecture Diagrams
- `document-processing-architecture.png` - Intelligent document processing flow
- `ecommerce-recommendation-architecture.png` - E-commerce ML recommendation system
- `ai-assistant-architecture.png` - Enterprise AI assistant with multi-tier integration
- `healthcare-ai-architecture.png` - HIPAA-compliant healthcare AI platform
- `manufacturing-predictive-maintenance.png` - IoT + ML predictive maintenance

### Pattern Diagrams
- `event-driven-pattern.png` - Event-driven architecture sequence
- `batch-processing-pattern.png` - Batch processing pipeline flow
- `realtime-inference-pattern.png` - Real-time inference pattern

### Infrastructure Diagrams
- `enterprise-security-architecture.png` - Security and compliance architecture
- `cost-optimization-strategy.png` - Multi-tier cost optimization approach
- `aiml-cicd-pipeline.png` - CI/CD pipeline for AI/ML projects
- `monitoring-observability-stack.png` - Comprehensive monitoring setup

## 🔄 Updating Diagrams

### When to Update
- New AWS services added
- Architecture patterns change
- User feedback on clarity
- AWS service updates/rebranding

### Update Process
1. Edit diagram in Lucidchart/Draw.io
2. Export as PNG (1200px width recommended)
3. Replace file in `/assets/diagrams/`
4. Update any references in documentation
5. Test all image links

## 📐 Diagram Templates

### Architecture Template Structure
```
┌─────────────────────────────────────────┐
│              Title                      │
│         (Service Category)              │
└─────────────────────────────────────────┘

[User/Input] → [AWS Service 1] → [AWS Service 2] → [Output]
                     ↓
              [Supporting Services]
                     ↓
              [Monitoring/Logging]
```

### Flow Indicators
- **→** Data flow
- **↓** Process flow
- **⟷** Bidirectional
- **⚡** Real-time
- **📊** Batch processing
- **🔄** Feedback loop

## 🎯 Best Practices

### Visual Clarity
- Maximum 7-9 components per diagram
- Clear component grouping
- Consistent spacing and alignment
- Readable fonts (minimum 12pt)

### Technical Accuracy
- Accurate service connections
- Proper data flow direction
- Include key configuration details
- Show security boundaries

### Documentation Integration
- Each diagram has descriptive alt text
- Flow summary in blockquote
- Component list with descriptions
- Implementation code examples

## 🔗 External Resources

- [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/)
- [Lucidchart AWS Templates](https://lucidchart.com/pages/templates/aws-architecture)
- [Draw.io AWS Shapes](https://github.com/aws-samples/aws-icons-for-plantuml)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

---

💡 **Need help with diagrams?** Check our [Contributing Guide](../CONTRIBUTING.md) for design review process.