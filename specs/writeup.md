# MedGemma Tele-Triage: AI-Powered Clinical Intake Agent

### Project name 
**MedGemma Tele-Triage**

### Your team 
*   **L.R.Sowmya**: Tech Lead Manager

### Problem statement
**The Silent Crisis: Triage Bottlenecks & Language Barriers**
Emergency Rooms and Urgent Care clinics globally are facing a breaking point. Patient wait times are skyrocketing, leading to "Left Without Being Seen" (LWBS) rates as high as 10-15%. Specifically, respiratory conditions—often contagious—require immediate isolation, yet patients sit in crowded waiting rooms for hours. Furthermore, language barriers delay critical care; a Triage Nurse often waits 20+ minutes for a translator, during which time a patient's condition (e.g., silent hypoxia) may deteriorate.

**Impact Potential:**
Our solution targets the "Golden Hour" of patient intake. By automating the initial subjective assessment (History of Present Illness) and objective acoustic analysis (Cough audio), we aim to:
1.  **Reduce Triage Time by 50%**: Automating the 10-minute intake interview allows nurses to focus purely on vitals and acute stabilization.
2.  **Zero-Latency Translation**: Leveraging MedASR/Whisper, non-English speakers receive the same standard of care instantly, removing equity gaps.
3.  **Early Detection**: Acoustic biomarkers (HeAR) identify high-risk respiratory patterns (e.g., pneumonia/COVID-19 signatures) that are inaudible to the human ear, flagging high-risk patients *before* they even see a doctor.
4.  **EHR Interoperability & Burnout Reduction**: Physicians spend nearly 50% of their day on EHR data entry. By auto-generating structured SOAP notes that are "one-click" importable, MedGemma reclaims hours of clinical time, directly combating physician burnout and ensuring high-quality, standardized data hygiene in the electronic record.

### Overall solution: Effective use of HAI-DEF models
**MedGemma Tele-Triage** is a fully functional, privacy-first web application that acts as an "AI Resident Doctor". It sits at the clinic front desk (or on a patient's phone at home) and conducts a compassionate, clinical interview.

We integrated three core pillars of the Google Health AI Developer Foundations (HAI-DEF):

1.  **Gemma 2 (MedGemma) for Clinical Reasoning**:
    Unlike generic LLMs, we utilized the medically-tuned Gemma 2 9B (via Ollama) to act as the central reasoning agent. It doesn't just "chat"; it follows a clinical decision tree. It asks clarifying questions based on symptoms, rules out red flags, and synthesizes a structured **SOAP Note (Subjective, Objective, Assessment, Plan)**. This note is what physicians actually need, converting 5 minutes of rambling audio into 30 seconds of readable text.

2.  **HeAR (Health Acoustic Representations) for Objective Biomarkers**:
    We deployed the HeAR model to analyze raw audio waveforms. While the patient speaks, the system isolates cough and breathing events, assigning a "Respiratory Risk Score" (0.0 - 1.0). This provides an objective data point that complements the subjective patient history, flagging "silent" respiratory distress.

3.  **MedASR / Whisper for Universal Access**:
    The system uses state-of-the-art ASR to handle multilingual inputs (demonstrated with Tamil mixed-language support), ensuring the AI understands the *medical intent* regardless of the patient's native tongue.

### Technical details: Product Feasibility
**Architecture & Scalability:**
The system is built as a production-ready **Next.js** (React) frontend and a high-performance **FastAPI (Python)** backend.
*   **Frontend**: Features a "Pulse" interface that gives patients visual feedback that they are being heard, reducing anxiety. The Doctor Dashboard is a "Kanban-style" triage board that prioritizes patients based on MedGemma's risk assessment, not just arrival time.
*   **Backend**: The inference pipeline is optimized for speed. We use `RunPod/Ollama` for the LLM reasoning to keep costs low while maintaining data privacy (HIPAA compliant potential since data doesn't leave the secure container).
*   **Agentic Workflow**: The system is not a passive chatbot. It has a state machine:
    *   *Listening Mode* -> *Transcription* -> *Acoustic Analysis* -> *Clinical Reasoning* -> *Final Review*.
    *   **Human-in-the-Loop**: We implemented a strict approval workflow. The AI generates the *draft*, but the physician must click "Approve & E-Sign" to validate it. This safety mechanism makes the product feasible for real-world deployment today, mitigating hallucination risks.

**Feasibility:**
The prototype runs on consumer hardware (MacBook M-series) with minimal latency (~2s for acoustic analysis). It requires no specialized medical hardware—just a standard smartphone microphone—making it scalable to rural clinics and telemedicine providers immediately.