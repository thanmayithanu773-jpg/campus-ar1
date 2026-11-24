# CAMPUS AR NAVIGATION
> **Explore Your Campus. Navigate Smarter.**

A complete, production-grade college campus navigation web application engineered as a final-year Bachelor of Computer Applications (BCA) capstone project. The system seamlessly fuses high-accuracy GPS positioning, interactive Leaflet/OpenStreetMap routing, live camera-based Three.js Augmented Reality (AR), configurable indoor corridor pathway navigation powered by Dijkstra’s shortest path algorithm, and a secure administrator dashboard with JWT authentication.

---

## 🌟 Key Highlights & Restrictions Compliance

- 🚫 **Strictly No QR Codes**: No QR scanning, generation, or markers.
- 🚫 **No Video Walkthroughs / Prerecorded Clips**: Pure, live real-time camera feed and 3D spatial geometry.
- 📡 **Node.js & Express.js Backend**: Robust REST APIs, JWT authentication, and Socket.IO real-time telemetry.
- 🍃 **MongoDB Atlas & Mongoose**: Normalized schemas for locations, pathway routes, administrators, and navigation logs.
- 📐 **Preserved GPS Coordinates**: All 10 campus locations maintain their exact latitude and longitude down to 6 decimal places.
- 🧭 **Three.js AR Directional Arrows**: Real-time 3D spatial arrows that dynamically rotate towards the destination relative to the user's compass bearing.
- 🏛️ **Camera-Based Visual Positioning**: Architectural landmark feature recognition without artificial barcodes or tags.
- 🛣️ **Indoor Pathway Navigation**: True topological corridor routing preventing straight-line wall traversal.

---

## 🏗️ Architecture & Technology Stack

| Layer | Technologies |
|---|---|
| **Frontend** | HTML5, CSS3 (Premium Glassmorphism & Cyber Navy Theme), Vanilla JavaScript (ES6+) |
| **Augmented Reality** | Three.js (r128), Device Orientation API, Camera MediaDevices API, WebXR API check |
| **Interactive Mapping** | Leaflet.js (v1.9.4), keyless OpenStreetMap raster tiles with a CSS dark-theme filter |
| **Backend** | Node.js (v20+), Express.js (v4.19+), Socket.IO (v4.7+) |
| **Database** | MongoDB Atlas, Mongoose (v8.3+) |
| **Security & Auth** | JSON Web Tokens (JWT), Bcrypt.js password hashing, CORS |
| **Algorithms** | Dijkstra's Shortest Path Algorithm, Spherical Haversine Geodesy |

---

## 📍 Predefined Campus Locations (Exact Coordinates)

All 10 predefined locations are automatically seeded into MongoDB on first boot:

| Location Name | Latitude | Longitude | Category | Building | Floor |
|---|---|---|---|---|---|
| **Wash Room** | `13.337356` | `77.131155` | Restroom | Main Academic Block | Ground Floor |
| **3rd Year BCA Classroom** | `13.336996` | `77.130134` | Academic | Computer Applications Wing | 2nd Floor |
| **2nd Year BCA Classroom** | `13.336973` | `77.130062` | Academic | Computer Applications Wing | 1st Floor |
| **1st Year BCA Classroom** | `13.336754` | `77.129898` | Academic | Computer Applications Wing | Ground Floor |
| **Seminar Hall** | `13.336954` | `77.130111` | Auditorium | Central Auditorium Complex | Ground Floor |
| **Staff Room** | `13.336816` | `77.130138` | Faculty | Main Academic Block | 1st Floor |
| **Meeting Room** | `13.337001` | `77.130134` | Administrative | Administrative Wing | 1st Floor |
| **Principal Office** | `13.337017` | `77.130165` | Administrative | Administrative Wing | Ground Floor |
| **Library** | `13.337043` | `77.130291` | Resource Center | Central Library Block | Ground Floor |
| **Academic Director Office Room** | `13.336810` | `77.130165` | Administrative | Administrative Wing | 1st Floor |

---

## 🚀 Quick Start & Installation

