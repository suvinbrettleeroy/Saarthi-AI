# Saarthi AI - Requirements Specification

**Team:** The Dark Knight  
**Hackathon:** AWS AI for Bharat  
**Version:** 1.0  
**Last Updated:** February 2026

---

## 1. Executive Summary

Saarthi AI is an intelligent government scheme discovery and eligibility platform designed to bridge the information gap between India's diverse population and thousands of available government schemes. The name "Saarthi" (guide) reflects our mission: to serve as a digital companion that simplifies complex government processes through AI-powered assistance.

### Problem Statement
Despite thousands of welfare schemes existing across central and state governments, eligible citizens rarely benefit because:
- Information is scattered across multiple portals
- Official language is complex and bureaucratic
- Eligibility criteria are confusing
- Documentation requirements are unclear
- Language barriers prevent access

### Our Solution
An AI-powered platform that:
- Calculates personalized eligibility scores (0-100%)
- Explains eligibility in simple, regional language
- Retrieves real-time scheme data using RAG
- Verifies documents using OCR
- Supports multilingual voice interaction
- Sends timely notifications

---

## 2. Target Audience

### Primary Users
1. **Rural Youth** (18-35 years)
   - Looking for skill development programs
   - Seeking employment schemes
   - Education scholarships

2. **Small Farmers** (25-60 years)
   - Agricultural subsidies
   - Crop insurance schemes
   - Equipment financing

3. **Women Entrepreneurs** (25-50 years)
   - MUDRA loans
   - Self-employment schemes
   - Skill training programs

4. **First-time Job Seekers** (18-30 years)
   - Employment schemes
   - Apprenticeship programs
   - Skill certification

### Secondary Users
- NGO workers helping communities
- District officials promoting schemes
- CSR teams implementing welfare programs

---

## 3. Functional Requirements

### 3.1 Core Features (Must-Have)

#### F1: Multilingual Voice Assistant
**Priority:** HIGH  
**Description:** Voice-enabled interface supporting Hindi, Tamil, English, and other Indian languages

**Requirements:**
- Speech-to-text conversion with 90%+ accuracy
- Support for code-mixed input (Hinglish, Tanglish)
- Text-to-speech output in user's preferred language
- Fallback to text input if voice fails
- Voice commands: "Check eligibility", "Find schemes", "Upload document"

**Acceptance Criteria:**
- User can speak query in Hindi/Tamil/English
- System responds within 3 seconds
- Audio quality is clear on 3G connections
- Works on mobile browsers

---

#### F2: AI-Powered Eligibility Engine
**Priority:** HIGH  
**Description:** Intelligent scoring system that calculates scheme eligibility based on user profile

**Requirements:**
- Score calculation (0-100%) based on multiple parameters
- Color-coded indicators (Green: Eligible, Yellow: Partially, Red: Not Eligible)
- Detailed explanation of score components
- Comparison with scheme requirements
- Personalized recommendations

**Scoring Parameters:**
- Income level (0-40 points)
- Age bracket (0-20 points)
- State/District match (0-15 points)
- Category (SC/ST/OBC/General) (0-15 points)
- Occupation type (0-10 points)

**Acceptance Criteria:**
- Score generated within 2 seconds
- Explanation is in simple language
- Shows what user needs to improve eligibility
- Handles missing information gracefully

**Example Output:**
```
Eligibility Score: 78% (Eligible)
✓ Your income (₹1.5L) is within limit (₹2L)
✓ Your age (28) matches requirement (18-35)
✓ You belong to eligible category (SC)
✗ Document pending: Income Certificate
```

---

#### F3: RAG-Based Scheme Recommender
**Priority:** HIGH  
**Description:** Retrieval-Augmented Generation system that fetches relevant schemes from official sources

**Requirements:**
- Vector database with embedded scheme documents
- Semantic search capability
- Real-time retrieval from government portals
- Context-aware recommendations
- Source citation for transparency

**Data Sources:**
- myscheme.gov.in (primary)
- data.gov.in (API integration)
- india.gov.in (fallback)
- State government portals

