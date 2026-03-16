EcoRescue – AI Powered Disaster Response System

📌 Overview
EcoRescue is an AI-powered disaster response and resource allocation system designed to assist emergency response teams during natural disasters such as floods, earthquakes, and large-scale evacuations.
The system uses computer vision and intelligent resource allocation algorithms to detect people in affected zones and automatically assign shelter beds and volunteers in real time.
The platform consists of:
AI-based people detection
Automated volunteer assignment
Shelter resource management
Risk monitoring dashboard
The goal of EcoRescue is to reduce response time, improve coordination, and optimize resource distribution during emergencies.

🚀 Features

1️⃣ AI-based People Detection
Uses YOLOv8 object detection model
Detects number of people in uploaded images
Automatically records detection data in the database
The model used:
YOLOv8n (Ultralytics)

2️⃣ Intelligent Resource Allocation
After detecting people in a zone, the system:
Finds available shelters
Checks available beds
Assigns volunteers
Updates shelter occupancy automatically
This is handled by the AI Agent Logic in the backend.

3️⃣ Risk Level Monitoring
Each zone is dynamically classified into risk levels based on:
Number of detected people
Shelter overload
Detection frequency
Risk Levels:
Risk Score	Level	Color
0–50	Safe	Green
51–100	Caution	Yellow
101–200	Elevated	Orange
200+	Severe	Red

The system automatically updates zone risk status.
4️⃣ Emergency Dashboard
The dashboard provides real-time information such as:
Total detected people
Available shelter beds
Active volunteers
Critical zones
Zone-level statistics
Data is fetched through the /dashboard API.

🏗 System Architecture


          Camera / Image Upload
                    │
                    ▼
           AI Detection (YOLOv8)
                    │
                    ▼
           FastAPI Backend Server
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
   MySQL DB     AI Agent      Risk Engine
       │            │            │
       ▼            ▼            ▼
     Shelters   Volunteer     Zone Status
     Database   Assignment     Update
                    │
                    ▼
             React Dashboard
🛠 Tech Stack
Backend
*FastAPI
*Python
*YOLOv8 (Ultralytics)
*OpenCV
*NumPy
*MySQL

Frontend
*React
*TypeScript
*Axios
*Tailwind CSS
The frontend is built using Create React App and runs on localhost:3000.

📂 Project Structure

EcoRescue/
│
├── backend/
│   ├── main.py
│   ├── yolov8n.pt
│
├── frontend/
│   ├── src/
│   ├── package.json
│
├── database/
│
└── README.md

🗄 Database Design
Main Tables Used:
1️⃣ Zones
Stores disaster zone information.
Fields:
id
name
risk_level
color
last_update

2️⃣ Shelters
Stores shelter details.
Fields:
id
zone_id
available_beds

3️⃣ Volunteers
Stores volunteer information.
Fields:
id
zone_id
status
Status:
Available
Assigned

4️⃣ Detections
Stores AI detection results.
Fields:
id
zone_id
detected_people
location
detection_time

5️⃣ Assignments
Links volunteers and shelters to a detection event.
Fields:
volunteer_id
detection_id
shelter_id

1️⃣ Clone the Repository
git clone https://github.com/MayurKk777/ecorescue.git
cd ecorescue

🖥 Backend Setup (FastAPI)
2️⃣ Create a Python Virtual Environment
python -m venv venv

Activate it:
Windows
venv\Scripts\activate
Mac / Linux
source venv/bin/activate

3️⃣ Install Backend Dependencies
pip install fastapi
pip install uvicorn
pip install mysql-connector-python
pip install ultralytics
pip install opencv-python
pip install numpy

4️⃣ Run the Backend Server
Navigate to the backend folder (if applicable):
cd backend
Start the FastAPI server:
uvicorn main:app --reload
Backend will run at:
http://localhost:8000
API documentation will be available at:
http://localhost:8000/docs

🌐 Frontend Setup (React)

5️⃣ Navigate to Frontend Folder
cd frontend

6️⃣ Install Node Modules
npm install

7️⃣ Start the React Development Server
npm start
Frontend will run at:
http://localhost:3000

🗄 Database Setup (MySQL)
Install MySQL
Create a database named:
CREATE DATABASE ecorescue;
Update database credentials in main.py if needed:
db_config = {
    'host': 'localhost',
    'user': 'root',
    'password': 'rootkey',
    'database': 'ecorescue'
}

🚀 Running the Full System

Start services in this order:
1️⃣ Start MySQL Database
2️⃣ Run FastAPI Backend
uvicorn main:app --reload
3️⃣ Start React Frontend
npm start
Now open the system in your browser:
http://localhost:3000
