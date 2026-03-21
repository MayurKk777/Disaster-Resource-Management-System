# 🌍 EcoRescue — AI-Powered Disaster Response System

An intelligent disaster response and resource allocation platform that combines **computer vision** and **automated resource management** to assist emergency teams during natural disasters. The system detects people in affected zones using YOLOv8, then automatically assigns shelters and volunteers in real time.

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [System Architecture](#-system-architecture)
- [Database Schema](#-database-schema)
- [API Endpoints](#-api-endpoints)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Risk Classification](#-risk-classification)
- [License](#-license)

---

## ✨ Features

### 🤖 AI-Based People Detection
- Uses **YOLOv8n** (Ultralytics) for real-time object detection
- Detects the number of people in uploaded images from disaster zones
- Automatically records detection data with timestamps and location

### 🧠 Intelligent Resource Allocation
After detecting people in a zone, the system's **AI Agent** automatically:
- Finds available shelters in the affected area
- Checks available bed capacity
- Assigns the nearest available volunteers
- Updates shelter occupancy in real time

### 📊 Dynamic Risk Monitoring
Each disaster zone is classified into risk levels based on:
- Number of detected people
- Shelter overload status
- Detection frequency

### 🖥 Emergency Dashboard
A real-time React dashboard displaying:
- Total detected people across all zones
- Available shelter beds
- Active volunteer count
- Critical zone alerts
- Zone-level statistics with color-coded risk indicators

---

## 🛠 Tech Stack

| Layer        | Technology                                                     |
| ------------ | -------------------------------------------------------------- |
| **Backend**  | Python 3, FastAPI, Uvicorn                                     |
| **AI/ML**    | YOLOv8 (Ultralytics), OpenCV, NumPy                           |
| **Frontend** | React, TypeScript, Tailwind CSS, Axios                         |
| **Database** | MySQL                                                          |

---

## 🏗 System Architecture

```
┌─────────────────────────┐
│   Camera / Image Upload │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│   AI Detection (YOLOv8) │
│   People counting &     │
│   zone classification   │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│   FastAPI Backend        │
│   REST API Server        │
└────┬───────┬────────┬───┘
     │       │        │
     ▼       ▼        ▼
┌────────┐ ┌──────┐ ┌──────────┐
│ MySQL  │ │  AI  │ │   Risk   │
│   DB   │ │Agent │ │  Engine  │
└────┬───┘ └──┬───┘ └────┬─────┘
     │        │          │
     ▼        ▼          ▼
┌────────┐ ┌────────┐ ┌──────────┐
│Shelter │ │Voluntr │ │  Zone    │
│Mgmt    │ │Assign  │ │  Status  │
└────────┘ └────────┘ └──────────┘
             │
             ▼
┌─────────────────────────┐
│   React Dashboard       │
│   Real-time monitoring  │
└─────────────────────────┘
```

---

## 🗄 Database Schema

### `zones`
| Column       | Type     | Description                          |
| ------------ | -------- | ------------------------------------ |
| id           | INT (PK) | Zone identifier                      |
| name         | VARCHAR  | Zone name                            |
| risk_level   | VARCHAR  | Current risk classification          |
| color        | VARCHAR  | UI color code (green/yellow/orange/red) |
| last_update  | DATETIME | Last status update timestamp         |

### `shelters`
| Column         | Type     | Description                        |
| -------------- | -------- | ---------------------------------- |
| id             | INT (PK) | Shelter identifier                |
| zone_id        | INT (FK) | Associated zone                   |
| available_beds | INT      | Number of available beds          |

### `volunteers`
| Column | Type     | Description                              |
| ------ | -------- | ---------------------------------------- |
| id     | INT (PK) | Volunteer identifier                     |
| zone_id| INT (FK) | Assigned zone                            |
| status | ENUM     | `Available` or `Assigned`                |

### `detections`
| Column          | Type      | Description                          |
| --------------- | --------- | ------------------------------------ |
| id              | INT (PK)  | Detection record identifier          |
| zone_id         | INT (FK)  | Zone where detection occurred        |
| detected_people | INT       | Number of people detected            |
| location        | VARCHAR   | Specific location within zone        |
| detection_time  | DATETIME  | Timestamp of detection               |

### `assignments`
| Column        | Type     | Description                            |
| ------------- | -------- | -------------------------------------- |
| volunteer_id  | INT (FK) | Assigned volunteer                     |
| detection_id  | INT (FK) | Related detection event                |
| shelter_id    | INT (FK) | Assigned shelter                       |

---

## 🔌 API Endpoints

### Detection
| Method | Endpoint        | Description                              |
| ------ | --------------- | ---------------------------------------- |
| POST   | `/detect`       | Upload image for AI people detection     |

### Zones
| Method | Endpoint        | Description                              |
| ------ | --------------- | ---------------------------------------- |
| GET    | `/zones`        | List all disaster zones with risk levels |

### Shelters
| Method | Endpoint        | Description                              |
| ------ | --------------- | ---------------------------------------- |
| GET    | `/shelters`     | List all shelters with bed availability  |

### Volunteers
| Method | Endpoint        | Description                              |
| ------ | --------------- | ---------------------------------------- |
| GET    | `/volunteers`   | List all volunteers with status          |

### Dashboard
| Method | Endpoint        | Description                              |
| ------ | --------------- | ---------------------------------------- |
| GET    | `/dashboard`    | Aggregated KPIs for the emergency dashboard |

> 📖 Full interactive API docs available at `http://localhost:8000/docs` (Swagger UI)

---

## 🚀 Getting Started

### Prerequisites

- **Python 3.10+**
- **Node.js 18+**
- **MySQL** running locally
- **npm**

### 1. Clone the Repository

```bash
git clone https://github.com/MayurKk777/Disaster-Resource-Management-System.git
cd Disaster-Resource-Management-System
```

### 2. Set Up the Database (MySQL)

```sql
CREATE DATABASE ecorescue;
```

Update database credentials in `backend/main.py` if needed:

```python
db_config = {
    'host': 'localhost',
    'user': 'root',
    'password': 'rootkey',
    'database': 'ecorescue'
}
```

### 3. Set Up the Backend

```bash
cd backend

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate        # macOS / Linux
# venv\Scripts\activate         # Windows

# Install dependencies
pip install fastapi uvicorn mysql-connector-python ultralytics opencv-python numpy
```

### 4. Set Up the Frontend

```bash
cd frontend
npm install
```

### 5. Run the Full System

Start services in this order:

```bash
# 1. Ensure MySQL is running

# 2. Start the FastAPI backend
cd backend
source venv/bin/activate
uvicorn main:app --reload
# → http://localhost:8000

# 3. Start the React frontend (new terminal)
cd frontend
npm start
# → http://localhost:3000
```

### 6. Access the Application

| Service            | URL                          |
| ------------------ | ---------------------------- |
| Frontend Dashboard | http://localhost:3000         |
| Backend API        | http://localhost:8000         |
| API Docs (Swagger) | http://localhost:8000/docs    |

---

## 📁 Project Structure

```
Disaster-Resource-Management-System/
├── backend/
│   ├── main.py              # FastAPI app with all endpoints
│   └── yolov8n.pt           # YOLOv8 pre-trained model weights
│
├── frontend/
│   ├── src/
│   │   ├── components/      # React UI components
│   │   ├── App.tsx           # Main application component
│   │   └── index.tsx         # Entry point
│   ├── package.json
│   └── tsconfig.json
│
├── database/                 # Database scripts / schemas
└── README.md
```

---

## 🚦 Risk Classification

The system dynamically classifies zones into risk levels:

| Risk Score | Level      | Color    | Action                              |
| ---------- | ---------- | -------- | ----------------------------------- |
| 0 – 50     | ✅ Safe     | 🟢 Green  | Normal monitoring                   |
| 51 – 100   | ⚠️ Caution | 🟡 Yellow | Increased surveillance              |
| 101 – 200  | 🔶 Elevated| 🟠 Orange | Deploy additional resources         |
| 200+       | 🔴 Severe  | 🔴 Red    | Full emergency response activated   |

Risk scores are calculated based on:
- **Detection count** — Number of people detected in the zone
- **Shelter capacity** — Remaining bed availability
- **Detection frequency** — How often new detections are happening

---

## 🔮 Future Enhancements

- [ ] Real-time video stream detection (RTSP/webcam)
- [ ] Drone integration for aerial zone monitoring
- [ ] Push notifications for critical zone alerts
- [ ] Historical analytics and trend reporting
- [ ] Multi-agency coordination features
- [ ] Mobile app for field volunteers

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

<p align="center">
  Built with ❤️ by <a href="https://github.com/MayurKk777">Mayur</a>
</p>