**Acceptance Criteria:**
- Returns top 5 most relevant schemes
- Response includes scheme name, description, eligibility, benefits
- Provides official source link
- Updates daily from government APIs
- Handles queries in natural language

---

#### F4: AI Simplification Engine
**Priority:** MEDIUM  
**Description:** Converts complex government language into simple, understandable text

**Requirements:**
- LLM-based text simplification
- Maintains factual accuracy
- Adapts to user's education level
- Preserves critical information (dates, amounts)
- Supports all major Indian languages

**Example:**
```
Official Text:
"The applicant must be a bonafide resident of the state for a minimum continuous period of 5 years preceding the date of application and should submit requisite documentation."

Simplified:
"You must have lived in the state for 5 years continuously. You need to submit proof of residence."
```

**Acceptance Criteria:**
- Reduced reading complexity by 3+ grade levels
- 95%+ factual accuracy
- Generates output within 5 seconds
- User testing shows 80%+ comprehension

---

#### F5: Document Helper (OCR)
**Priority:** MEDIUM  
**Description:** Optical Character Recognition system to extract and verify documents

**Requirements:**
- Upload support: JPG, PNG, PDF
- Extract text from images
- Identify document type (Income Certificate, Aadhaar, etc.)
- Validate extracted information
- Compare with scheme requirements
- Highlight missing/incorrect information

**Supported Documents:**
- Income certificates
- Caste certificates
- Age proof (Aadhaar, Birth certificate)
- Bank passbook
- Ration card

**Acceptance Criteria:**
- 85%+ OCR accuracy on clear images
- Works with photos taken on mobile phones
- Handles regional language documents
- Processes document within 10 seconds
- Provides clear error messages

---

#### F6: User Dashboard
**Priority:** HIGH  
**Description:** Clean, intuitive interface showing personalized scheme information

**Dashboard Components:**
- Eligibility score meter
- Recommended schemes (top 5)
- Application status tracker
- Document upload section
- Notification center
- Profile management

**Design Principles:**
- Mobile-first design
- Lightweight (loads on 2G)
- Maximum 3 clicks to any feature
- Visual icons for low-literacy users
- Regional language support

**Acceptance Criteria:**
- Loads within 5 seconds on 3G
- Works on screens from 320px width
- Accessible (WCAG 2.1 Level A)
- Offline-capable for core features

---

### 3.2 Advanced Features (Good-to-Have)

#### F7: Skill Development Recommender
**Priority:** LOW  
**Description:** Suggests relevant skill training programs based on user profile

**Requirements:**
- Integration with NSDC/Skill India database
- Location-based training center finder
- Course completion tracking
- Certification verification

---

#### F8: Opportunity Recommendation Engine
**Priority:** LOW  
**Description:** Job and livelihood opportunity matching

**Requirements:**
- Integration with National Career Service (ncs.gov.in)
- Job matching based on skills
- Alert system for new opportunities
- Application tracking

---

#### F9: Email/SMS Notification System
**Priority:** MEDIUM  
**Description:** Automated alerts for deadlines and new schemes

**Requirements:**
- Email notifications for scheme deadlines
- SMS alerts for document verification
- Weekly digest of new schemes
- Reminder before application closing

---

## 4. Non-Functional Requirements

### 4.1 Performance
- **Response Time:** < 3 seconds for eligibility calculation
- **Page Load:** < 5 seconds on 3G connection
- **Concurrent Users:** Support 100+ simultaneous users
- **API Response:** < 2 seconds for RAG retrieval

### 4.2 Scalability
- Horizontal scaling capability
- Support for 10,000+ schemes in database
- Handle 1 million+ user profiles
- Regional deployment strategy

### 4.3 Security
- Encrypted data transmission (HTTPS)
- Secure document storage (encrypted at rest)
- No storage of sensitive personal data
- GDPR/Indian privacy law compliance
- Regular security audits

### 4.4 Accessibility
- WCAG 2.1 Level A compliance
- Screen reader compatible
- Keyboard navigation support
- High contrast mode
- Text resizing (up to 200%)

