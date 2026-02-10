# Project Aasha – System Design Document

**Team:** DimensionAI  
**Team Leader:** Ananya Marghade  
**Version:** 1.0

---

## 1. Problem Overview

India's 1M+ ASHA and ANM workers are the backbone of rural healthcare, but they face critical challenges:

- **Undertrained** – Limited medical knowledge, reliance on paper handbooks
- **Overburdened** – Managing diverse conditions from anemia to TB with minimal support
- **No Real-Time Guidance** – Lack of decision-support tools during field visits
- **Poor Connectivity** – Rural areas often have unreliable internet access

**Result:** Delayed treatment, incorrect referrals, and preventable deaths.

---

## 2. Solution Summary

**Project Aasha** is a voice-first, offline-first AI mobile app that acts as a medical co-pilot for ASHA/ANM workers.

### Core Capabilities
- AI-based symptom analysis and risk triage
- Voice interaction in local Indian languages
- Offline operation with cloud sync
- Automated health records and referrals
- Emergency escalation with ambulance integration

### Design Principles
- **Decision support only** – Does not diagnose or replace doctors
- **Voice-first** – Hands-free operation in regional languages
- **Offline-first** – Full functionality without internet

---

## 3. High-Level System Architecture
flowchart TD

A[ASHA / ANM Worker<br/>Voice Interaction] --> B[Mobile App<br/>(Flutter / React Native)]

B --> B1[Voice UI]
B --> B2[Triage Dashboard]
B --> B3[Patient Records]

B --> C[On-Device AI Layer<br/>(Offline-First)]

C --> C1[Speech-to-Text<br/>(TensorFlow Lite)]
C --> C2[NLP Engine<br/>(spaCy)]
C --> C3[Triage ML Model]

C --> D[Local Storage<br/>(SQLite)]
D --> D1[Patient Records]
D --> D2[Sync Queue]
D --> D3[Medical Knowledge Cache]

D -->|Internet Available| E[Backend Services<br/>(AWS)]

E --> E1[API Gateway]
E --> E2[Sync Service]
E --> E3[Analytics]
E --> E4[MongoDB<br/>(Patient Database)]
E --> E5[Medical Knowledge Graph]

E --> F[External APIs]
F --> F1[ABHA Health ID]
F --> F2[Maps / GPS]
F --> F3[Ambulance Services 108]

---

## 4. Core Components

### 4.1 Mobile Application
- **Technology:** Flutter or React Native
- **Features:**
  - Voice input/output interface
  - Symptom capture and vital signs entry
  - Triage dashboard with color-coded alerts
  - Patient records management
  - Offline-first state management
  - Background sync

### 4.2 On-Device AI/NLP
- **Speech-to-Text:** TensorFlow Lite model (offline) + AWS Transcribe (online fallback)
- **NLP Engine:** spaCy for symptom entity extraction
- **Triage Model:** Lightweight ML model (Random Forest/Neural Network)
- **Knowledge Graph:** Cached medical protocols and symptom-disease mappings

### 4.3 Local Storage
- **Database:** SQLite
- **Stores:**
  - Patient demographics and history
  - Consultation records
  - Sync queue (pending operations)
  - Cached medical knowledge

### 4.4 Backend Services
- **API Gateway:** REST/GraphQL endpoints
- **Auth Service:** AWS Cognito for user authentication
- **Sync Service:** Handles data synchronization and conflict resolution
- **NLP Service:** Advanced processing when online
- **Analytics:** Track usage, model performance, health outcomes

### 4.5 Medical Knowledge Graph
- **Technology:** MongoDB with graph capabilities or Neo4j
- **Contains:**
  - Symptom-disease relationships
  - Treatment protocols
  - Referral pathways
  - Drug information

---

## 5. AI & Triage Logic

### 5.1 Symptom Analysis Pipeline

Voice Input → STT → NLP Entity Extraction → Knowledge Graph Query ↓ Triage Classification


### 5.2 Triage System (Hybrid Approach)

**Rule-Based Component (Priority 1)**
- Hard-coded emergency rules
- Examples:
  - SpO2 < 90% → RED
  - Severe bleeding → RED
  - Chest pain + difficulty breathing → RED

**ML Component (Priority 2)**
- Random Forest or Neural Network classifier
- Input features:
  - Symptom embeddings
  - Vital signs (normalized)
  - Patient demographics
  - Symptom duration and severity
- Output: [P(Green), P(Yellow), P(Red)] + confidence score

**Decision Logic:**
IF rule-based triggers RED/YELLOW: Use rule-based result ELSE: Use ML prediction with confidence threshold


### 5.3 Triage Categories

- **🟢 Green:** Home care with self-management guidance
- **🟡 Yellow:** Referral to PHC/CHC within 48 hours
- **🔴 Red:** Emergency escalation, ambulance integration

---

