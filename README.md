# 🎯 AI Resume-Job Matcher

> Semantic matching system for resumes and job descriptions using fine-tuned BERT

[![Model on HF](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Model-yellow)](https://huggingface.co/ij98/resume-job-encoder2)
[![Python](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![AWS](https://img.shields.io/badge/AWS-SageMaker-orange)](https://aws.amazon.com/sagemaker/)

---

## 💡 What Is This?

Ever wondered how ATS (Applicant Tracking Systems) actually match resumes to jobs? I built one to find out!

This project uses AI to understand **semantic similarity** - not just keyword matching, but actual meaning.

**Example:**
```
Resume: "Python developer with ML experience"
Job: "Seeking Machine Learning Engineer"
→ Match Score: 86% ✅
```

**Why it's cool:**
- 🧠 Understands context (knows "ML Engineer" = "Machine Learning Developer")
- 📄 Works with real documents (PDF, DOCX, TXT)
- ☁️ Deployed to production on AWS SageMaker
- 🎓 Learned end-to-end ML deployment

---

## 🚀 Quick Start

### Try the Model (No Installation)

```python
from sentence_transformers import SentenceTransformer, util

# Load from Hugging Face
model = SentenceTransformer("ij98/resume-job-encoder2")

# Your texts
resume = "Experienced ML Engineer with PyTorch"
job = "Seeking Machine Learning Engineer with cloud experience"

# Get match score
emb1 = model.encode(resume)
emb2 = model.encode(job)
similarity = util.cos_sim(emb1, emb2).item()

print(f"Match: {similarity:.1%}")  # Output: Match: 85.8%
```

### Run the Notebooks

```bash
# 1. Install dependencies
pip install sentence-transformers transformers torch datasets PyPDF2 python-docx sagemaker boto3

# 2. Open in Jupyter or Google Colab
jupyter notebook ResumeMatcherScript.ipynb

# For AWS deployment
jupyter notebook ResumeMatcher.ipynb
```

---

## 📂 Project Files

```
resume-job-matcher/
├── ResumeMatcherScript.ipynb    # Training & local inference
├── ResumeMatcher.ipynb          # AWS SageMaker deployment
└── README.md                    # This file
```

---

## 🛠️ What I Built

### 1. Training Pipeline (`ResumeMatcherScript.ipynb`)
- Fine-tuned Sentence-BERT on resume-job pairs
- Document parsing (PDF/DOCX/TXT support)
- Training with contrastive learning
- Evaluation metrics

### 2. Production Deployment (`ResumeMatcher.ipynb`)
- Deployed to AWS SageMaker
- Real-time inference endpoint
- Published model to Hugging Face Hub
- Cloud-based architecture

---

## 📊 How It Works

```
Step 1: Extract Text from Documents
[Resume PDF] → Text Extraction → "Python developer with 5 years..."

Step 2: Convert to Embeddings
Text → BERT Encoder → [768-dimensional vector]

Step 3: Calculate Similarity
Resume Vector ──┐
                ├─→ Cosine Similarity → Match Score (0.0 to 1.0)
Job Vector ─────┘
```

---

## 💻 Usage Examples

### Match One Resume to Multiple Jobs

```python
# Load documents
resume_text = extract_text_from_file("my_resume.pdf")
job_texts = [
    extract_text_from_file("job1.pdf"),
    extract_text_from_file("job2.pdf"),
    extract_text_from_file("job3.pdf")
]

# Encode
model = SentenceTransformer("ij98/resume-job-encoder2")
resume_emb = model.encode(resume_text)
job_embs = model.encode(job_texts)

# Calculate similarities
from sentence_transformers import util
similarities = util.cos_sim(resume_emb, job_embs)[0]

# Show results
for i, score in enumerate(similarities):
    print(f"Job {i+1}: {score:.1%} match")
```

### AWS SageMaker Inference

```python
from sagemaker.predictor import Predictor

# Use deployed endpoint
predictor = Predictor(endpoint_name="your-endpoint")
result = predictor.predict({"inputs": "Your resume text here"})
```

---

## 🎓 What I Learned

### Technical Skills
- ✅ Fine-tuning BERT with Sentence Transformers
- ✅ AWS SageMaker deployment (first production ML deployment!)
- ✅ Document parsing (PDF/DOCX extraction)
- ✅ PyTorch & Hugging Face ecosystem
- ✅ End-to-end ML pipeline (training → deployment)

### Key Insights
- 💡 ATS systems are smarter than keyword matching
- 💡 They understand semantic similarity and context
- 💡 "ML Engineer" and "Data Scientist" are 85%+ similar
- 💡 Deployment is harder than training (learned the hard way!)

---

## 📈 Model Performance

| Metric | Value |
|--------|-------|
| Base Model | all-MiniLM-L6-v2 |
| Test MSE | 0.063 |
| Model Size | 90.9 MB |
| Training Time | ~10 minutes |

**Training Config:**
- Epochs: 3
- Batch Size: 16
- Loss Function: Cosine Similarity Loss

---

## 🔧 Tech Stack

- **ML Framework**: PyTorch, Transformers
- **Model**: Sentence-BERT (fine-tuned)
- **Cloud**: AWS SageMaker
- **Document Processing**: PyPDF2, python-docx
- **Model Hub**: Hugging Face

---

## 🌟 Features

- [x] Semantic similarity (understands context, not just keywords)
- [x] Document support (PDF, DOCX, TXT)
- [x] Production deployment (AWS SageMaker)
- [x] Open-source model (Hugging Face Hub)
- [x] Batch processing support
- [ ] Web interface (future improvement)
- [ ] Multi-language support (future)

---

## 📝 notebooks Overview

### ResumeMatcherScript.ipynb
**Purpose**: Train and test the model locally

**What it does:**
1. Creates sample resume/job documents
2. Loads documents from folders
3. Creates training pairs with labels
4. Fine-tunes BERT model
5. Evaluates performance
6. Tests inference locally

**Run in**: Google Colab or Jupyter

### ResumeMatcher.ipynb  
**Purpose**: Deploy model to AWS SageMaker

**What it does:**
1. Sets up AWS credentials
2. Loads pre-trained model
3. Deploys to SageMaker endpoint
4. Tests real-time inference
5. Shows how to delete endpoint (to avoid costs!)

**Run in**: AWS SageMaker Studio

---

## 🎯 Use Cases

### For Job Seekers
- Understand how your resume matches different roles
- Identify skill gaps
- Optimize resume for better matches

### For Recruiters
- Pre-screen candidates automatically
- Rank applicants by relevance
- Save time on manual resume review

### For Developers
- Learn NLP model deployment
- Understand semantic similarity
- Build custom matching systems

---

## 💡 Why This Project Matters

**The Problem**: Traditional ATS systems use keyword matching, which misses:
- Synonyms ("ML Engineer" vs "Machine Learning Developer")
- Related skills ("Python" in resume matches "Data Science" in job)
- Context and experience level

**The Solution**: Semantic understanding using BERT
- Captures meaning, not just words
- Understands relationships between terms
- More accurate matching

**Personal Impact**: 
- Demystified how ATS systems work
- Helped me write better resumes
- Learned production ML deployment

---

## 🚧 Current Limitations

- **Small training set**: Proof of concept (need more data)
- **English only**: No multi-language support yet
- **No OCR**: Scanned PDFs won't work
- **Context window**: Limited to 512 tokens

---

## 🔮 Future Improvements

- [ ] Expand training data (10K+ labeled pairs)
- [ ] Build web interface (Streamlit)
- [ ] Add skill extraction & highlighting
- [ ] Multi-language support
- [ ] OCR for scanned documents
- [ ] Industry-specific fine-tuning

---

## 📚 Resources & Links

- 🤗 **Model**: [ij98/resume-job-encoder2](https://huggingface.co/ij98/resume-job-encoder2)
- 📖 **Sentence-BERT**: [sbert.net](https://www.sbert.net/)
- ☁️ **AWS SageMaker**: [aws.amazon.com/sagemaker](https://aws.amazon.com/sagemaker/)

---

## 🙏 Acknowledgments

- [Sentence-Transformers](https://www.sbert.net/) for the framework
- [Hugging Face](https://huggingface.co/) for model hosting
- AWS SageMaker for deployment infrastructure

---

## 📬 Contact

- LinkedIn: [Your Profile](https://linkedin.com/in/yourprofile)
- Email: your.email@example.com

**Questions?** Open an issue or reach out!

---

## ⚠️ Important Notes

### AWS Costs
If you deploy to SageMaker:
- `ml.t2.medium` costs ~$0.05/hour (~$36/month)
- **Always delete endpoint when done!**

```python
# Delete endpoint to stop charges
import boto3
sm = boto3.client('sagemaker')
sm.delete_endpoint(EndpointName="your-endpoint")
```

### Privacy
- Never commit real resumes or job descriptions
- Use sample data for testing only
- Keep credentials out of notebooks

---

## 📄 License

MIT License - Free to use for learning and building!

---

**⭐ Star this repo if you found it helpful!**

*Built with curiosity and a desire to understand how things actually work* 🚀
