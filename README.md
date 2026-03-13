<div align="center">

# VaidyaScribe — Ambient AI Clinical Scribe



**Speak naturally. Get structured FHIR-compliant clinical records instantly.**

Built for **PS-1: Mobile-First Ambient AI Scribe** — HACKMATRIX 2.0 (a national hackathon) Co-organized by Jilo Health & NJACK IIT Patna 

---

## 🌐 Live Deployment

**https://vaidyascribe-iitp.vercel.app**

---

</div>

---

## The Problem

Doctors in Tier 2/3 Indian cities spend 30–40% of their time on paperwork. Clinical notes are handwritten, unstructured, and never interoperable. Junior doctors and nurses re-enter the same data multiple times across systems. Critical clinical signals get lost in translation.

**VaidyaScribe converts a doctor–patient conversation directly into structured FHIR R4 records — in real time.**

---

## What It Does

| Step | What Happens |
|------|-------------|
| 🎙️ Record | Doctor speaks naturally in Hindi, English, or Hinglish |
| 📝 Transcribe | Groq Whisper `large-v3` converts speech to text in real time |
| 🧠 Extract | Groq LLaMA `3.3-70b` pulls symptoms, diagnosis, vitals, medications, lab orders |
| ⚕️ FHIR | Auto-generates a FHIR R4 Bundle with Patient, Encounter, Condition, Observation, MedicationRequest, ServiceRequest |
| ⬇️ Export | Download the full FHIR Bundle as `.json` |

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18 + Vite (PWA) — deployed on Vercel |
| Speech-to-Text | Groq Whisper `large-v3` |
| NLP / Extraction | Groq `llama-3.3-70b-versatile` |
| FHIR Mapping | Custom FHIR R4 mapper (no external dependency) |
| Multilingual | Hindi + English + Hinglish (auto-detected) |

**100% free to run** — Groq free tier is sufficient for demo and light production use.

---

## Project Structure

```
scribe-app/
├── public/
│   └── manifest.json         # PWA manifest
└── src/
    ├── App.jsx                # Main app + ClinicalView
    ├── main.jsx               # Entry point
    ├── styles/
    │   └── global.css         # Clean light theme
    ├── components/
    │   ├── Header.jsx         # Top nav
    │   ├── RecordingPanel.jsx # Mic + timer + waveform visualizer
    │   ├── TranscriptPanel.jsx# Raw transcript display
    │   └── FHIRPanel.jsx      # FHIR bundle viewer + JSON export
    └── services/
        ├── groq.js            # Whisper + LLaMA API calls
        └── fhir.js            # FHIR R4 bundle builder
```

---

## FHIR R4 Resources Generated

| Resource | What It Captures |
|----------|-----------------|
| `Patient` | Name, age, gender extracted from conversation |
| `Encounter` | Visit type, date, chief complaint |
| `Condition` | Diagnoses (encounter-diagnosis) + Symptoms (problem-list-item) |
| `Observation` | Vitals — BP, Temperature, SpO2, Pulse, Weight with LOINC codes |
| `MedicationRequest` | Prescribed medicines with dose and frequency |
| `ServiceRequest` | Ordered lab tests and investigations |

All resources are linked by reference (`urn:uuid`) and wrapped in a FHIR `transaction` Bundle.

---

## Sample Hinglish Input

> *"Patient ka naam Ramesh hai, 45 saal ke hain. Unhe 3 din se bukhar hai, 102 degree temperature. Khasi bhi hai aur body pain. BP 140/90 hai. Diagnosis: Viral fever with upper respiratory tract infection. Tab Paracetamol 500mg TDS 5 days, Tab Cetirizine OD at night. CBC aur CRP test karwaiye. 3 din baad follow up."*

**Output:** FHIR Bundle with 1 Patient · 1 Encounter · 2 Conditions · 5 Observations · 2 MedicationRequests · 2 ServiceRequests

---

## Key Stakeholders

- General Physicians in SME hospitals
- Junior doctors and duty medical officers
- Nurses documenting bedside notes
- Clinic owners in Tier 2/3 cities

---

## Self Hosting

```bash
git clone https://github.com/YOUR_USERNAME/vaidya-scribe.git
cd scribe-app
npm install
```

Create `.env`:
```
VITE_GROQ_API_KEY=gsk_your_key_here
```

```bash
npm run dev
```

Free API key: [Groq Console](https://console.groq.com) — no credit card required.

---

## Deploy

```bash
npm run build
# Deploy dist/ to Vercel or Render (Static Site)
# Add VITE_GROQ_API_KEY as environment variable
```

---

> *"A doctor today spends 40% of their time on paperwork. VaidyaScribe brings it to zero."*
