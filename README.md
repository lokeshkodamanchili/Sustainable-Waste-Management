# Sustainable Waste Management Assistant Using Generative AI

A complete, modern smart-city web application designed to help users scan and identify waste items, receive instant AI-generated disposal and recycling instructions, find local dropoff centers via interactive maps, and analyze waste footprints through statistical dashboards.

---

## 🌟 Key Features

1. **AI Waste Scanner**: Classify waste using Groq's **Llama 3.3 (70B) model** or a local heuristics engine. Receive categories, hazard alerts, step-by-step sorting guidelines, and sustainable product suggestions.
2. **Interactive Dropoff Map**: Integrated **Leaflet.js** and **OpenStreetMap** with custom green, yellow, and red location pins indicating Recycling, E-waste, Compost, and Chemical collection stations. Panning, marker-cluster simulation, popups, and sidebar selection navigation are fully supported.
3. **Analytics Dashboard**: Multi-dimensional **Chart.js** visuals summarizing total logs, recyclable shares, daily activity line trends, and category distribution percentages.
4. **Resilient Local Mock Mode**: If Firebase credentials or Groq API keys are absent, the application automatically activates local mock modes (fallback databases + JSON-based filesystem databases), allowing immediate fully-functional offline testing.
5. **Settings Suite**: Configure service connectors, trigger test dataset seeding, reset the system database, and check health connectivity.

---

## 🛠️ Technology Stack

* **Frontend**: React.js (Vite, custom CSS, Lucide icons, Leaflet.js, Chart.js)
* **Backend**: Flask (Python 3, Flask-CORS, python-dotenv)
* **Databases**: Google Firebase Firestore (with fallback local JSON engine)
* **Generative AI**: Groq Cloud SDK (llama-3.3-70b-versatile)

---

## 📂 Project Structure

```text
waste-management-app/
├── README.md                       # Project guides & documentation
├── .gitignore                      # Git ignored folder lists
├── backend/
│   ├── app.py                      # Main Flask API router
│   ├── requirements.txt            # Python environment packages
│   ├── .env.template               # Environment variable config
│   ├── services/
│   │   ├── ai_service.py           # Groq AI classification client
│   │   ├── db_service.py           # Firebase database operations manager
│   │   └── mock_data.py            # Static listings of waste presets and stations
│   └── tests/
│       └── test_backend.py         # REST API test cases
└── frontend/
    ├── package.json                # Node configuration and dependencies
    ├── vite.config.js              # Vite server proxy configs
    ├── index.html                  # Core HTML file loading fonts and Leaflet CSS
    └── src/
        ├── main.jsx                # React DOM render entry
        ├── App.jsx                 # App shell coordination
        ├── index.css               # Dark-theme and glassmorphism styling sheet
        ├── components/             # Sub-components (Dashboard, Map, Scanner, etc.)
        └── services/
            └── api.js              # HTTP client methods for Flask endpoints
```

---

## 🚀 Installation & Local Run Guide

### 1. Prerequisite Checks
Ensure you have the following installed on your machine:
* Python 3.8 or higher
* Node.js (v16+) and npm

---

### 2. Configure Environment Variables
Inside the `backend/` folder, make a copy of the template:
```bash
cp backend/.env.template backend/.env
```
By default, the application runs in local fallback mode. To connect cloud APIs:
* Set `GROQ_API_KEY` to your Groq developer key.
* Set `USE_FIREBASE=true` and specify your Firebase account credentials file.

---

### 3. Run the Backend API Server
Navigate to the backend directory, configure a python virtual environment, install requirements, and start Flask:

On **macOS/Linux**:
```bash
cd backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python app.py
```

On **Windows (PowerShell)**:
```powershell
cd backend
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
python app.py
```
The server starts on `http://127.0.0.1:5000`. You can test it by visiting `/api/health` in your browser.

---

### 4. Run the Frontend Client
Navigate to the frontend folder, install packages, and start Vite dev server:
```bash
cd ../frontend
npm install
npm run dev
```
Open `http://localhost:5173` in your browser to view the application.

---

## 🧪 Testing

The backend includes a comprehensive suite of API test cases validating classification fallbacks, statistics compilations, and logs deletion.

To run tests:
```bash
cd backend
python -m unittest discover -s tests -p "test_*.py"
```

---

## 📄 REST API Endpoint Documentation

| Method | Endpoint | Description | Payloads / Query Parameters |
| :--- | :--- | :--- | :--- |
| **GET** | `/api/health` | Service status, active database engine, and Groq connection status. | None |
| **POST** | `/api/classify` | Submits a waste item name, returns category, hazard warning, and disposal list. | `{ "item": "banana peel" }` |
| **GET** | `/api/history` | Fetches historical scan logs sorted descending by date. | None |
| **POST** | `/api/history` | Manually inserts a waste log record into the history stack. | `{ "wasteType": "Bottle", ... }` |
| **DELETE** | `/api/history/<id>`| Removes a specific scan log from database by unique ID. | None |
| **GET** | `/api/statistics` | Computes daily scan frequencies, category shares, and recycling rate. | None |
| **GET** | `/api/centers` | Retrieves nearby dropoff facilities. Supports type filtering. | Optional: `?type=E-waste centers` |
| **POST** | `/api/settings/reset`| Wipes all documents from the database collection. | None |