### 4.5 Localization
- Support for 12+ Indian languages
- Right-to-left text support (Urdu)
- Regional date/currency formats
- Cultural sensitivity in content

### 4.6 Reliability
- 99% uptime SLA
- Automatic failover
- Data backup every 24 hours
- Error logging and monitoring

---

## 5. Data Requirements

### 5.1 Government Scheme Data

**Primary Sources:**
1. **myScheme Portal** (https://www.myscheme.gov.in/)
   - 200+ central schemes
   - 500+ state schemes
   - Real-time updates

2. **Open Government Data** (https://data.gov.in/)
   - Structured CSV/JSON datasets
   - API access for automation
   - Historical scheme data

3. **India.gov.in** (https://www.india.gov.in/my-government/schemes)
   - Official scheme descriptions
   - Ministry-wise categorization

**Data Fields Required:**
- Scheme name
- Ministry/Department
- Eligibility criteria (income, age, category, state)
- Benefits/subsidies
- Required documents
- Application process
- Deadline dates
- Contact information

**Update Frequency:** Daily sync with government APIs

---

### 5.2 User Profile Data

**Stored Information:**
- Basic: Name, age, gender
- Location: State, district, pin code
- Economic: Income bracket, occupation
- Category: SC/ST/OBC/General
- Education level
- Language preference

**Privacy Considerations:**
- Minimal data collection
- User consent required
- Option to delete account
- No third-party sharing
- Anonymous analytics only

---

### 5.3 Document Storage

**Approach:** Temporary storage only
- Documents stored for 7 days
- Automatic deletion after verification
- Encrypted storage (AES-256)
- No permanent retention

---

## 6. Integration Requirements

### 6.1 External APIs

#### Government APIs
- **myScheme API:** Scheme search and filtering
- **DigiLocker:** Document verification (future)
- **Aadhaar eKYC:** Identity verification (future)

#### AI/ML Services
- **OpenAI GPT-4/Gemini:** Simplification engine
- **Whisper API:** Speech-to-text
- **Google TTS:** Text-to-speech

#### Communication
- **SendGrid/Mailgun:** Email notifications
- **Twilio/MSG91:** SMS alerts (future)

### 6.2 Database Integration
- **Vector Database:** FAISS/ChromaDB for RAG
- **Primary Database:** PostgreSQL for user data
- **Cache Layer:** Redis for performance

---

## 7. User Stories

### Story 1: First-Time User Discovery
**As a** rural youth looking for skill training  
**I want to** ask "What schemes are available for me?" in Tamil  
**So that** I can discover relevant government programs without technical knowledge

**Acceptance Criteria:**
- User speaks query in Tamil
- System shows 3-5 relevant schemes
- Eligibility is clearly indicated
- Explanation is in simple Tamil

---

### Story 2: Eligibility Checking
**As a** small farmer  
**I want to** check if I qualify for PM-KISAN  
**So that** I can apply with confidence

**Acceptance Criteria:**
- User enters basic profile information
- System calculates eligibility score
- Shows what documents are needed
- Provides application link

---

### Story 3: Document Verification
**As a** woman entrepreneur  
**I want to** upload my income certificate photo  
**So that** the system can verify I meet requirements

**Acceptance Criteria:**
- User uploads photo from mobile
- OCR extracts income amount
- System compares with scheme limit
- Shows verification status

---

### Story 4: Simple Language Explanation
**As a** low-literacy user  
**I want to** understand scheme details in simple language  
**So that** I can make informed decisions

**Acceptance Criteria:**
- Official text is simplified
- Uses common words only
- Maintains accuracy
- Available in regional language

---

## 8. Technical Constraints

### Must Use:
- **Primary Language:** Python (for AI/ML ecosystem)
- **Backend Framework:** FastAPI
- **Frontend:** React or Streamlit
- **AI/ML:** LangChain, FAISS, Transformers

### Hosting:
- **Preferred:** AWS (for hackathon alignment)
- **Services:** EC2, S3, RDS, Lambda
- **Alternative:** Free tiers for prototype

### Browser Support:
- Chrome (mobile & desktop)
- Firefox
- Safari
- Mobile browsers (priority)

### Network:
- Must work on 2G/3G
- Progressive Web App (PWA) capability
- Offline mode for core features

---

## 9. Success Metrics

### Hackathon Demo Metrics
- **Demo Stability:** 0 crashes during presentation
- **Response Time:** < 3 seconds average
- **Accuracy:** 90%+ eligibility calculation accuracy
- **Voice Recognition:** 85%+ accuracy for demo languages

### Impact Metrics (Projected)
- **User Adoption:** 1000+ users in first month (pilot)
- **Scheme Discovery:** 5x increase in awareness
- **Application Rate:** 30% increase in eligible applications
- **User Satisfaction:** 4.5/5 rating

### Technical Metrics
- **API Uptime:** 99%+
- **RAG Relevance:** 80%+ relevant results
- **OCR Accuracy:** 85%+ on standard documents
- **Load Time:** < 5 seconds on 3G

---

## 10. Out of Scope (Not for Hackathon)

The following features are planned for future but NOT in hackathon prototype:

❌ Direct application submission  
❌ Payment gateway integration  
❌ WhatsApp/IVR integration  
❌ Blockchain verification  
❌ Advanced ML models (use pre-trained only)  
❌ Mobile native apps (web-only for now)  
❌ Multi-tenant architecture  
❌ Complete DigiLocker integration  

---

## 11. Risk Assessment

### Technical Risks
| Risk | Impact | Mitigation |
|------|--------|------------|
| API rate limits hit during demo | High | Cache responses, use fallback data |
| OCR fails on poor quality images | Medium | Clear upload guidelines, manual override |
| Voice recognition inaccurate | Medium | Always provide text input option |
| Government API downtime | High | Daily data sync, offline fallback |

### Project Risks
| Risk | Impact | Mitigation |
|------|--------|------------|
| Feature creep | High | Strict scope control, MVP focus |
| Insufficient time for RAG | High | Use pre-built libraries, smaller dataset |
| Team member unavailable | Medium | Clear task division, documentation |

---

## 12. Acceptance Criteria for Hackathon Win

### Must Demonstrate:
✅ **Working voice input** in at least 2 languages  
✅ **Live eligibility calculation** with visual score  
✅ **RAG retrieval** from real government data  
✅ **OCR demo** with uploaded document  
✅ **Simplification** of complex scheme text  
✅ **Smooth UI** with no crashes  

### Presentation Must Show:
✅ Clear problem-solution narrative  
✅ Social impact potential  
✅ Technical depth (architecture)  
✅ Scalability vision  
✅ Live demo (not video)  
✅ Real government data sources  

### Judges Will Evaluate:
1. **Innovation** (30%) - Eligibility scoring + RAG + Voice
2. **Technical Execution** (30%) - Clean demo, no crashes
3. **Social Impact** (25%) - Bharat focus, inclusion
4. **Scalability** (15%) - Can this grow?

---

## 13. Development Timeline (7 Days)

**Day 1-2:** Data collection from government portals  
**Day 3:** Eligibility engine + scoring logic  
**Day 4-5:** RAG setup with vector database  
**Day 6:** Voice + Simplification engine  
**Day 7:** OCR + Dashboard + Integration  
**Final Day:** Testing + Presentation prep  

---

## 14. Appendix

### A. Government Portal Access
- myScheme: No API key needed for public data
- data.gov.in: Free API registration
- NCS: Public job listings available

### B. Sample User Profiles for Testing
1. Rural youth, Tamil Nadu, income ₹50,000/year
2. Small farmer, Uttar Pradesh, 2 hectares land
3. Woman entrepreneur, Maharashtra, first business

### C. Sample Schemes for Demo
1. PM-KISAN (farmer direct benefit)
2. PM-KAUSHAL (skill development)
3. MUDRA Loan (micro-finance)
4. National Apprenticeship Scheme
5. Pradhan Mantri Awas Yojana

---

**Document Status:** FINAL  
**Approved By:** Team Lead  
**Next Review:** Post-Hackathon
