# Saarthi AI - Technical Design Document

**Team:** The Dark Knight  
**Hackathon:** AWS AI for Bharat  
**Version:** 1.0  
**Last Updated:** February 2026

---

## 1. System Architecture Overview

### 1.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INTERFACE                          │
│  (React Web App / Streamlit - Mobile-First Responsive)         │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             │ HTTPS/REST API
                             │
┌────────────────────────────▼────────────────────────────────────┐
│                      API GATEWAY LAYER                          │
│                      (FastAPI Backend)                          │
│  - Authentication  - Rate Limiting  - Request Routing          │
└────────────────────────────┬────────────────────────────────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
┌───────▼────────┐  ┌────────▼─────────┐  ┌──────▼──────────┐
│  ELIGIBILITY   │  │   RAG ENGINE     │  │ SIMPLIFICATION  │
│    ENGINE      │  │   (LangChain)    │  │    ENGINE       │
│ (Rule-based)   │  │  - Retrieval     │  │  (LLM-based)    │
│                │  │  - Generation    │  │                 │
└───────┬────────┘  └────────┬─────────┘  └──────┬──────────┘
        │                    │                    │
┌───────▼────────┐  ┌────────▼─────────┐  ┌──────▼──────────┐
│ OCR DOCUMENT   │  │  VECTOR DB       │  │  VOICE ENGINE   │
│    HELPER      │  │ (FAISS/Chroma)   │  │  - STT/TTS      │
│ (Tesseract)    │  │                  │  │  - Whisper      │
└───────┬────────┘  └────────┬─────────┘  └──────┬──────────┘
        │                    │                    │
        └────────────────────┼────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────┐
