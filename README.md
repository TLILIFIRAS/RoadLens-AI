<div align="center">

# 🛣️ RoadLens AI — AI-Powered Pavement Inspection Platform

[![Demo Video](https://img.shields.io/badge/Demo-YouTube-ff0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=FpNz4Gg3nFs)
[![GitHub](https://img.shields.io/badge/GitHub-TLILIFIRAS-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/TLILIFIRAS)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Firas%20Tlili-0a66c2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/firastlili/)

An end-to-end AI-powered pavement inspection and reporting platform.
Upload road imagery, detect defects automatically with YOLO26, review AI detections, compute a PCI score, and export professional inspection reports — all in one collaborative web platform.

</div>

---

## 🔗 Links

| Resource | URL |
|----------|-----|
| 🎬 Demo Video | [https://www.youtube.com/watch?v=FpNz4Gg3nFs](https://www.youtube.com/watch?v=FpNz4Gg3nFs) |
| 💼 LinkedIn | [linkedin.com/in/firastlili](https://www.linkedin.com/in/firastlili/) |
| 🐙 GitHub | [github.com/TLILIFIRAS](https://github.com/TLILIFIRAS) |

---

## 🧠 Pipeline

```
Upload Images → YOLO26 Inference → OpenCV Annotation → PCI Calculation
→ Engineer Review (Confirm / Flag / Dismiss) → PDF / CSV Report Export
```

---

## ✨ Features

- 🤖 **AI Defect Detection** — YOLO26 detects cracks, potholes, and manholes with bounding boxes and confidence scores
- 📊 **PCI Scoring** — Density-based logarithmic Pavement Condition Index (0–100) computed automatically
- 🎚️ **Confidence Threshold** — Adjustable detection sensitivity per inspection (15%–75%)
- 🖼️ **Before/After Slider** — Drag to compare original and AI-annotated imagery side by side
- 🔍 **Detection Highlighting** — Click any detection to highlight its bounding box on the image
- 🗂️ **Image Gallery View** — Grid of all inspection images with defect count badges and lightbox
- ✅ **Engineer Review** — Confirm, flag, or dismiss each AI detection before finalizing
- 📝 **Engineer Notes** — Add inline notes per detection with full attribution tracking
- 🔄 **Bulk Re-analyze** — Re-run YOLO with a new threshold without creating a new inspection
- 📄 **PDF Reports** — Branded reports with annotated images, PCI score, detection tables, engineer notes
- 📋 **CSV Export** — Structured data export for engineering workflows
- 🗺️ **Map View** — Auto-geocoded project pins on OpenStreetMap, color-coded by PCI
- 📈 **Analytics Dashboard** — Defect distribution, PCI trend, inspection status charts (Recharts)
- 👥 **Multi-user Auth** — JWT-based login with three roles: Technicien, Engineer, Manager
- 🏢 **Team Collaboration** — Shared projects with full activity attribution in reports
- 💀 **Loading Skeletons** — Professional skeleton UI on all pages while data loads

---

## 🏆 Defect Classes

| Class | Severity | PCI Weight | Description |
|-------|----------|------------|-------------|
| `pothole` | High | × 1.0 | Surface depressions — immediate safety risk |
| `crack` | Medium | × 0.4 | Surface cracking — structural stress indicator |
| `manhole` | Low | × 0.1 | Manhole covers — infrastructure mapping |

---

## 📐 PCI Scoring Model

```
PCI = 100 − Σ ( 30 × log(1 + defects_per_image_type) × weight_type )
```

| Score | Rating |
|-------|--------|
| 85–100 | Excellent |
| 70–84 | Very Good |
| 55–69 | Good |
| 40–54 | Fair |
| 25–39 | Poor |
| 10–24 | Very Poor |
| 0–9 | Failed |

---

## 👥 User Roles

| Role | Permissions |
|------|-------------|
| **Technicien** | Upload images, create inspections, view results |
| **Engineer** | All above + confirm/flag/dismiss detections, add notes, export reports |
| **Manager** | Full access + manage team members, change roles, disable accounts |

---

## 🛠️ Tech Stack

### AI & Computer Vision

| Tool | Role |
|------|------|
| YOLO26 (Ultralytics) | Pavement defect detection — fine-tuned custom model |
| OpenCV (headless) | Image annotation, bounding box drawing |

### Backend

| Tool | Role |
|------|------|
| FastAPI 0.111 | REST API |
| SQLAlchemy 2.0 | ORM |
| SQLite | Zero-setup database |
| python-jose | JWT authentication |
| passlib + bcrypt | Password hashing |
| ReportLab 4.1 | PDF report generation |
| Pydantic v2 | Request/response validation |
| Uvicorn | ASGI server |

### Frontend

| Tool | Role |
|------|------|
| React 19 + TypeScript | UI framework |
| Vite 8 | Build tool |
| Tailwind CSS v4 | Styling |
| React Router DOM v7 | Routing |
| Axios | HTTP client with retry logic |
| Recharts | Analytics charts |
| Leaflet.js + react-leaflet | Interactive map |
| Lucide React | Icons |
| react-dropzone | File upload UI |

---

## 📁 Project Structure

```
/
├── backend/
│   ├── app/
│   │   ├── main.py                    # FastAPI app, CORS, routers
│   │   ├── api/
│   │   │   ├── auth.py                # Register, login, user management
│   │   │   ├── projects.py
│   │   │   ├── inspections.py         # Routes + background YOLO task
│   │   │   ├── detections.py          # Confirm / flag / dismiss + notes
│   │   │   └── reports.py             # PDF & CSV endpoints
│   │   ├── models/
│   │   │   ├── user.py                # User + UserRole enum
│   │   │   ├── project.py
│   │   │   ├── inspection.py
│   │   │   ├── image.py
│   │   │   └── detection.py
│   │   ├── schemas/                   # Pydantic request/response schemas
│   │   ├── services/
│   │   │   ├── detection.py           # YOLO inference + OpenCV annotation
│   │   │   ├── pci.py                 # PCI scoring + AI summary
│   │   │   └── reports.py            # PDF + CSV generation (ReportLab)
│   │   └── core/
│   │       ├── config.py
│   │       ├── database.py
│   │       └── security.py            # JWT + bcrypt
│   ├── uploads/
│   │   ├── originals/                 # Raw uploaded images
│   │   └── annotated/                 # YOLO-annotated output images
│   ├── best.pt                        # YOLO26 weights
│   ├── requirements.txt
│   └── Dockerfile
│
└── frontend/
    ├── src/
    │   ├── api/                       # Axios wrappers + retry client
    │   ├── context/
    │   │   └── AuthContext.tsx        # JWT auth state + provider
    │   ├── pages/
    │   │   ├── Home.tsx               # Landing page with video background
    │   │   ├── Login.tsx / Register.tsx
    │   │   ├── Dashboard.tsx          # Charts + stats
    │   │   ├── Projects.tsx / ProjectDetail.tsx / NewProject.tsx
    │   │   ├── Inspections.tsx / InspectionDetail.tsx / NewInspection.tsx
    │   │   ├── Reports.tsx
    │   │   └── MapView.tsx
    │   ├── components/
    │   │   ├── Layout.tsx             # Sidebar + user panel
    │   │   ├── ImageCompareSlider.tsx # Before/after drag slider + bbox highlight
    │   │   ├── AnalysisPanel.tsx      # Slide-in analysis progress panel
    │   │   ├── Skeleton.tsx           # Loading skeleton components
    │   │   ├── PCIBadge.tsx
    │   │   └── StatusBadge.tsx
    │   └── types/index.ts
    ├── public/
    │   └── home.mp4                   # Landing page background video
    └── vite.config.ts
```

---

## 🚀 Getting Started

### Prerequisites
- Python 3.10+ (Anaconda/Miniconda recommended)
- Node.js 18+
- `best.pt` YOLO26 model weights

### 1. Copy the model
```bash
copy best.pt backend\best.pt
```

### 2. Install backend dependencies
```bash
conda activate pytorch
cd backend
pip install -r requirements.txt
```

### 3. Install frontend dependencies
```bash
cd frontend
npm install
```

### 4. Start both servers with one command
```bash
cd frontend
npm run dev
```

| Service | URL |
|---------|-----|
| Frontend | http://localhost:5173 |
| Backend API | http://127.0.0.1:8000 |
| API Docs (Swagger) | http://127.0.0.1:8000/docs |

The SQLite database (`roadlens.db`) is created automatically on first run.

---

## 🔐 Environment Variables

Configure via `backend/.env`:

| Variable | Default | Description |
|----------|---------|-------------|
| `DATABASE_URL` | `sqlite:///./roadlens.db` | SQLAlchemy connection string |
| `UPLOAD_DIR` | `uploads` | Root directory for image storage |
| `MODEL_PATH` | `best.pt` | Path to YOLO weights |
| `APP_ENV` | `development` | Environment flag |

---

## 📡 API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/auth/register` | Create account |
| `POST` | `/api/auth/login` | Login — returns JWT |
| `GET` | `/api/auth/me` | Current user info |
| `GET` | `/api/auth/users` | List users *(manager only)* |
| `POST` | `/api/projects/` | Create project |
| `GET` | `/api/projects/` | List all projects |
| `DELETE` | `/api/projects/{id}` | Delete project + cascade |
| `POST` | `/api/inspections/` | Create inspection |
| `POST` | `/api/inspections/{id}/upload` | Upload images |
| `POST` | `/api/inspections/{id}/analyze` | Trigger YOLO analysis |
| `POST` | `/api/inspections/{id}/reanalyze` | Re-analyze with new threshold |
| `GET` | `/api/inspections/{id}/results` | Get detections + images |
| `PATCH` | `/api/detections/{id}` | Confirm / flag / dismiss + note |
| `GET` | `/api/reports/{id}/pdf` | Download PDF report |
| `GET` | `/api/reports/{id}/csv` | Download CSV report |

---

## 👤 Author

**Firas Tlili**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0a66c2?style=flat&logo=linkedin)](https://www.linkedin.com/in/firastlili/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github)](https://github.com/TLILIFIRAS)

---

<div align="center">
<sub>Made with ❤️ by Eng. Firas Tlili</sub>
</div>
