# Project Aasha – The AI Medical Co-Pilot
> Empowering India's 1M+ ASHA and ANM workers with AI-powered decision support for rural healthcare

**Team:** DimensionAI  
**Team Leader:** Ananya Marghade

---

## Table of Contents

- [Overview](#-overview)
- [Problem Statement](#-problem-statement)
- [Solution](#-solution)
- [Key Features](#-key-features)
- [System Workflow](#-system-workflow)
- [Technology Stack](#-technology-stack)
- [Project Structure](#-project-structure)
- [Ethical Considerations & Limitations](#-ethical-considerations--limitations)
- [Future Scope](#-future-scope)
- [Getting Started](#-getting-started)
- [License](#-license)

---

## Overview

Project Aasha is a **voice-first, offline-first AI mobile application** designed to act as a medical decision-support co-pilot for India's frontline healthcare workers—ASHA (Accredited Social Health Activist) and ANM (Auxiliary Nurse Midwife) workers. 

By leveraging AI-powered symptom analysis, intelligent triage, and automated documentation, Aasha bridges the gap between undertrained field workers and quality healthcare delivery in rural India.

**⚠️Important:** Aasha provides decision support only—it does not diagnose or replace medical professionals.

---

## Problem Statement

India's rural healthcare system relies on **1M+ ASHA and ANM workers** who serve as the backbone of community health. However, they face critical challenges:

- **Undertraining:** Limited medical knowledge and reliance on outdated paper handbooks
- **Overburdened:** Managing diverse conditions from anemia to tuberculosis with minimal support
- **Lack of Real-Time Guidance:** No access to decision-support tools during field visits
- **Delayed Treatment:** Incorrect assessments lead to preventable deaths and poor health outcomes
- **Poor Connectivity:** Rural areas often lack reliable internet access

These challenges result in delayed treatment, incorrect referrals, and preventable deaths across underserved communities.

---

## Solution

**Project Aasha** is an AI-powered mobile co-pilot that provides real-time, voice-based decision support to ASHA and ANM workers during patient interactions.

### Design Principles

- **Decision Support Only:** Assists workers but does not provide diagnoses
- **Voice-First Interaction:** Supports local Indian languages for accessibility
- **Offline-First Architecture:** Works seamlessly in low-connectivity rural areas

### Unique Differentiators

- Voice-first interface in regional languages
- Offline-first AI inference engine
- Color-coded AI triage system (🟢 Green / 🟡 Yellow / 🔴 Red)
- AI as a co-pilot, not a replacement for doctors
- Automated documentation with ABHA integration
- GPS-based follow-up and referral tracking  

---

## Key Features

### Voice-Based Symptom Input
- Natural language processing in regional Indian languages
- Hands-free operation for field workers

### AI Symptom Analysis
- NLP-powered symptom extraction and analysis
- Medical knowledge graph for contextual understanding

### Smart Triage System
- **🟢 Green:** Home care with self-management guidance
- **🟡 Yellow:** Referral to PHC/CHC within 48 hours
- **🔴 Red:** Emergency escalation with immediate ambulance integration

### Protocol-Based Treatment Guidance
- Step-by-step examination protocols
- Evidence-based care recommendations

### Automated Digital Health Records
- Auto-generated patient documentation
- ABHA (Ayushman Bharat Health Account) integration
- Secure, encrypted health data storage

### GPS-Based Follow-Up System
- Location-tagged patient visits
- Automated follow-up reminders
- Referral tracking and closure

### Emergency Escalation
- One-tap ambulance integration
- Auto-generated referral notes for hospitals
- Real-time emergency alerts

### Offline Mode with Sync Support
- Full functionality without internet
- Background sync when connectivity is restored
- Local storage with cloud backup

---

## System Workflow
The following workflow explains how Project Aasha assists ASHA/ANM workers during a field consultation:

### Step 1: Voice-Based Symptom Input

The ASHA/ANM worker interacts with the app using voice in a regional language

Patient symptoms are captured hands-free during the visit

### Step 2: AI Symptom Analysis

NLP models extract key symptoms, duration, and severity

A medical knowledge graph provides clinical context and protocol mapping

### Step 3: AI-Driven Risk Triage

Based on symptom analysis, the system classifies patient risk into three levels:

🟢 Green — Home care guidance

🟡 Yellow — Referral to PHC/CHC within 48 hours

🔴 Red — Emergency escalation

### Step 4: Guided Action & Decision Support

Step-by-step protocol-based examination guidance

Clear treatment or referral recommendations

Automatic ambulance escalation for high-risk (Red) cases

### Step 5: Automated Documentation & Follow-Up

Digital health record generated automatically

ABHA-compliant record creation

GPS-tagged follow-up reminders for continuity of care

This workflow ensures faster decision-making, fewer incorrect referrals, and improved last-mile healthcare delivery while keeping the human worker in control.


---

## Technology Stack

### Frontend
- **Flutter** or **React Native** – Cross-platform mobile development
- Local storage for offline-first architecture

### Backend
- **Python** – Core backend services
- **FastAPI/Flask** – RESTful API development

### AI/NLP
- **AWS Transcribe** – Voice-to-text in regional languages
- **spaCy** – NLP and entity extraction
- **Transformers (Hugging Face)** – Advanced language models
- **Medical Knowledge Graph** – Symptom-disease mapping

### ML Frameworks
- **TensorFlow** – Model training and inference
- **PyTorch** – Deep learning models

### Database
- **MongoDB** – NoSQL database for flexible health records
- **Local storage with sync engine** – Offline data persistence

### Cloud & Infrastructure
- **AWS** – Cloud hosting and services
- **AWS S3** – Media and document storage
- **AWS Lambda** – Serverless functions

### APIs & Integrations
- **ABHA APIs** – Ayushman Bharat Health Account integration
- **Maps & GPS APIs** – Location tracking and follow-ups
- **Ambulance APIs** – Emergency service integration

---

## Project Structure
project-aasha/
│
├── mobile-app/                 # Flutter / React Native mobile application
│   ├── lib/
│   │   ├── screens/            # App UI screens
│   │   ├── services/           # API calls and local services
│   │   ├── models/             # Data models
│   │   ├── widgets/            # Reusable UI components
│   │   └── utils/              # Helper utilities
│   ├── assets/                 # Images, audio files, fonts
│   └── pubspec.yaml            # Mobile app dependencies
│
├── backend/                    # Python backend services
│   ├── api/
│   │   ├── routes/             # REST API endpoints
│   │   ├── controllers/        # Business logic
│   │   └── middleware/         # Authentication, logging, etc.
│   ├── ai/
│   │   ├── nlp/                # NLP pipelines and models
│   │   ├── triage/             # Risk triage logic
│   │   └── knowledge_graph/    # Medical knowledge base
│   ├── models/                 # Database models
│   ├── services/               # External API integrations
│   └── requirements.txt        # Python dependencies
│
├── ml-models/                  # Machine learning components
│   ├── training/               # Model training scripts
│   ├── inference/              # Inference engines
│   └── data/                   # Training and evaluation datasets
│
├── docs/                       # Project documentation
│   ├── api-docs.md             # API reference
│   ├── architecture.md         # System architecture
│   └── user-guide.md           # User and field-worker guide
│
├── tests/                      # Test suites
│   ├── unit/                   # Unit tests
│   ├── integration/            # Integration tests
│   └── e2e/                    # End-to-end tests
│
├── .github/                    # GitHub workflows (CI/CD)
├── docker-compose.yml          # Container orchestration
├── README.md                   # Project overview and setup
└── LICENSE                     # MIT License


---

## Ethical Considerations & Limitations

### Ethical Principles

- **No Diagnosis:** Aasha provides decision support only and does not diagnose medical conditions
- **Human-in-the-Loop:** All recommendations require validation by trained healthcare workers
- **Data Privacy:** Patient data is encrypted and complies with healthcare data protection standards
- **Transparency:** Workers are informed that AI suggestions are advisory, not prescriptive
- **Bias Mitigation:** Models are trained on diverse datasets to minimize regional and demographic bias

### Limitations

⚠️ **Not a Medical Device:** Aasha is not certified as a medical device and should not be used as a sole decision-making tool  
⚠️ **Requires Training:** ASHA/ANM workers must be trained on proper usage  
⚠️ **Limited Scope:** Covers common conditions; complex cases require specialist referral  
⚠️ **Connectivity Dependent (Sync):** While offline-capable, data sync requires periodic connectivity  
⚠️ **Language Support:** Initial release supports limited regional languages (expandable)  

---

## Future Scope

### Short-Term (3-6 months)
- Expand language support to 15+ Indian regional languages
- Integration with government health portals (e-Sanjeevani, CoWIN)
- Telemedicine module for remote doctor consultations
- Offline voice synthesis for audio guidance

### Medium-Term (6-12 months)
- Computer vision for rash/wound analysis
- Predictive analytics for disease outbreak detection
- Integration with wearable health devices
- Community health dashboard for supervisors

### Long-Term (1-2 years)
- AI-powered training modules for ASHA/ANM workers
- Blockchain-based health record verification
- Expansion to other frontline workers (teachers, anganwadi workers)
- International deployment in similar low-resource settings

---

## 🏁 Getting Started

### Prerequisites
- Node.js (v16+) or Flutter SDK
- Python 3.9+
- MongoDB
- AWS Account (for cloud services)

### Installation

```bash
# Clone the repository
git clone https://github.com/dimensionai/project-aasha.git
cd project-aasha

# Backend setup
cd backend
pip install -r requirements.txt
python app.py

# Mobile app setup (Flutter example)
cd mobile-app
flutter pub get
flutter run
Configuration
Create a .env file in the backend directory:

MONGODB_URI=your_mongodb_connection_string
AWS_ACCESS_KEY=your_aws_access_key
AWS_SECRET_KEY=your_aws_secret_key
ABHA_API_KEY=your_abha_api_key
License
This project is licensed under the MIT License - see the LICENSE file for details.

Contributing
We welcome contributions from the community! Please read our CONTRIBUTING.md for guidelines.

Contact
Team DimensionAI
Team Leader: Ananya Marghade
Email: ananyamarghade@gmail.com