## 6. Offline-First Design Approach

### 6.1 Core Principles
- All features work without internet by default
- Sync opportunistically when connection available
- Graceful degradation for advanced features

### 6.2 Offline Capabilities
- ✅ Voice input and transcription (on-device STT)
- ✅ Symptom analysis and triage (local ML models)
- ✅ Protocol-based guidance (cached knowledge)
- ✅ Patient record creation (local SQLite)
- ✅ GPS tracking and follow-up reminders

### 6.3 Sync Strategy
- **Triggers:** WiFi detected, manual sync, scheduled (every 6 hours)
- **Priority:** RED cases synced first
- **Conflict Resolution:** Server timestamp wins (default), manual merge for critical conflicts
- **Queue Management:** Retry failed syncs with exponential backoff

### 6.4 Model Deployment
- Models converted to TensorFlow Lite (<50MB each)
- INT8 quantization for smaller size
- Bundled with app or downloaded on first launch
- OTA updates for model improvements

---

## 7. Data Flow (Brief)

### Typical Patient Interaction

ASHA worker speaks symptoms in local language ↓
On-device STT converts to text ↓
NLP extracts entities (symptoms, duration, severity) ↓
App prompts for vital signs (temp, BP, SpO2, etc.) ↓
Triage model classifies risk level (Green/Yellow/Red) ↓
Display guidance and next steps ↓
Auto-generate health record (ABHA compliant) ↓
Store locally + add to sync queue ↓
[When online] Sync to cloud + trigger referral/ambulance if needed


---

## 8. Tech Stack

### Frontend
- **Framework:** Flutter or React Native
- **State Management:** Provider/Riverpod (Flutter) or Redux (RN)
- **Local DB:** SQLite, Hive, or Realm

### Backend
- **Language:** Python
- **Framework:** FastAPI or Flask
- **Hosting:** AWS Lambda + API Gateway (serverless)

### AI/ML
- **Speech-to-Text:** AWS Transcribe (online), TensorFlow Lite (offline)
- **NLP:** spaCy, Hugging Face Transformers
- **ML Frameworks:** TensorFlow, PyTorch
- **Model Serving:** TensorFlow Lite, ONNX Runtime

### Database
- **Primary:** MongoDB (patient records, analytics)
- **Knowledge Graph:** Neo4j or MongoDB with graph queries
- **Local:** SQLite

### Cloud & Infrastructure
- **Cloud Provider:** AWS
- **Services:** S3, Lambda, API Gateway, Cognito, CloudWatch, SNS
- **CI/CD:** GitHub Actions, AWS CodePipeline

### External APIs
- **ABHA:** Health ID creation and record sharing
- **Maps:** Google Maps API for GPS and facility mapping
- **Ambulance:** 108 emergency service integration

---

## 9. Key Assumptions & Limitations

### Assumptions
- ASHA workers have Android smartphones (Android 8+)
- Basic smartphone literacy among users
- Periodic internet access for syncing (at least once per week)
- Medical protocols are standardized and available
- Government support for ABHA integration

### Limitations
- **Not a Diagnostic Tool:** Provides decision support only, not medical diagnosis
- **Model Uncertainty:** AI predictions may have errors; human judgment is essential
- **Language Coverage:** Initial launch supports 10 languages; more to be added
- **Connectivity:** Some features (ambulance API, real-time sync) require internet
- **Device Constraints:** Performance depends on phone specs (RAM, storage)
- **Data Quality:** Accuracy depends on correct symptom input and vital sign measurement

### Ethical Considerations
- Transparent AI reasoning (explainability)
- Confidence scores displayed to users
- Clear disclaimers about AI limitations
- Patient data privacy and security (encryption, RBAC)
- Compliance with Indian health data regulations

---

## 10. Future Enhancements

### Phase 2 Features
- **Federated Learning:** Train models across devices without centralizing patient data
- **Expanded Language Support:** Add 10+ more regional languages and dialects
- **Image Analysis:** Skin condition detection, wound assessment via camera
- **Telemedicine Integration:** Video consultation with doctors for YELLOW cases
- **Predictive Analytics:** Identify high-risk patients for proactive care

### Phase 3 Features
- **Government Integration:** Direct integration with National Health Mission systems
- **Supply Chain:** Track medicine availability at PHCs
- **Community Health Insights:** Aggregate data for disease outbreak detection
- **Wearable Integration:** Sync with low-cost health monitoring devices
- **Chatbot Support:** Text-based interaction for follow-up queries

### Research Directions
- Multi-modal learning (voice + image + vitals)
- Transfer learning for rare diseases
- Explainable AI improvements
- Edge computing optimization

---

## Contact

**Team DimensionAI**  
**Team Leader:** Ananya Marghade  
**Project:** Hackathon Submission

---

*This document is a living artifact and will be updated as the project evolves.*