### Prerequisites
- [Node.js](https://nodejs.org/) (version 18 or 20+ LTS)
- [npm](https://www.npmjs.com/) (version 9+)
- (Optional) [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) account or local MongoDB server. *(If MongoDB is offline, the backend seamlessly falls back to the in-memory engine with JSON persistence so you can test immediately without any database setup!)*

### 1. Clone & Install Dependencies
```bash
git clone <repository-url> campus-ar-navigation
cd campus-ar-navigation
npm install
```

### 2. Configure Environment Variables
Copy `.env.example` to create your active `.env`:
```bash
cp .env.example .env
```
Default configuration values:
```env
PORT=5001
NODE_ENV=development
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/campus_ar_navigation?retryWrites=true&w=majority
JWT_SECRET=campus_ar_super_secret_jwt_key_2026_bca_prod
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admincampus2026
ARRIVAL_RADIUS_METERS=5
GPS_ACCURACY_THRESHOLD_METERS=20
```

### 3. Initialize & Seed Database
```bash
npm run seed
```
*(Note: Starting the server also automatically runs the database seeder if collections are uninitialized).*

### 4. Launch Application Server
```bash
npm start
```
Open your web browser and navigate to:
```
http://localhost:5001
```

---

## 📱 Navigation Modes

### 1. GPS Map Navigation
- Connects to the browser's high-accuracy Geolocation API via `navigator.geolocation.watchPosition`.
- Renders an interactive Leaflet dark matrix map centered over the college campus.
- Computes Haversine distances and estimated walking durations in real time.
- Renders glowing neon pathway polylines based on actual corridor connections.
- Alerts user upon arriving within the 5-meter destination radius.

### 2. Live AR Navigation
- Activates device rear camera using `navigator.mediaDevices.getUserMedia({ video: { facingMode: 'environment' } })`.
- Utilizes Three.js to render a 3D cyan cyber-directional arrow over the live camera viewport.
- Reads compass azimuth via `DeviceOrientationEvent` (`webkitCompassHeading` on iOS, `alpha` on Android).
- Dynamically orientates the 3D arrow to physically point towards the next corridor waypoint.
- Integrates a real-time compass tape display, distance counter, and step-by-step turn instructions.

### 3. Camera-Based Visual Positioning (No QR Codes)
- Features real-time landmark recognition analyzing camera frames against registered campus architectural features (e.g., *Library Security Gate*, *Principal Brass Nameplate*, *Room 302 Brass Plaque*, *BCA Department Banner*).
- Displays a visual landmark tracking reticle and a confidence meter (e.g., `85% Confidence - Landmark Locked`).
- Automatically transitions between visual feature tracking and GPS-guided dead reckoning when visual confidence fluctuates.

### 4. Indoor Pathway Navigation
- Uses Dijkstra’s shortest path algorithm implemented in `backend/utils/dijkstra.js`.
- Respects walls, staircases, and physical doors, preventing impossible straight-line walking routes.
- Administrators can configure new pathways and corridor segments dynamically via the admin panel.

---

## 🔐 Administrator Dashboard

### Default Credentials:
- **Username**: `admin`
- **Password**: `admincampus2026`

### Capabilities:
- **Overview Metrics**: Total locations, active destinations, registered pathways, and navigation queries.
- **Location Management**: Add, edit, or delete campus locations with instant coordinate validation.
- **Pathway Graph Configurator**: Create bi-directional walking connections between rooms and corridors.
- **Live Telemetry & Logs**: Real-time navigation requests and arrival events tracked via Socket.IO.

---

## 📡 REST API Reference

### Public APIs

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/health` | System health check and status |
| `GET` | `/api/locations` | Returns all 10 campus locations |
| `GET` | `/api/locations/:id` | Returns details for a single campus location |
| `GET` | `/api/routes` | Returns all indoor & outdoor corridor connections |
| `POST` | `/api/navigation/calculate` | Computes Dijkstra shortest path and turn-by-turn guidance |
| `POST` | `/api/navigation/log` | Logs navigation start or destination arrival |
| `GET` | `/api/dashboard` | Returns summary statistics and recent activity logs |

### Protected Administrator APIs (Requires Bearer JWT)

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/admin/login` | Authenticates administrator and returns JWT |
| `GET` | `/api/admin/profile` | Returns authenticated admin details |
| `POST` | `/api/locations` | Adds a new campus location |
| `PUT` | `/api/locations/:id` | Updates an existing campus location |
| `DELETE` | `/api/locations/:id` | Deletes a campus location |
| `POST` | `/api/routes` | Creates a new pathway corridor edge |
| `PUT` | `/api/routes/:id` | Modifies an existing pathway edge |
| `DELETE` | `/api/routes/:id` | Deletes a pathway edge |

---

## 🚢 Deployment Guide

### Database: MongoDB Atlas
1. Create a free M0 cluster on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
2. Create a database user and whitelist IP `0.0.0.0/0` (or your hosting IP).
3. Copy the connection URI: `mongodb+srv://<user>:<password>@cluster0.xxx.mongodb.net/campus_ar_navigation?retryWrites=true&w=majority`.

### Backend: Render.com
1. Create a new **Web Service** pointing to your repository.
2. Set Environment to **Node**.
3. Build Command: `npm install`
4. Start Command: `node backend/server.js`
5. Add Environment Variables:
   - `MONGODB_URI`: `<Your MongoDB Atlas URI>`
   - `JWT_SECRET`: `<Secure Random String>`
   - `NODE_ENV`: `production`

### Frontend: Vercel / Integrated Express Serving
- The application is configured as a full-stack integrated application. When running on Render, the Node.js server automatically serves the `frontend/` static bundle.
- Alternatively, deploy the `frontend/` folder directly to [Vercel](https://vercel.com/) and configure the API base URL to your Render backend.

---

## 🧪 Hardware & Device Compatibility Notice

- **Camera & Live AR**: Mobile devices running iOS Safari (iOS 13+) or Android Google Chrome require an HTTPS connection (or `localhost`) to grant camera and device orientation sensor permissions.
- **Desktop Browsers**: When tested on desktop laptops without rear-facing cameras, the AR engine automatically activates the cyber-spatial simulator mode, permitting full testing of 3D arrows, destination changes, and turn guidance.
- **GPS Indoors**: Deep indoor building areas may experience GPS signal attenuation; the integrated Camera-Based Visual Positioning feature and indoor pathway graph resolve indoor wayfinding.
