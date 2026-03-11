# VaidyaScribe — Ambient AI Clinical Scribe
## Mobile-First FHIR R4 + Groq Powered

### Quick Start

```bash
cd vaidya-scribe
npm install
npm run dev
```

Open `http://localhost:5173` — works best on mobile browser or Chrome DevTools mobile view.

---

### What It Does

1. **Record** — Tap the mic button, speak naturally (Hindi + English / Hinglish)
2. **Transcribe** — Groq Whisper `large-v3` converts speech to text
3. **Extract** — Groq `llama-3.3-70b-versatile` extracts clinical entities (symptoms, diagnosis, medications, vitals, lab orders)
4. **FHIR** — Auto-generates FHIR R4 Bundle with:
   - `Patient` resource
   - `Encounter` resource
   - `Condition` resources (symptoms + diagnosis)
   - `Observation` resources (vitals with LOINC codes)
   - `MedicationRequest` resources
   - `ServiceRequest` resources (lab orders)
5. **Export** — Download FHIR Bundle as `.json`

---

### Stack

| Layer | Tech |
|---|---|
| Frontend | React 18 + Vite (PWA) |
| Speech-to-Text | Groq Whisper large-v3 |
| NLP Extraction | Groq llama-3.3-70b-versatile |
| FHIR Mapping | Custom FHIR R4 mapper |
| Styling | Vanilla CSS (no Tailwind needed) |

---

### Env / Config

No `.env` needed — API key is entered in-app and stays in browser memory only (never stored).

To hardcode for demo:
```js
// src/App.jsx line 18
const [apiKey, setApiKey] = useState("gsk_YOUR_KEY_HERE");
const [showKeyInput, setShowKeyInput] = useState(false);
```

---

### Deploy as PWA on Android

```bash
npm run build
# Deploy dist/ folder to any static host (Vercel, Netlify, Render)
# Open on Android Chrome → "Add to Home Screen"
```

---

### Project Structure

```
src/
├── App.jsx                  # Main app + ClinicalView
├── main.jsx                 # Entry point
├── styles/
│   └── global.css           # All styles (dark medical theme)
├── components/
│   ├── Header.jsx           # Top nav bar
│   ├── RecordingPanel.jsx   # Mic + visualizer + timer
│   ├── TranscriptPanel.jsx  # Raw transcript display
│   └── FHIRPanel.jsx        # FHIR bundle viewer + export
└── services/
    ├── groq.js              # Whisper + LLaMA API calls
    └── fhir.js              # FHIR R4 bundle builder
```

---

### Sample Hinglish Transcript (for demo)

> "Patient ka naam Ramesh hai, 45 saal ke hain. Unhe 3 din se bukhar hai, 102 degree temperature. Khasi bhi hai aur body pain. BP 140/90 hai. Diagnosis: Viral fever with upper respiratory tract infection. Tab Paracetamol 500mg TDS 5 days, Tab Cetirizine OD at night, Cough syrup 10ml TDS. CBC aur CRP test karwaiye. 3 din baad follow up."

This will generate a complete FHIR bundle with Patient, Encounter, 2 Conditions, 5 Observations, 3 MedicationRequests, 2 ServiceRequests.
