# Saarthi AI
### Team: The Dark Knight

Saarthi AI is a multilingual, AI-powered public access assistant designed to simplify access to government schemes, skill programs, and opportunities across India.

Our mission is to make public services understandable, accessible, and actionable for every citizen.

---

## Problem

India has thousands of government schemes for farmers, students, women, entrepreneurs, and job seekers. 

However, many eligible citizens do not apply because:

- Information is scattered across multiple portals  
- Eligibility rules are difficult to understand  
- Official language is complex  
- Documentation requirements are unclear  

Saarthi AI addresses this gap through intelligent automation and personalization.

---

## Solution Overview

Saarthi AI acts as a digital guide that:

- Supports multilingual voice interaction (Tamil, Hindi, English)  
- Calculates an eligibility score (0–100%)  
- Recommends relevant schemes using RAG-based retrieval  
- Simplifies official scheme language  
- Verifies uploaded documents using OCR  
- Provides skill and opportunity recommendations  
- Sends personalized email notifications  

It does not just provide information — it guides users step-by-step.

---

## Core Features

- Multilingual Voice Assistant  
- AI Eligibility Engine with scoring & explanation  
- RAG-Based Scheme Recommender  
- AI Simplification Engine  
- OCR-Based Document Helper  
- Clean User Dashboard  
- Basic Skill Recommender  
- Basic Opportunity Engine  
- Email Notification System  

---

## Data Sources

Saarthi AI integrates real government data from:

- https://www.myscheme.gov.in/
- https://data.gov.in/
- https://www.india.gov.in/my-government/schemes
- https://www.ncs.gov.in/
- https://www.skillindia.gov.in/

---

## Technology Stack

**Backend:** Python, FastAPI  
**Frontend:** React + Tailwind CSS  
**AI Frameworks:** LangChain, FAISS / ChromaDB  
**Speech Processing:** Whisper, gTTS  
**OCR:** Tesseract  
**Database:** PostgreSQL / MongoDB  

---

## Architecture

Frontend → FastAPI Backend → AI Modules → Database + Vector Store → Email Service

AI Modules include:
- Eligibility Engine
- RAG Retrieval Engine
- Simplification Engine
- OCR Document Validator

---

## Target Beneficiaries

- Rural youth  
- Small farmers  
- Women entrepreneurs  
- First-time job seekers  

---

## Vision

Saarthi AI aims to transform complex government systems into simple, accessible digital guidance.  

Our long-term vision includes WhatsApp integration, IVR-based voice access, and district-level deployment in collaboration with public institutions.

---

## Hackathon Submission

This repository contains:
- requirements.md
- design.md
- Presentation deck

---

Saarthi AI — A digital guide for inclusive public access.