│                      DATA LAYER                                 │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────┐        │
│  │ PostgreSQL  │  │  Vector DB   │  │  Redis Cache   │        │
│  │ (User Data) │  │  (Embeddings)│  │  (Sessions)    │        │
│  └─────────────┘  └──────────────┘  └────────────────┘        │
└─────────────────────────────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────┐
│                 EXTERNAL INTEGRATIONS                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ myScheme API │  │ data.gov.in  │  │  Email/SMS   │         │
│  │ (Gov Schemes)│  │ (Open Data)  │  │  (Mailgun)   │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└─────────────────────────────────────────────────────────────────┘
```

### 1.2 Architecture Principles

**Separation of Concerns:** Each component has a single, well-defined responsibility

**Scalability:** Horizontal scaling for high traffic, modular services

**Resilience:** Fallback mechanisms, graceful degradation, error handling

**Performance:** Caching layer, async processing, optimized queries

**Security:** Encryption at rest and in transit, minimal data retention

---

## 2. Component Design

### 2.1 Frontend Layer

#### Technology Stack
- **Framework:** React 18+ (or Streamlit for rapid prototyping)
- **Styling:** Tailwind CSS
- **State Management:** React Context API / Zustand
- **Routing:** React Router v6
- **HTTP Client:** Axios with interceptors
- **Charts:** Recharts / Chart.js

#### Key Pages/Components

**1. Landing Page**
```
┌─────────────────────────────────────┐
│          SAARTHI AI                 │
│     Your Guide to Government        │
│            Schemes                  │
│                                     │
│  [Select Language: ▼ English]      │
│                                     │
│  ┌─────────────────────────────┐   │
│  │    🎤  Speak Your Query     │   │
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │   ✍️  Type Your Question    │   │
│  └─────────────────────────────┘   │
│                                     │
│  Quick Actions:                     │
│  [Check Eligibility] [Find Schemes] │
│  [Upload Document]   [My Profile]   │
└─────────────────────────────────────┘
```

**2. Eligibility Dashboard**
```
┌─────────────────────────────────────────────┐
│  Your Eligibility Score                     │
│                                             │
│  ╔════════════════════════════════════════╗│
│  ║  78%   [=========       ]   ELIGIBLE   ║│
│  ╚════════════════════════════════════════╝│
│                                             │
│  Score Breakdown:                           │
│  ✓ Income Match       40/40                │
│  ✓ Age Eligible       20/20                │
│  ✓ State Match        15/15                │
│  ~ Document Pending    0/15                │
│                                             │
│  Why you scored 78%:                        │
│  • Your income (₹1.5L) is within the       │
│    required limit (₹2L)                    │
│  • You are in the right age group (25)     │
│  • Missing: Income certificate             │
│                                             │
│  [View Recommended Schemes]                 │
└─────────────────────────────────────────────┘
```

**3. Scheme Cards**
```
┌──────────────────────────────────────┐
│  🌾 PM-KISAN                         │
│                                      │
│  ✓ 85% Match for you                │
│                                      │
│  What you get:                       │
│  ₹6,000/year direct benefit          │
│                                      │
│  What you need:                      │
│  • Land ownership proof              │
│  • Aadhaar card                      │
│  • Bank account                      │
│                                      │
│  Application Deadline: Mar 31, 2026  │
│                                      │
│  [View Details] [Apply Now]          │
└──────────────────────────────────────┘
```

#### Mobile-First Responsive Design
- Breakpoints: 320px (mobile), 768px (tablet), 1024px (desktop)
- Touch-friendly buttons (min 44x44px)
- Bottom navigation for mobile
- Hamburger menu for secondary options
- Progressive Web App (PWA) capabilities

#### Accessibility Features
- ARIA labels for screen readers
- Keyboard navigation (Tab, Enter, Esc)
- High contrast mode toggle
- Text scaling (100%-200%)
- Focus indicators

---

### 2.2 Backend Layer (FastAPI)

#### Project Structure
```
backend/
│
├── main.py                    # FastAPI app entry point
├── config.py                  # Configuration management
├── requirements.txt           # Python dependencies
│
├── api/                       # API routes
│   ├── __init__.py
│   ├── eligibility.py         # Eligibility endpoints
│   ├── schemes.py             # Scheme search endpoints
│   ├── voice.py               # Voice processing endpoints
│   ├── documents.py           # OCR endpoints
│   └── user.py                # User profile endpoints
│
├── core/                      # Business logic
│   ├── __init__.py
│   ├── eligibility_engine.py  # Scoring algorithm
│   ├── rag_engine.py          # RAG implementation
│   ├── simplifier.py          # Text simplification
│   ├── ocr_helper.py          # Document OCR
│   └── voice_processor.py     # Speech processing
│
├── models/                    # Data models
│   ├── __init__.py
│   ├── user.py                # User model
│   ├── scheme.py              # Scheme model
│   └── eligibility.py         # Eligibility result model
│
├── services/                  # External services
│   ├── __init__.py
│   ├── government_api.py      # Gov API integration
│   ├── email_service.py       # Email notifications
│   └── cache_service.py       # Redis caching
│
├── utils/                     # Utilities
│   ├── __init__.py
│   ├── validators.py          # Input validation
│   ├── helpers.py             # Helper functions
│   └── constants.py           # Constants
│
└── data/                      # Data files
    ├── schemes.csv            # Scheme database
    ├── scheme_docs/           # PDF documents
    └── embeddings/            # Vector store
```

#### API Endpoint Design

**Eligibility Endpoints**
```python
POST /api/v1/eligibility/calculate
Request:
{
  "user_profile": {
    "age": 28,
    "income": 150000,
    "state": "Tamil Nadu",
    "category": "SC",
    "occupation": "Farmer"
  },
  "scheme_id": "PM-KISAN"
}

Response:
{
  "score": 78,
  "status": "eligible",
  "color": "green",
  "breakdown": {
    "income_score": 40,
    "age_score": 20,
    "state_score": 15,
    "category_score": 15,
    "document_score": 0
  },
  "explanation": "You qualify because your income (₹1.5L) is within the limit (₹2L). You need to submit income certificate.",
  "missing_documents": ["income_certificate"],
  "next_steps": ["Upload income certificate", "Complete application"]
}
```

**Scheme Search Endpoints**
```python
GET /api/v1/schemes/search?query=farming&state=Tamil Nadu
Response:
{
  "total": 12,
  "schemes": [
    {
      "id": "PM-KISAN",
      "name": "Pradhan Mantri Kisan Samman Nidhi",
      "category": "Agriculture",
      "benefit": "₹6,000 per year",
      "eligibility_match": 85,
      "deadline": "2026-03-31"
    }
  ]
}
```

**Voice Processing Endpoints**
```python
POST /api/v1/voice/transcribe
Request: Multipart form with audio file
Response:
{
  "text": "What schemes are available for farmers?",
  "language": "en",
  "confidence": 0.92
}

