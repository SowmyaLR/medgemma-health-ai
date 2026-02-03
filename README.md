# Agentic AI Tele-Triage System

<img width="1024" height="1024" alt="Gemini_Generated_Image_ufttwhufttwhuftt" src="https://github.com/user-attachments/assets/36ca12e2-e8ad-417f-a558-b0e4ad5b7746" />

### Built for the Med-Gemma Impact Challenge

**Tele-Triage** is an autonomous AI agent designed to alleviate clinician burnout and improve patient access to care by automating the "intake-to-documentation" pipeline. 

By leveraging **Google's MedGemma** (reasoning) and **HeAR** (acoustic analysis concepts), this application listens to patients, analyzes clinical and acoustic biomarkers, and drafts professional clinical notes before the doctor even enters the room.

---

## 🏥 Healthcare Impact
This project directly addresses **two critical crises** in modern healthcare:

1.  **Clinician Burnout**: Doctors spend up to **50% of their day** on EHR documentation (SOAP notes). Tele-Triage automates this, returning hours to patient care.
2.  **Triage Bottlenecks**: In tele-health, nurses manually screen thousands of calls. Our "Agentic Workflow" acts as a **Level 1 Triage Agent**, autonomously flagging high-risk patients (e.g., detecting respiratory distress via cough sounds).


The system is not just a chatbot; it is an **Agent**:
-   **Perceives**: Listens to raw audio (Speech + Cough/Lung sounds).
-   **Reasons**: Uses **MedGemma** to interpret the clinical narrative (`S` and `O` sections).
-   **Acts**: Synthesizes a structured medical record (`A` and `P` draft) and pushes it to the clinician's queue.

We simulate the integration of **HeAR (Health Acoustic Representations)** to go beyond text. By analyzing the *sound* of a cough (not just the description of it), we introduce a novel modality—**Acoustic Biomarkers**—into the standard LLM reasoning chain.

---

## ⚙️ Technical Architecture

```mermaid
graph TD
    %% Users
    Patient(["👤 Patient"])
    Doctor(["👨‍⚕️ Doctor"])

    %% Frontend
    subgraph Frontend [Next.js Client]
        P_Portal["Patient Portal"]
        D_Portal["Doctor Dashboard"]
    end

    %% Backend
    subgraph Backend [FastAPI Server]
        Router["API Router"]
        
        subgraph Processing [AI Processing Pipeline]
            Whisper["🗣️ Whisper ASR<br/>(Speech-to-Text)"]
            Librosa["🌊 Acoustic Analysis<br/>(Breathing/Cough Detection)"]
            MedGemma["🧠 MedGemma 4B<br/>(Clinical Reasoning)"]
        end
    end

    %% External/Local Service
    Ollama["🦙 Ollama Local Inference"]

    %% Connections
    Patient -->|Voice/Symptom Input| P_Portal
    P_Portal -->|Audio Data| Router
    
    Router --> Whisper
    Router --> Librosa
    
    Whisper -->|Transcript| MedGemma
    Librosa -->|Acoustic Biomarkers| MedGemma
    
    MedGemma <-->|Llama 3/Gemma Weights| Ollama
    
    MedGemma -->|SOAP Note & Triage Score| D_Portal
    D_Portal -->|Review & Action| Doctor
    
    classDef user fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef component fill:#f3e5f5,stroke:#4a148c,stroke-width:2px;
    classDef ai fill:#fff3e0,stroke:#e65100,stroke-width:2px;
    
    class Patient,Doctor user;
    class P_Portal,D_Portal,Router component;
    class Whisper,Librosa,MedGemma,Ollama ai;
```

### **Hybrid AI Stack**
-   **Reasoning Engine**: `MedGemma 4B` (running locally via **Ollama**).
    -   *Why?* Ensures medical domain accuracy and data privacy (local inference).
-   **ASR (Speech-to-Text)**: `Whisper` (via Python Transformers).
    -   *Why?* Robust handling of medical terminology in speech.
-   **Acoustic Analysis**: Python `librosa` based heuristics (Simulating **HeAR**).
    -   *Why?* Measures signal energy and zero-crossing rates to flag "Abnormal Respiratory Sounds".

### **Stack**
-   **Frontend**: Next.js 14, Tailwind CSS (Patient & Doctor Portals).
-   **Backend**: Python FastAPI.
-   **Deployment**: Fully local (Privacy-first).

---

## 🚀 How it Works
1.  **Patient Interface**: Exploring the "Patient" mode, a user speaks naturally about their symptoms.
2.  **Multi-Modal Analysis**:
    -   The backend extracts the *text transcript*.
    -   It simultaneously analyzes *audio waveforms* for distress signals.
3.  **Agentic Reasoning**:
    -   MedGemma receives the transcript + acoustic risk score.
    -   It synthesizes a **SOAP Note** (Subjective, Objective, Assessment, Plan).
4.  **Doctor Interface**: The clinician sees a sorted queue. High-risk inputs (acoustic or clinical) are prioritized.

---

## 🛠️ Setup Instructions

### Prerequisites
-   Node.js & npm
-   Python 3.10+
-   **Ollama** (for MedGemma)
-   **FFmpeg** (for Audio processing)

### Quick Start

1.  **Start AI Server (Ollama)**
    ```bash
    ollama run medgemma
    ```

2.  **Start Backend**
    ```bash
    cd server
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    uvicorn main:app --reload --port 8000
    ```

3.  **Start Frontend**
    ```bash
    cd client
    npm run dev
    # Visit http://localhost:3000
    ```

## Demo

[![Watch the demo](https://img.youtube.com/vi/0F5j3puTxPI/0.jpg)](https://www.youtube.com/watch?v=0F5j3puTxPI)