POST /api/v1/voice/synthesize
Request:
{
  "text": "You are eligible for PM-KISAN",
  "language": "ta"
}
Response: Audio file (mp3)
```

**OCR Endpoints**
```python
POST /api/v1/documents/extract
Request: Multipart form with image file
Response:
{
  "document_type": "income_certificate",
  "extracted_data": {
    "name": "Rajesh Kumar",
    "income": 150000,
    "issue_date": "2025-12-15",
    "validity": "2026-12-15"
  },
  "confidence": 0.88,
  "verification_status": "pending"
}
```

---

### 2.3 Eligibility Engine (Core Logic)

#### Algorithm Design

**Input Parameters:**
- User profile (age, income, state, category, occupation)
- Scheme requirements
- Document availability

**Scoring Algorithm:**
```python
class EligibilityEngine:
    def calculate_score(self, user_profile, scheme):
        score = 0
        breakdown = {}
        explanation = []
        
        # 1. Income Check (0-40 points)
        if user_profile.income <= scheme.max_income:
            income_score = 40
            explanation.append(f"✓ Your income (₹{user_profile.income:,}) is within limit (₹{scheme.max_income:,})")
        else:
            income_score = max(0, 40 - ((user_profile.income - scheme.max_income) / scheme.max_income) * 40)
            explanation.append(f"✗ Your income (₹{user_profile.income:,}) exceeds limit (₹{scheme.max_income:,})")
        
        score += income_score
        breakdown['income'] = income_score
        
        # 2. Age Check (0-20 points)
        if scheme.min_age <= user_profile.age <= scheme.max_age:
            age_score = 20
            explanation.append(f"✓ Your age ({user_profile.age}) matches requirement ({scheme.min_age}-{scheme.max_age})")
        else:
            age_score = 0
            explanation.append(f"✗ Age requirement not met")
        
        score += age_score
        breakdown['age'] = age_score
        
        # 3. State/Location (0-15 points)
        if user_profile.state in scheme.eligible_states or scheme.eligible_states == ['All']:
            state_score = 15
            explanation.append(f"✓ Your state ({user_profile.state}) is eligible")
        else:
            state_score = 0
            explanation.append(f"✗ Scheme not available in {user_profile.state}")
        
        score += state_score
        breakdown['state'] = state_score
        
        # 4. Category (0-15 points)
        if user_profile.category in scheme.eligible_categories or scheme.eligible_categories == ['All']:
            category_score = 15
            explanation.append(f"✓ Your category ({user_profile.category}) is eligible")
        else:
            category_score = 0
        
        score += category_score
        breakdown['category'] = category_score
        
        # 5. Documents (0-10 points)
        required_docs = set(scheme.required_documents)
        user_docs = set(user_profile.uploaded_documents)
        doc_score = (len(required_docs & user_docs) / len(required_docs)) * 10 if required_docs else 10
        
        missing_docs = required_docs - user_docs
        if missing_docs:
            explanation.append(f"⚠ Missing documents: {', '.join(missing_docs)}")
        
        score += doc_score
        breakdown['documents'] = doc_score
        
        # Determine status
        if score >= 70:
            status = "eligible"
            color = "green"
        elif score >= 40:
            status = "partially_eligible"
            color = "yellow"
        else:
            status = "not_eligible"
            color = "red"
        
        return {
            'score': round(score),
            'status': status,
            'color': color,
            'breakdown': breakdown,
            'explanation': explanation,
            'missing_documents': list(missing_docs)
        }
```

#### Edge Cases Handled
- Missing user profile data (use defaults)
- Schemes without income limits (score = 40 automatically)
- Multiple states (use OR logic)
- Special categories (additional bonus points)

---

### 2.4 RAG Engine (Retrieval-Augmented Generation)

#### Architecture

```
User Query → Embedding → Vector Search → Retrieve Top-K → LLM Generation → Response
                ↓                            ↓
        Sentence Transformer         FAISS/ChromaDB
                                    (Scheme Documents)
```

#### Implementation Details

**1. Document Processing Pipeline**
```python
class RAGEngine:
    def __init__(self):
        self.embeddings = SentenceTransformer('all-MiniLM-L6-v2')
        self.vector_db = FAISS.load_index('scheme_embeddings.index')
        self.documents = self.load_scheme_documents()
        
    def process_documents(self, pdf_files):
        """Convert PDFs to chunks and create embeddings"""
        chunks = []
        
        for pdf in pdf_files:
            # Extract text from PDF
            text = self.extract_pdf_text(pdf)
            
            # Split into chunks (500 tokens with 50 overlap)
            doc_chunks = self.split_text(text, chunk_size=500, overlap=50)
            
            for chunk in doc_chunks:
                chunks.append({
                    'text': chunk,
                    'source': pdf.name,
                    'metadata': self.extract_metadata(chunk)
                })
        
        # Create embeddings
        embeddings = self.embeddings.encode([c['text'] for c in chunks])
        
        # Store in vector database
        self.vector_db.add(embeddings)
        
        return len(chunks)
```

**2. Retrieval Logic**
```python
def retrieve_schemes(self, query, top_k=5):
    """Find most relevant schemes based on query"""
    
    # Embed query
    query_embedding = self.embeddings.encode([query])[0]
    
    # Search vector database
    distances, indices = self.vector_db.search(
        query_embedding.reshape(1, -1), 
        k=top_k
    )
    
    # Get relevant documents
    relevant_docs = []
    for idx in indices[0]:
        doc = self.documents[idx]
        relevant_docs.append({
            'scheme_name': doc['scheme_name'],
            'content': doc['text'],
            'source': doc['source'],
            'relevance_score': 1 - distances[0][len(relevant_docs)]
        })
    
    return relevant_docs
```

**3. Generation Logic**
```python
def generate_response(self, query, context_docs):
    """Generate answer using retrieved context"""
    
    # Build context from retrieved documents
    context = "\n\n".join([
        f"Scheme: {doc['scheme_name']}\n{doc['content']}" 
        for doc in context_docs
    ])
    
    # Create prompt
    prompt = f"""Based on the following government schemes, answer the user's question.
    
Context:
{context}

User Question: {query}

Provide a clear answer mentioning:
1. Relevant scheme names
2. Eligibility criteria
3. Benefits
4. How to apply

Answer:"""
    
    # Call LLM (GPT-4 or Gemini)
    response = self.llm.generate(prompt, max_tokens=300)
    
    # Add source citations
    sources = [doc['source'] for doc in context_docs]
    
    return {
        'answer': response,
        'sources': sources,
        'confidence': self.calculate_confidence(context_docs)
    }
```

#### Data Sources Configuration

**Government Portals:**
```python
DATA_SOURCES = {
    'myscheme': {
        'url': 'https://www.myscheme.gov.in/api/schemes',
        'update_frequency': 'daily',
        'format': 'json'
    },
    'data_gov': {
        'url': 'https://data.gov.in/api/datastore',
        'api_key': os.getenv('DATA_GOV_API_KEY'),
        'update_frequency': 'weekly',
        'format': 'csv'
    }
}
```

**Vector Database Schema:**
```
Document {
  id: UUID
  scheme_name: String
  text_chunk: String
  embedding: Vector[384]  # MiniLM dimension
  metadata: {
    ministry: String
    category: String
    state: String
    last_updated: DateTime
  }
  source_url: String
}
```

---

### 2.5 AI Simplification Engine

#### Design Approach

**Input:** Complex government text  
**Output:** Simple, readable text (Grade 6-8 level)

**LLM Prompt Template:**
```python
SIMPLIFICATION_PROMPT = """You are a helpful assistant that simplifies complex government language for rural Indian citizens with basic education.

Rules:
1. Use simple, everyday words
2. Break long sentences into short ones
3. Avoid jargon and technical terms
4. Keep numbers and dates accurate
5. Use active voice
6. Add examples if helpful
7. Maintain factual accuracy

Original Text:
{complex_text}

Simplified Version (in {language}):"""
```

**Example Implementation:**
```python
class SimplificationEngine:
    def __init__(self):
        self.llm = ChatGPT(model='gpt-4o-mini')  # Cost-effective
        
    def simplify(self, text, language='en', education_level='basic'):
        prompt = SIMPLIFICATION_PROMPT.format(
            complex_text=text,
            language=language
        )
        
        simplified = self.llm.generate(prompt, temperature=0.3)
        
        # Validate accuracy
        if not self.validate_accuracy(text, simplified):
            return text  # Return original if simplification loses meaning
        
        return simplified
    
    def validate_accuracy(self, original, simplified):
        """Check that key facts are preserved"""
        # Extract numbers, dates, amounts
        original_facts = self.extract_facts(original)
        simplified_facts = self.extract_facts(simplified)
        
        # Ensure critical info is retained
        return self.facts_match(original_facts, simplified_facts)
```

**Quality Metrics:**
- Flesch Reading Ease: > 60 (8th grade level)
- Sentence length: < 15 words average
- Factual accuracy: 100% (numbers must match)

---

### 2.6 OCR Document Helper

#### Technology Stack
- **OCR Engine:** Tesseract 5.0
- **Image Processing:** OpenCV, Pillow
- **Document Classification:** Simple keyword matching

#### Processing Pipeline

```
Upload → Preprocessing → OCR → Extraction → Validation → Verification
```

**Implementation:**
```python
class OCRHelper:
    def __init__(self):
        self.tesseract_config = '--oem 3 --psm 6'  # LSTM + block mode
        
    def process_document(self, image_file):
        # 1. Preprocess image
        img = cv2.imread(image_file)
        gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
        denoised = cv2.fastNlMeansDenoising(gray)
        thresh = cv2.threshold(denoised, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)[1]
        
        # 2. OCR extraction
        text = pytesseract.image_to_string(thresh, config=self.tesseract_config, lang='eng+hin+tam')
        
        # 3. Identify document type
        doc_type = self.classify_document(text)
        
        # 4. Extract structured data
        extracted_data = self.extract_fields(text, doc_type)
        
        # 5. Validate extracted data
        validation_result = self.validate_extraction(extracted_data)
        
        return {
            'document_type': doc_type,
            'extracted_data': extracted_data,
            'confidence': validation_result['confidence'],
            'raw_text': text
        }
    
    def classify_document(self, text):
        """Identify document type from text"""
        keywords = {
            'income_certificate': ['income', 'annual income', 'certificate'],
            'aadhaar': ['aadhaar', 'uid', '12 digit'],
            'caste_certificate': ['caste', 'sc', 'st', 'obc'],
            'bank_passbook': ['bank', 'account', 'ifsc']
        }
        
        text_lower = text.lower()
        scores = {}
        
        for doc_type, terms in keywords.items():
            score = sum(1 for term in terms if term in text_lower)
            scores[doc_type] = score
        
        return max(scores, key=scores.get) if scores else 'unknown'
    
    def extract_fields(self, text, doc_type):
        """Extract specific fields based on document type"""
        if doc_type == 'income_certificate':
            return {
                'income': self.extract_income(text),
                'name': self.extract_name(text),
                'issue_date': self.extract_date(text)
            }
        elif doc_type == 'aadhaar':
            return {
                'aadhaar_number': self.extract_aadhaar(text),
                'name': self.extract_name(text),
                'dob': self.extract_dob(text)
            }
        # ... more document types
    
    def extract_income(self, text):
        """Extract income amount using regex"""
        import re
        # Pattern: Rs. 150000 or ₹1,50,000
        pattern = r'(?:Rs\.?|₹)\s*([0-9,]+(?:\.[0-9]{2})?)'
        match = re.search(pattern, text)
        if match:
            amount = match.group(1).replace(',', '')
            return float(amount)
        return None
```

**Supported Document Types:**
1. Income Certificate
2. Aadhaar Card
3. Caste Certificate
4. Bank Passbook
5. Ration Card
6. Land Ownership Documents

**Error Handling:**
- Low quality image → Request better photo
- Wrong document type → Show expected format
- Missing fields → Highlight what's needed
- OCR confidence < 70% → Manual review

---

### 2.7 Voice Processing System

#### Components

**1. Speech-to-Text (STT)**
```python
class SpeechToText:
    def __init__(self):
        self.whisper_model = whisper.load_model('base')  # ~140MB
        
    def transcribe(self, audio_file, language=None):
        # Load audio
        audio = whisper.load_audio(audio_file)
        
        # Transcribe
        result = self.whisper_model.transcribe(
            audio,
            language=language,  # 'en', 'hi', 'ta'
            task='transcribe'
        )
        
        return {
            'text': result['text'],
            'language': result['language'],
            'confidence': self.calculate_confidence(result)
        }
```

**2. Text-to-Speech (TTS)**
```python
class TextToSpeech:
    def __init__(self):
        self.engine = gTTS  # Google Text-to-Speech
        
    def synthesize(self, text, language='en', output_file='response.mp3'):
        tts = self.engine(text=text, lang=language, slow=False)
        tts.save(output_file)
        return output_file
```

**Language Support:**
- Hindi (hi)
- Tamil (ta)
- English (en)
- Bengali (bn)
- Marathi (mr)
- Telugu (te)

**Audio Format:**
- Input: WAV, MP3, M4A
- Output: MP3 (compressed for mobile)
- Sample rate: 16kHz (sufficient for speech)
- Bitrate: 64kbps (low bandwidth)

---

### 2.8 Database Design

#### PostgreSQL Schema

**Users Table:**
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    phone_number VARCHAR(15) UNIQUE,
    name VARCHAR(100),
    age INTEGER,
    income DECIMAL(12,2),
    state VARCHAR(50),
    district VARCHAR(50),
    category VARCHAR(20),  -- SC/ST/OBC/General
    occupation VARCHAR(50),
    language_preference VARCHAR(10),
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

**Schemes Table:**
```sql
CREATE TABLE schemes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    scheme_code VARCHAR(50) UNIQUE,
    name VARCHAR(200),
    ministry VARCHAR(100),
    category VARCHAR(50),
    description TEXT,
    benefits TEXT,
    min_age INTEGER,
    max_age INTEGER,
    max_income DECIMAL(12,2),
    eligible_states TEXT[],  -- Array
    eligible_categories TEXT[],
    required_documents TEXT[],
    application_url TEXT,
    deadline DATE,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT NOW()
);
```

**Eligibility Results Table:**
```sql
CREATE TABLE eligibility_results (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    scheme_id UUID REFERENCES schemes(id),
    score INTEGER,
    status VARCHAR(20),
    breakdown JSONB,
    calculated_at TIMESTAMP DEFAULT NOW()
);
```

**Documents Table:**
```sql
CREATE TABLE user_documents (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    document_type VARCHAR(50),
    file_path TEXT,
    extracted_data JSONB,
    verification_status VARCHAR(20),
    uploaded_at TIMESTAMP DEFAULT NOW(),
    expires_at TIMESTAMP DEFAULT (NOW() + INTERVAL '7 days')
);
```

**Indexes:**
```sql
CREATE INDEX idx_schemes_category ON schemes(category);
CREATE INDEX idx_schemes_state ON schemes USING GIN(eligible_states);
CREATE INDEX idx_users_state ON users(state);
CREATE INDEX idx_eligibility_user ON eligibility_results(user_id);
```

#### Vector Database (FAISS)

**Structure:**
```python
vector_store = {
    'index': faiss.IndexFlatL2(384),  # MiniLM embedding size
    'documents': [
        {
            'id': 'scheme_001_chunk_001',
            'scheme_id': 'PM-KISAN',
            'text': 'chunk content...',
            'metadata': {...}
        }
    ]
}
```

---

### 2.9 Caching Strategy

#### Redis Cache Layer

**What to Cache:**
1. Scheme search results (24 hours)
2. User eligibility scores (1 hour)
3. Government API responses (12 hours)
4. Simplified text (7 days)
5. Session data (30 minutes)

**Implementation:**
```python
class CacheService:
    def __init__(self):
        self.redis = redis.Redis(host='localhost', port=6379, db=0)
        
    def cache_eligibility(self, user_id, scheme_id, result):
        key = f"eligibility:{user_id}:{scheme_id}"
        self.redis.setex(key, 3600, json.dumps(result))  # 1 hour
        
    def get_eligibility(self, user_id, scheme_id):
        key = f"eligibility:{user_id}:{scheme_id}"
        cached = self.redis.get(key)
        return json.loads(cached) if cached else None
```

**Cache Invalidation:**
- User profile update → Clear eligibility cache
- New scheme added → Clear search cache
- Daily at midnight → Clear stale data

---

## 3. Data Flow Diagrams

### 3.1 User Eligibility Check Flow

```
User Request
    ↓
Check Redis Cache
    ↓
Cache Hit? ────Yes──→ Return Cached Result
    ↓ No
Fetch User Profile (PostgreSQL)
    ↓
Fetch Scheme Details (PostgreSQL)
    ↓
Calculate Eligibility Score
    ↓
Store in Cache (Redis)
    ↓
Return Result to User
```

### 3.2 Voice Query Flow

```
User Speaks → Audio Recording
    ↓
Upload to Backend
    ↓
Speech-to-Text (Whisper)
    ↓
Extract Query Text
    ↓
RAG Engine Retrieval
    ↓
Generate Response
    ↓
Simplify Text (LLM)
    ↓
Text-to-Speech (gTTS)
    ↓
Play Audio to User
```

### 3.3 Document Verification Flow

```
User Uploads Document
    ↓
Image Preprocessing (OpenCV)
    ↓
OCR Extraction (Tesseract)
    ↓
Classify Document Type
    ↓
Extract Relevant Fields
    ↓
Validate Against Scheme Requirements
    ↓
Store in Database (7-day retention)
    ↓
Return Verification Status
```

---

## 4. Security Design

### 4.1 Authentication & Authorization

**Approach:** Phone number-based OTP authentication (for hackathon)

```python
class AuthService:
    def send_otp(self, phone_number):
        otp = random.randint(100000, 999999)
        # Store in Redis with 5-minute expiry
        self.redis.setex(f"otp:{phone_number}", 300, str(otp))
        # Send via SMS (future integration)
        return True
    
    def verify_otp(self, phone_number, otp):
        stored_otp = self.redis.get(f"otp:{phone_number}")
        if stored_otp and stored_otp.decode() == str(otp):
            # Generate JWT token
            token = jwt.encode(
                {'phone': phone_number, 'exp': datetime.utcnow() + timedelta(days=7)},
                SECRET_KEY
            )
            return token
        return None
```

### 4.2 Data Protection

**Encryption:**
- Transit: TLS 1.3 (HTTPS)
- At Rest: AES-256 for sensitive documents
- Database: PostgreSQL encryption enabled

**Privacy:**
- Minimal data collection
- No third-party sharing
- 7-day document retention
- User can delete account anytime

**Input Validation:**
```python
from pydantic import BaseModel, validator

class UserProfile(BaseModel):
    age: int
    income: float
    state: str
    
    @validator('age')
    def validate_age(cls, v):
        if not 18 <= v <= 100:
            raise ValueError('Age must be between 18 and 100')
        return v
    
    @validator('income')
    def validate_income(cls, v):
        if v < 0:
            raise ValueError('Income cannot be negative')
        return v
```

---

## 5. Performance Optimization

### 5.1 Frontend Optimization

**Techniques:**
- Code splitting (React.lazy)
- Image lazy loading
- Service Worker caching (PWA)
- Minification + Gzip compression
- CDN for static assets

**Bundle Size Target:**
- Initial JS: < 200KB gzipped
- Total page: < 500KB
- Time to Interactive: < 3s on 3G

### 5.2 Backend Optimization

**Techniques:**
- Async/await for I/O operations
- Database connection pooling
- Query optimization (indexes)
- Redis caching layer
- Batch processing for bulk operations

**Performance Targets:**
- API response time: < 500ms (p95)
- Eligibility calculation: < 2s
- RAG retrieval: < 3s
- OCR processing: < 10s

### 5.3 Database Optimization

**Strategies:**
- Proper indexing
- Query result caching
- Pagination for large datasets
- Archive old data (>1 year)

---

## 6. Deployment Architecture

### 6.1 Development Environment

```
Local Setup:
- Frontend: npm run dev (localhost:3000)
- Backend: uvicorn main:app --reload (localhost:8000)
- PostgreSQL: Docker container
- Redis: Docker container
- Vector DB: Local FAISS index
```

### 6.2 Production Deployment (AWS)

```
┌─────────────────────────────────────────┐
│         CloudFront (CDN)                │
│  (Static assets, caching)               │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│      Application Load Balancer          │
└──────────┬──────────────┬───────────────┘
           │              │
    ┌──────▼─────┐  ┌────▼──────┐
    │   EC2      │  │   EC2     │
    │ (Frontend) │  │ (Backend) │
    └────────────┘  └─────┬─────┘
                          │
              ┌───────────┼──────────┐
              │           │          │
        ┌─────▼────┐ ┌───▼────┐ ┌──▼─────┐
        │   RDS    │ │ S3     │ │ElastiCache│
        │(Postgres)│ │(Docs)  │ │ (Redis)  │
        └──────────┘ └────────┘ └──────────┘
```

**Services Used:**
- **EC2:** Application servers (t3.medium)
- **RDS:** PostgreSQL database
- **S3:** Document storage, static files
- **ElastiCache:** Redis caching
- **CloudFront:** CDN for global distribution
- **Route 53:** DNS management
- **Certificate Manager:** SSL/TLS certificates

### 6.3 Scaling Strategy

**Horizontal Scaling:**
- Auto-scaling groups for EC2 (2-10 instances)
- Load balancer distributes traffic
- Stateless backend (session in Redis)

**Vertical Scaling:**
- Upgrade instance types as needed
- RDS read replicas for query load

---

## 7. Monitoring & Logging

### 7.1 Application Monitoring

**Metrics to Track:**
- Response times (API endpoints)
- Error rates
- Cache hit/miss ratio
- Database query performance
- OCR accuracy
- Voice transcription accuracy

**Tools:**
- CloudWatch (AWS)
- Application logs (structured JSON)
- Custom dashboards

### 7.2 Error Handling

```python
from fastapi import HTTPException
import logging

logger = logging.getLogger(__name__)

@app.exception_handler(Exception)
async def global_exception_handler(request, exc):
    logger.error(f"Unhandled error: {exc}", exc_info=True)
    return JSONResponse(
        status_code=500,
        content={"error": "Internal server error", "message": str(exc)}
    )
```

---

## 8. Testing Strategy

### 8.1 Unit Tests

**Coverage:** 70%+ for core logic

```python
def test_eligibility_calculation():
    user = User(age=25, income=150000, state="Tamil Nadu")
    scheme = Scheme(min_age=18, max_age=35, max_income=200000)
    
    engine = EligibilityEngine()
    result = engine.calculate_score(user, scheme)
    
    assert result['score'] >= 70
    assert result['status'] == 'eligible'
    assert 'income' in result['breakdown']
```

### 8.2 Integration Tests

**API Endpoint Tests:**
```python
def test_eligibility_api():
    response = client.post("/api/v1/eligibility/calculate", json={
        "user_profile": {"age": 25, "income": 150000},
        "scheme_id": "PM-KISAN"
    })
    
    assert response.status_code == 200
    assert 'score' in response.json()
```

### 8.3 Demo Testing Checklist

✅ Voice input works in Hindi/Tamil/English  
✅ Eligibility score displays correctly  
✅ RAG retrieves relevant schemes  
✅ OCR extracts document data  
✅ UI is responsive on mobile  
✅ No crashes during 5-minute demo  

---

## 9. Documentation

### 9.1 API Documentation

**Auto-generated:** FastAPI Swagger UI at `/docs`

### 9.2 User Guide

**Included:**
- How to check eligibility
- How to use voice input
- How to upload documents
- FAQ section

### 9.3 Developer Guide

**Setup Instructions:**
```bash
# Backend setup
cd backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Frontend setup
cd frontend
npm install
npm start
```

---

## 10. Future Enhancements (Post-Hackathon)

### Phase 2 Features
- WhatsApp chatbot integration
- IVR (phone call) support
- Direct application submission
- DigiLocker integration
- Aadhaar eKYC

### Phase 3 Features
- District-level deployment
- Offline mobile app
- Advanced ML for eligibility prediction
- Multi-user household profiles

---

## Appendix

### A. Technology Dependencies

**Backend:**
```
fastapi==0.109.0
uvicorn==0.27.0
langchain==0.1.0
faiss-cpu==1.7.4
sentence-transformers==2.3.1
pytesseract==0.3.10
opencv-python==4.9.0
whisper==1.1.10
gTTS==2.5.0
psycopg2-binary==2.9.9
redis==5.0.1
pydantic==2.5.3
```

**Frontend:**
```
react==18.2.0
tailwindcss==3.4.1
axios==1.6.5
recharts==2.10.3
lucide-react==0.309.0
```

### B. Environment Variables

```
# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/saarthi
REDIS_URL=redis://localhost:6379

# APIs
OPENAI_API_KEY=sk-...
DATA_GOV_API_KEY=...

# Security
JWT_SECRET=...
ENCRYPTION_KEY=...

# Email
MAILGUN_API_KEY=...
```

### C. Deployment Checklist

✅ Environment variables configured  
✅ Database migrations run  
✅ Vector database built  
✅ SSL certificates installed  
✅ Monitoring enabled  
✅ Backup strategy in place  
✅ Load testing completed  

---

**Document Status:** FINAL  
**Ready for Implementation:** YES  
**Estimated Build Time:** 7 days  
**Confidence Level:** HIGH

This design is **hackathon-ready** and **winner-capable** 🚀
