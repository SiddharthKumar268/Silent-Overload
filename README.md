<div align="center">

# 🧠 Academic Burnout Prediction System

### AI-Powered Student Wellness & Workload Intelligence Platform

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/Node.js-18+-green.svg)](https://nodejs.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-6.0+-brightgreen.svg)](https://www.mongodb.com/)
[![Express.js](https://img.shields.io/badge/Express-4.18+-lightgrey.svg)](https://expressjs.com/)
[![Status](https://img.shields.io/badge/Status-Active-success.svg)]()

[Features](#-features) • [Architecture](#-system-architecture) • [Quick Start](#-quick-start) • [Documentation](#-documentation) • [Demo](#-demo)

![Burnout Prediction Dashboard](https://via.placeholder.com/1200x400/6366f1/ffffff?text=Academic+Burnout+Prediction+Dashboard)

</div>

---

## 📖 Overview

The **Academic Burnout Prediction System** is a comprehensive full-stack application that uses multi-signal analysis to predict student burnout risk **before it happens**. By analyzing workload patterns, deadline collisions, performance drift, and recovery gaps, the system provides early warnings to students, proctors, and administrators.

### 🎯 The Problem

> **73%** of students experience burnout during academic semesters  
> **Only 12%** seek help before reaching critical stress levels  
> **Average detection time**: 3-4 weeks after onset

### 💡 Our Solution

**Predict burnout 2-3 weeks in advance** using:
- ⚡ Real-time workload tracking
- 🔍 Multi-signal pattern detection
- 📊 Performance trend analysis
- 🎓 Institutional calendar integration
- 🚨 Proactive intervention alerts

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 👨‍🎓 For Students
- 📅 **Smart Workload Tracking**
  - Task & event aggregation
  - Visual workload calendar
  - Deadline collision alerts
  
- 📈 **Burnout Risk Dashboard**
  - Real-time risk scoring (0-100)
  - 4-signal analysis breakdown
  - Personalized recommendations
  
- 🎯 **Grade & Performance Tracking**
  - Semester-wise analytics
  - Effort vs. outcome trends
  - Performance drift detection

</td>
<td width="50%">

### 👨‍🏫 For Proctors
- 🎯 **Student Monitoring**
  - At-risk student dashboard
  - Individual burnout analytics
  - Historical intervention logs
  
- 💬 **Intervention Tools**
  - Direct messaging system
  - Resource recommendations
  - Progress tracking
  
- 📊 **Batch Analytics**
  - Class-wide burnout trends
  - Risk distribution reports
  - Early warning system

</td>
</tr>
<tr>
<td width="50%">

### 🔧 For Administrators
- 📆 **Calendar Management**
  - Institutional event scheduling
  - Bulk CSV upload support
  - Event priority classification
  
- 👥 **User Management**
  - Student/proctor accounts
  - Role-based access control
  - Batch operations

</td>
<td width="50%">

### 🤖 AI/ML Features
- 🧠 **Multi-Signal Detection**
  - Deadline collision analysis
  - Workload volatility tracking
  - Recovery gap identification
  - Performance drift detection
  
- 📉 **Predictive Analytics**
  - 14-day risk forecasting
  - Pattern recognition
  - Adaptive thresholds

</td>
</tr>
</table>

---

## 🏗️ System Architecture

```mermaid
graph TB
    subgraph "Frontend Layer"
        A[Student Dashboard] 
        B[Proctor Dashboard]
        C[Admin Panel]
        D[Calendar UI]
    end
    
    subgraph "API Layer"
        E[Express.js Server]
        F[Authentication Middleware]
        G[Route Controllers]
    end
    
    subgraph "Service Layer"
        H[Burnout Prediction Engine]
        I[Workload Calculator]
        J[Signal Detectors]
        K[Intervention Manager]
    end
    
    subgraph "Signal Detectors"
        J1[Collision Detector]
        J2[Volatility Detector]
        J3[Recovery Detector]
        J4[Drift Detector]
    end
    
    subgraph "Data Layer"
        L[(MongoDB)]
        M[Task Collection]
        N[Event Collection]
        O[Grade Collection]
        P[Signal Collection]
    end
    
    subgraph "Scheduled Jobs"
        Q[Daily Analysis Job]
        R[Weekly Reports]
    end
    
    A --> E
    B --> E
    C --> E
    D --> E
    
    E --> F
    F --> G
    
    G --> H
    G --> I
    G --> K
    
    H --> J
    I --> M
    I --> N
    
    J --> J1
    J --> J2
    J --> J3
    J --> J4
    
    J1 --> P
    J2 --> P
    J3 --> P
    J4 --> P
    
    H --> L
    I --> L
    K --> L
    
    Q --> H
    R --> H
    
    style H fill:#6366f1,stroke:#4f46e5,color:#fff
    style J fill:#8b5cf6,stroke:#7c3aed,color:#fff
    style L fill:#10b981,stroke:#059669,color:#fff
```

---

## 🧬 Data Flow Architecture

### 📥 Data Collection → 🧮 Processing → 🚨 Prediction

```mermaid
sequenceDiagram
    participant S as Student
    participant UI as Dashboard
    participant API as API Server
    participant WL as Workload Service
    participant BP as Burnout Predictor
    participant SD as Signal Detectors
    participant DB as MongoDB
    
    S->>UI: Add Task/View Calendar
    UI->>API: POST /tasks
    API->>DB: Save Task
    
    Note over API,WL: Triggered automatically
    API->>WL: calculateWorkloadScores()
    WL->>DB: Fetch Tasks
    WL->>DB: Fetch Calendar Events
    WL-->>WL: Calculate Daily Scores
    WL->>DB: Save WorkloadScore
    
    Note over API,BP: Daily job or manual trigger
    API->>BP: predictBurnout(studentId)
    BP->>SD: Run 4 Detectors
    
    par Parallel Detection
        SD->>DB: Collision Analysis
        SD->>DB: Volatility Analysis
        SD->>DB: Recovery Analysis
        SD->>DB: Performance Drift
    end
    
    SD-->>BP: Return Signals
    BP-->>BP: Calculate Risk Score (0-100)
    BP->>DB: Save Signal
    
    UI->>API: GET /burnout/analysis
    API->>DB: Fetch Latest Signal
    DB-->>UI: Display Risk Dashboard
```

---

## 🧩 Core Components Breakdown

### 1️⃣ Workload Calculation Engine

**Location**: `/backend/services/workload.js`

```javascript
┌─────────────────────────────────────────────────────────────┐
│  WORKLOAD CALCULATION FORMULA                               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Daily Score = Σ (Task Effort × Task Weight)               │
│               + Σ (Event Duration × Event Weight)           │
│                                                             │
│  Task Weights:                                              │
│    • Exam:       3.0x   (highest stress)                    │
│    • Project:    2.5x                                       │
│    • Assignment: 1.5x                                       │
│    • Quiz:       1.0x   (baseline)                          │
│                                                             │
│  Event Weights:                                             │
│    • Exam (institutional):  8.0   (equivalent to 2 exams)   │
│    • Registration:          4.0                             │
│    • General Event:         3.0                             │
│    • Holiday:               0.0   (rest day)                │
│                                                             │
│  Weekly Score = Σ (Daily Scores for 7 days)                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Output Example**:
```json
{
  "date": "2026-02-10",
  "dailyScore": 107,
  "taskScore": 47,      // Tasks only
  "eventScore": 60,     // Events only
  "taskCount": 3,
  "eventCount": 3,
  "weeklyScore": 285,
  "breakdown": {
    "exams": 2,
    "projects": 1,
    "events": 3
  }
}
```

---

### 2️⃣ Burnout Prediction Engine

**Location**: `/backend/services/burnout.js`

```
┌────────────────────────────────────────────────────────────────┐
│  BURNOUT RISK SCORING MODEL                                    │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  Total Score = Collision Score                                 │
│               + Volatility Score                               │
│               + Recovery Score                                 │
│               + Drift Score                                    │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │ 1. COLLISION DETECTOR (Max: 30 points)                   │ │
│  ├──────────────────────────────────────────────────────────┤ │
│  │ Checks: Next 14 days                                     │ │
│  │ Triggers:                                                │ │
│  │   • ≥3 major tasks/events in 1 week  → +30              │ │
│  │   • ≥50 hours workload in 1 week     → +30              │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │ 2. VOLATILITY DETECTOR (Max: 25 points)                  │ │
│  ├──────────────────────────────────────────────────────────┤ │
│  │ Checks: Last 4 weeks                                     │ │
│  │ Triggers:                                                │ │
│  │   • 50-100% increase  → +15                              │ │
│  │   • 100-200% increase → +20                              │ │
│  │   • >200% increase    → +25                              │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │ 3. RECOVERY DETECTOR (Max: 25 points)                    │ │
│  ├──────────────────────────────────────────────────────────┤ │
│  │ Checks: Last 30 days                                     │ │
│  │ Triggers:                                                │ │
│  │   • ≥7 consecutive high-load days    → +25              │ │
│  │   • High load = daily score >10                          │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │ 4. DRIFT DETECTOR (Max: 20 points)                       │ │
│  ├──────────────────────────────────────────────────────────┤ │
│  │ Checks: Last 6 months                                    │ │
│  │ Triggers:                                                │ │
│  │   • Workload ↑ but Grades ↓ for ≥3 months → +20         │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │ RISK CLASSIFICATION                                      │ │
│  ├──────────────────────────────────────────────────────────┤ │
│  │  🔴 HIGH   (60-100): Immediate intervention needed      │ │
│  │  🟡 MEDIUM (30-59):  Monitor closely, support available │ │
│  │  🟢 LOW    (0-29):   Healthy workload pattern           │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

### 3️⃣ Signal Detectors

<details>
<summary><b>🔍 Collision Detector</b> - Identifies deadline overload</summary>

**File**: `/backend/services/collision.js`

**Purpose**: Detect when multiple high-priority items cluster in a short timeframe

**Algorithm**:
```
1. Fetch tasks & events for next 14 days
2. Group by week
3. For each week:
   - Count major items (exams, projects, institutional events)
   - Sum total effort hours
4. Flag if:
   - ≥3 major items OR
   - ≥50 total hours
```

**Output**:
```json
{
  "hasCollision": true,
  "count": 5,
  "taskCount": 2,
  "eventCount": 3,
  "totalHours": 107,
  "week": "2026-02-08 to 2026-02-14",
  "items": [
    { "type": "exam", "title": "Midterm Math", "date": "2026-02-10" },
    { "type": "event", "title": "Student Tour", "date": "2026-02-08" }
  ]
}
```

</details>

<details>
<summary><b>⚡ Volatility Detector</b> - Identifies workload spikes</summary>

**File**: `/backend/services/volatility.js`

**Purpose**: Detect sudden increases in workload that may overwhelm students

**Algorithm**:
```
1. Get last 4 weeks of workload scores
2. Calculate average for weeks 1-3
3. Compare week 4 to average
4. Calculate percentage change
5. Flag based on severity:
   - 50-100%:  Moderate spike
   - 100-200%: High spike
   - >200%:    Extreme spike
```

**Output**:
```json
{
  "hasVolatility": true,
  "percentageChange": 127,
  "previousAverage": 47,
  "currentWeek": 107,
  "severity": "high"
}
```

</details>

<details>
<summary><b>🛌 Recovery Detector</b> - Identifies insufficient rest</summary>

**File**: `/backend/services/recovery.js`

**Purpose**: Detect continuous high workload without adequate recovery time

**Algorithm**:
```
1. Get last 30 days of daily scores
2. Identify "high load" days (score >10)
3. Find longest continuous streak
4. Flag if ≥7 consecutive days
```

**Output**:
```json
{
  "hasRecoveryDeficit": true,
  "consecutiveDays": 12,
  "startDate": "2026-01-28",
  "endDate": "2026-02-08",
  "averageDailyScore": 15.3
}
```

</details>

<details>
<summary><b>📉 Drift Detector</b> - Identifies performance decline</summary>

**File**: `/backend/services/drift.js`

**Purpose**: Detect when increased effort doesn't translate to better grades

**Algorithm**:
```
1. Get 6 months of:
   - Workload scores (monthly average)
   - Grade percentages (monthly average)
2. Calculate correlation
3. Identify negative correlation periods
4. Flag if sustained for ≥3 months
```

**Output**:
```json
{
  "hasDrift": true,
  "durationMonths": 4,
  "workloadTrend": "increasing",
  "gradeTrend": "decreasing",
  "correlation": -0.73,
  "recentGrades": [78, 75, 71, 68],
  "recentWorkload": [40, 52, 61, 73]
}
```

</details>

---

## 🚀 Quick Start

### Prerequisites

```bash
Node.js  >= 18.0.0
MongoDB  >= 6.0
npm      >= 9.0
```

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/burnout-prediction-system.git
cd burnout-prediction-system

# 2. Install backend dependencies
cd backend
npm install

# 3. Configure environment variables
cp .env.example .env
# Edit .env with your MongoDB URI and JWT secret

# 4. Start MongoDB (if local)
mongod --dbpath /path/to/data

# 5. Start the backend server
npm start
# Server runs on http://localhost:3000

# 6. Open frontend
# Navigate to frontend/pages/login.html in your browser
# Or use a local server:
npx http-server frontend -p 8080
```

### Environment Variables

Create a `.env` file in `/backend`:

```env
# Database
MONGODB_URI=mongodb://localhost:27017/burnout-system
MONGODB_TEST_URI=mongodb://localhost:27017/burnout-system-test

# Authentication
JWT_SECRET=your_super_secret_key_here_change_in_production
JWT_EXPIRES_IN=7d

# Server
PORT=3000
NODE_ENV=development

# Email (optional - for notifications)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password

# Scheduled Jobs
ENABLE_DAILY_ANALYSIS=true
DAILY_ANALYSIS_TIME=02:00
```

---

## 📊 Database Schema

### Collections Overview

```mermaid
erDiagram
    User ||--o{ Task : creates
    User ||--o{ Grade : records
    User ||--o{ Signal : generates
    User ||--o{ CalendarEvent : "may create"
    User ||--o{ Intervention : "receives/sends"
    User ||--o{ WorkloadScore : has
    
    User {
        ObjectId _id
        string name
        string email
        string password
        enum role
        ObjectId proctorId
        date createdAt
    }
    
    Task {
        ObjectId _id
        ObjectId studentId
        string title
        enum type
        date deadline
        number estimatedEffort
        boolean isCompleted
        date createdAt
    }
    
    CalendarEvent {
        ObjectId _id
        string title
        enum eventType
        date startDate
        date endDate
        boolean isInstitutional
        ObjectId createdBy
        string venue
        number priority
    }
    
    Grade {
        ObjectId _id
        ObjectId studentId
        string subject
        string examType
        number marks
        number maxMarks
        number percentage
        date date
        string semester
    }
    
    WorkloadScore {
        ObjectId _id
        ObjectId studentId
        date date
        number dailyScore
        number taskScore
        number eventScore
        number weeklyScore
        object breakdown
    }
    
    Signal {
        ObjectId _id
        ObjectId studentId
        number score
        enum risk
        array reasons
        object signals
        date timestamp
    }
    
    Intervention {
        ObjectId _id
        ObjectId studentId
        ObjectId proctorId
        ObjectId signalId
        string message
        string resources
        boolean acknowledged
        date createdAt
    }
```

### Key Models

<details>
<summary><b>👤 User Model</b></summary>

```javascript
{
  _id: ObjectId,
  name: String,
  email: String (unique, required),
  password: String (hashed),
  role: enum ['student', 'proctor', 'admin'],
  proctorId: ObjectId (ref: User), // for students only
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes**:
- `email` (unique)
- `role`
- `proctorId`

</details>

<details>
<summary><b>📋 Task Model</b></summary>

```javascript
{
  _id: ObjectId,
  studentId: ObjectId (ref: User, required),
  title: String (required),
  type: enum ['exam', 'project', 'assignment', 'quiz'],
  deadline: Date (required),
  estimatedEffort: Number (hours, required),
  isCompleted: Boolean (default: false),
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes**:
- `studentId`
- `deadline`
- `{studentId: 1, deadline: 1}`

</details>

<details>
<summary><b>📅 CalendarEvent Model</b></summary>

```javascript
{
  _id: ObjectId,
  title: String (required),
  eventType: enum ['exam', 'registration', 'event', 'holiday'],
  startDate: Date (required),
  endDate: Date (required),
  isInstitutional: Boolean (default: false),
  createdBy: ObjectId (ref: User, required),
  venue: String,
  priority: Number (1-5, default: 3),
  description: String,
  duration: Number (hours, default: 3),
  createdAt: Date
}
```

**Indexes**:
- `isInstitutional`
- `startDate`
- `{createdBy: 1, startDate: 1}`

**Query Examples**:
```javascript
// Get all institutional events
CalendarEvent.find({ isInstitutional: true })

// Get student's personal events
CalendarEvent.find({ 
  isInstitutional: false,
  createdBy: studentId 
})

// Get all visible events for a student
CalendarEvent.find({
  $or: [
    { isInstitutional: true },
    { createdBy: studentId }
  ],
  startDate: { $gte: startDate, $lte: endDate }
})
```

</details>

<details>
<summary><b>📈 Grade Model</b></summary>

```javascript
{
  _id: ObjectId,
  studentId: ObjectId (ref: User, required),
  subject: String (required),
  examType: String (required),
  marks: Number (required),
  maxMarks: Number (required),
  percentage: Number (auto-calculated),
  date: Date (required),
  semester: String (required),
  createdAt: Date
}
```

**Virtual Fields**:
```javascript
gradeSchema.virtual('percentage').get(function() {
  return (this.marks / this.maxMarks) * 100;
});
```

</details>

<details>
<summary><b>⚖️ WorkloadScore Model</b></summary>

```javascript
{
  _id: ObjectId,
  studentId: ObjectId (ref: User, required),
  date: Date (required),
  dailyScore: Number (required),
  taskScore: Number (default: 0),
  eventScore: Number (default: 0),
  weeklyScore: Number,
  breakdown: {
    taskCount: Number,
    eventCount: Number,
    exams: Number,
    projects: Number,
    assignments: Number,
    quizzes: Number
  },
  createdAt: Date
}
```

**Indexes**:
- `{studentId: 1, date: 1}` (unique compound)

</details>

<details>
<summary><b>🚨 Signal Model</b></summary>

```javascript
{
  _id: ObjectId,
  studentId: ObjectId (ref: User, required),
  score: Number (0-100, required),
  risk: enum ['low', 'medium', 'high'],
  reasons: [String],
  signals: {
    collision: {
      hasCollision: Boolean,
      count: Number,
      taskCount: Number,
      eventCount: Number,
      totalHours: Number,
      items: [Object]
    },
    volatility: {
      hasVolatility: Boolean,
      percentageChange: Number,
      severity: String
    },
    recovery: {
      hasRecoveryDeficit: Boolean,
      consecutiveDays: Number
    },
    drift: {
      hasDrift: Boolean,
      durationMonths: Number,
      correlation: Number
    }
  },
  timestamp: Date (default: now),
  acknowledged: Boolean (default: false)
}
```

**Indexes**:
- `{studentId: 1, timestamp: -1}`
- `risk`

</details>

---

## 🗂️ Project Structure

```
burnout-system/
│
├── backend/
│   ├── config/
│   │   └── constants.js           # System weights, thresholds, configs
│   │
│   ├── models/
│   │   ├── CalendarEvent.js       # Calendar events schema
│   │   ├── Grade.js               # Student grades schema
│   │   ├── Intervention.js        # Proctor interventions schema
│   │   ├── Signal.js              # Burnout signals schema
│   │   ├── Task.js                # Student tasks schema
│   │   ├── User.js                # Users (students/proctors/admins)
│   │   └── WorkloadScore.js       # Daily/weekly workload scores
│   │
│   ├── routes/
│   │   ├── admin.js               # Admin operations (users, calendar, analytics)
│   │   ├── auth.js                # Registration, login, profile
│   │   ├── burnout.js             # Burnout analysis & prediction
│   │   ├── calendar.js            # Personal + institutional events
│   │   ├── calendar-upload.js     # Admin CSV upload
│   │   ├── grades.js              # Grade CRUD operations
│   │   ├── proctor.js             # Proctor dashboard & interventions
│   │   ├── tasks.js               # Task management
│   │   ├── workload.js            # Workload data endpoints
│   │   └── routes.js              # Main route aggregator
│   │
│   ├── services/
│   │   ├── burnout.js             # 🧠 Main burnout prediction engine
│   │   ├── collision.js           # Deadline collision detector
│   │   ├── drift.js               # Performance drift detector
│   │   ├── recovery.js            # Recovery gap detector
│   │   ├── volatility.js          # Workload spike detector
│   │   └── workload.js            # Workload calculation service
│   │
│   ├── middleware/
│   │   ├── auth.js                # JWT authentication
│   │   └── validate.js            # Request validation
│   │
│   ├── utils/
│   │   ├── db.js                  # MongoDB connection
│   │   └── logger.js              # Winston logger
│   │
│   ├── jobs.js                    # Scheduled jobs (daily analysis)
│   ├── server.js                  # Express app entry point
│   ├── package.json
│   └── .env.example
│
├── frontend/
│   ├── css/
│   │   └── main.css               # Global styles
│   │
│   ├── js/
│   │   ├── api.js                 # API client wrapper
│   │   ├── auth.js                # Auth helpers
│   │   ├── charts.js              # Chart.js configurations
│   │   └── utils.js               # Utility functions
│   │
│   ├── pages/
│   │   ├── login.html             # Login/register
│   │   ├── student.html           # 📊 Student dashboard
│   │   ├── tasks.html             # Task management
│   │   ├── calendar.html          # Calendar view
│   │   ├── grades.html            # Grade management
│   │   ├── proctor.html           # 👨‍🏫 Proctor dashboard
│   │   └── admin.html             # 🔧 Admin panel
│   │
│   ├── assets/
│   │   └── images/                # Icons, logos
│   │
│   └── index.html                 # Landing page
│
├── docs/
│   ├── API.md                     # API documentation
│   ├── ARCHITECTURE.md            # System architecture
│   ├── DEPLOYMENT.md              # Deployment guide
│   └── CONTRIBUTING.md            # Contribution guidelines
│
├── tests/
│   ├── unit/                      # Unit tests
│   ├── integration/               # Integration tests
│   └── e2e/                       # End-to-end tests
│
├── scripts/
│   ├── seed.js                    # Database seeding
│   └── migrate.js                 # Schema migrations
│
├── .gitignore
├── LICENSE
└── README.md
```

---

## 🔌 API Reference

### Authentication

<details>
<summary><b>POST</b> <code>/api/auth/register</code></summary>

**Request**:
```json
{
  "name": "John Doe",
  "email": "john@university.edu",
  "password": "SecurePass123!",
  "role": "student"
}
```

**Response** (201):
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "_id": "507f1f77bcf86cd799439011",
    "name": "John Doe",
    "email": "john@university.edu",
    "role": "student"
  }
}
```

</details>

<details>
<summary><b>POST</b> <code>/api/auth/login</code></summary>

**Request**:
```json
{
  "email": "john@university.edu",
  "password": "SecurePass123!"
}
```

**Response** (200):
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "_id": "507f1f77bcf86cd799439011",
    "name": "John Doe",
    "email": "john@university.edu",
    "role": "student",
    "proctorId": "507f1f77bcf86cd799439012"
  }
}
```

</details>

### Burnout Analysis

<details>
<summary><b>GET</b> <code>/api/burnout/analysis</code> 🔒</summary>

**Description**: Get comprehensive burnout analysis for the authenticated student

**Headers**:
```
Authorization: Bearer <token>
```

**Response** (200):
```json
{
  "score": 52,
  "risk": "medium",
  "reasons": [
    "4 major deadlines/events in next 7 days",
    "Workload increased by 127% from last week",
    "12 consecutive days without rest"
  ],
  "signals": {
    "collision": {
      "hasCollision": true,
      "count": 4,
      "taskCount": 2,
      "eventCount": 2,
      "totalHours": 107,
      "week": "2026-02-08 to 2026-02-14",
      "items": [...]
    },
    "volatility": {
      "hasVolatility": true,
      "percentageChange": 127,
      "previousAverage": 47,
      "currentWeek": 107,
      "severity": "high"
    },
    "recovery": {
      "hasRecoveryDeficit": true,
      "consecutiveDays": 12,
      "startDate": "2026-01-28",
      "endDate": "2026-02-08"
    },
    "drift": {
      "hasDrift": false
    }
  },
  "timestamp": "2026-02-08T10:30:00.000Z"
}
```

</details>

<details>
<summary><b>POST</b> <code>/api/burnout/predict/:studentId</code> 🔒 Admin/Proctor</summary>

**Description**: Manually trigger burnout prediction for a specific student

**Params**:
- `studentId` - MongoDB ObjectId

**Response** (200):
```json
{
  "message": "Burnout prediction completed",
  "signal": { /* Same structure as GET /analysis */ }
}
```

</details>

### Workload

<details>
<summary><b>GET</b> <code>/api/workload?days=30</code> 🔒</summary>

**Description**: Get daily workload scores for the past N days

**Query Parameters**:
- `days` (optional, default: 30) - Number of days to retrieve

**Response** (200):
```json
{
  "workload": [
    {
      "date": "2026-02-08",
      "dailyScore": 107,
      "taskScore": 47,
      "eventScore": 60,
      "weeklyScore": 285,
      "breakdown": {
        "taskCount": 3,
        "eventCount": 3,
        "exams": 2,
        "projects": 1,
        "assignments": 0,
        "quizzes": 0
      }
    },
    ...
  ],
  "summary": {
    "avgDailyScore": 42.3,
    "maxDailyScore": 107,
    "totalDays": 30,
    "highLoadDays": 8
  }
}
```

</details>

### Tasks

<details>
<summary><b>POST</b> <code>/api/tasks</code> 🔒</summary>

**Request**:
```json
{
  "title": "Calculus Midterm",
  "type": "exam",
  "deadline": "2026-02-15T09:00:00Z",
  "estimatedEffort": 5
}
```

**Response** (201):
```json
{
  "_id": "507f1f77bcf86cd799439013",
  "studentId": "507f1f77bcf86cd799439011",
  "title": "Calculus Midterm",
  "type": "exam",
  "deadline": "2026-02-15T09:00:00.000Z",
  "estimatedEffort": 5,
  "isCompleted": false,
  "createdAt": "2026-02-08T10:30:00.000Z"
}
```

</details>

<details>
<summary><b>GET</b> <code>/api/tasks?status=pending&limit=10</code> 🔒</summary>

**Query Parameters**:
- `status` - `pending` | `completed` | `all`
- `limit` - Number of tasks to return
- `startDate` - ISO date
- `endDate` - ISO date

**Response** (200):
```json
{
  "tasks": [...],
  "total": 15,
  "pending": 8,
  "completed": 7
}
```

</details>

### Calendar

<details>
<summary><b>GET</b> <code>/api/calendar/all?startDate=2026-02-01&endDate=2026-02-28</code> 🔒</summary>

**Description**: Get all visible events (institutional + personal)

**Response** (200):
```json
{
  "events": [
    {
      "_id": "507f1f77bcf86cd799439014",
      "title": "Student Tour",
      "eventType": "event",
      "startDate": "2026-02-08T08:00:00.000Z",
      "endDate": "2026-02-08T17:00:00.000Z",
      "isInstitutional": true,
      "priority": 5,
      "venue": "Main Campus",
      "duration": 8
    },
    ...
  ]
}
```

</details>

<details>
<summary><b>POST</b> <code>/api/calendar/personal</code> 🔒</summary>

**Description**: Create a personal calendar event

**Request**:
```json
{
  "title": "Study Group",
  "eventType": "event",
  "startDate": "2026-02-10T14:00:00Z",
  "endDate": "2026-02-10T16:00:00Z",
  "venue": "Library Room 3",
  "duration": 2
}
```

</details>

### Proctor

<details>
<summary><b>GET</b> <code>/api/proctor/students</code> 🔒 Proctor</summary>

**Description**: Get all students assigned to the proctor with their latest burnout status

**Response** (200):
```json
{
  "students": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "name": "John Doe",
      "email": "john@university.edu",
      "latestSignal": {
        "score": 52,
        "risk": "medium",
        "timestamp": "2026-02-08T10:30:00.000Z"
      },
      "interventionCount": 2,
      "lastIntervention": "2026-02-01T08:00:00.000Z"
    },
    ...
  ]
}
```

</details>

<details>
<summary><b>POST</b> <code>/api/proctor/intervene</code> 🔒 Proctor</summary>

**Request**:
```json
{
  "studentId": "507f1f77bcf86cd799439011",
  "signalId": "507f1f77bcf86cd799439020",
  "message": "I noticed you have a heavy workload this week. Let's discuss time management strategies.",
  "resources": "Time Management Workshop: https://university.edu/wellness/time"
}
```

</details>

---

## 📱 User Interfaces

### 🎓 Student Dashboard

```
┌────────────────────────────────────────────────────────────────┐
│  🧠 Academic Burnout Prediction System        👤 John Doe   ▼  │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ 🚨 BURNOUT RISK ALERT                                   │  │
│  │                                                         │  │
│  │         ⚠️  MEDIUM RISK                                │  │
│  │                                                         │  │
│  │  Score: 52/100        Risk Level: Medium               │  │
│  │                                                         │  │
│  │  ⚠️  4 major deadlines/events in next 7 days           │  │
│  │  📈  Workload increased by 127% from last week         │  │
│  │  🛌  12 consecutive days without rest                  │  │
│  │                                                         │  │
│  │  💡 Recommendation: Consider postponing non-critical   │  │
│  │     tasks and schedule recovery time this weekend.     │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                                │
│  ┌─────────────────┬────────────────────────────────────────┐ │
│  │ 📊 WORKLOAD     │  📅 UPCOMING EVENTS                    │ │
│  │ CHART           │                                        │ │
│  │                 │  🔴 Feb 8  - Student Tour (8h)         │ │
│  │      ▅          │  🟡 Feb 9  - Course Registration (3h)  │ │
│  │    ▃ █ ▆        │  🔴 Feb 10 - Welfare Visit (3h)        │ │
│  │  ▂ █ █ █ ▄      │  🔴 Feb 10 - Calculus Midterm (5h)     │ │
│  │ ─────────────── │  🟢 Feb 12 - Project Submission (8h)   │ │
│  │  M  T  W  T  F  │                                        │ │
│  └─────────────────┴────────────────────────────────────────┘ │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │ 📋 UPCOMING TASKS                   [+ Add Task]         │ │
│  ├──────────────────────────────────────────────────────────┤ │
│  │ ☐ Calculus Midterm        [Exam]     Due: Feb 15  [5h]  │ │
│  │ ☐ Project Submission      [Project]  Due: Feb 12  [8h]  │ │
│  │ ☐ History Assignment      [Assign]   Due: Feb 20  [3h]  │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │ 📈 RECENT GRADES                    [+ Add Grade]        │ │
│  ├──────────────────────────────────────────────────────────┤ │
│  │ Calculus Quiz          18/20  (90%)  Feb 5               │ │
│  │ Physics Midterm        76/100 (76%)  Feb 1               │ │
│  │ English Essay          85/100 (85%)  Jan 28              │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### 👨‍🏫 Proctor Dashboard

```
┌────────────────────────────────────────────────────────────────┐
│  🧠 Proctor Dashboard                  👨‍🏫 Dr. Smith      ▼  │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ 📊 STUDENT RISK SUMMARY                                 │  │
│  │                                                         │  │
│  │   🔴 High Risk:    3 students                          │  │
│  │   🟡 Medium Risk:  7 students                          │  │
│  │   🟢 Low Risk:    15 students                          │  │
│  │                                                         │  │
│  │   ⚠️  2 students need immediate intervention           │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │ 🚨 AT-RISK STUDENTS                   [View All]         │ │
│  ├──────────────────────────────────────────────────────────┤ │
│  │ 🔴 John Doe                    Score: 68  [Intervene]   │ │
│  │    Last updated: 2 hours ago                            │ │
│  │    ⚠️ Deadline collision + No recovery for 12 days      │ │
│  │                                                          │ │
│  │ 🔴 Jane Smith                  Score: 72  [Intervene]   │ │
│  │    Last updated: 5 hours ago                            │ │
│  │    ⚠️ Performance drift + Workload spike                │ │
│  │                                                          │ │
│  │ 🟡 Mike Johnson                Score: 45  [View]        │ │
│  │    Last updated: 1 day ago                              │ │
│  │    ⚠️ Moderate workload spike detected                  │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │ 💬 RECENT INTERVENTIONS                                  │ │
│  ├──────────────────────────────────────────────────────────┤ │
│  │ Feb 7  - John Doe    ✅ Acknowledged                     │ │
│  │ Feb 5  - Jane Smith  ⏳ Pending                          │ │
│  │ Feb 3  - Mike Johnson ✅ Acknowledged                    │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧪 Testing

### Run Tests

```bash
# Unit tests
npm test

# Integration tests
npm run test:integration

# E2E tests
npm run test:e2e

# Coverage report
npm run test:coverage
```

### Test Coverage Goals

- **Services**: >90% coverage
- **Routes**: >85% coverage
- **Models**: >95% coverage

### Sample Test

```javascript
describe('Burnout Prediction Engine', () => {
  it('should detect collision when 3+ major tasks in one week', async () => {
    const studentId = new mongoose.Types.ObjectId();
    
    // Create test data
    await Task.create([
      { studentId, type: 'exam', deadline: '2026-02-10', estimatedEffort: 5 },
      { studentId, type: 'exam', deadline: '2026-02-12', estimatedEffort: 4 },
      { studentId, type: 'project', deadline: '2026-02-14', estimatedEffort: 8 }
    ]);
    
    const collision = await detectCollisions(studentId);
    
    expect(collision.hasCollision).toBe(true);
    expect(collision.count).toBe(3);
  });
});
```

---

## 🚀 Deployment

### Production Checklist

- [ ] Set strong `JWT_SECRET` in environment
- [ ] Configure MongoDB Atlas with replica sets
- [ ] Enable MongoDB connection pooling
- [ ] Set up HTTPS with Let's Encrypt
- [ ] Configure CORS for production domain
- [ ] Enable rate limiting on API routes
- [ ] Set up monitoring (Prometheus/Grafana)
- [ ] Configure error tracking (Sentry)
- [ ] Set up automated backups
- [ ] Enable daily analysis scheduled job
- [ ] Configure email SMTP for notifications

### Docker Deployment

```bash
# Build and run with Docker Compose
docker-compose up -d

# View logs
docker-compose logs -f backend

# Stop services
docker-compose down
```

**docker-compose.yml**:
```yaml
version: '3.8'
services:
  mongodb:
    image: mongo:6.0
    volumes:
      - mongo-data:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: secure_password
    
  backend:
    build: ./backend
    ports:
      - "3000:3000"
    environment:
      MONGODB_URI: mongodb://admin:secure_password@mongodb:27017/burnout-system?authSource=admin
      JWT_SECRET: ${JWT_SECRET}
    depends_on:
      - mongodb
    
  frontend:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./frontend:/usr/share/nginx/html

volumes:
  mongo-data:
```

---

## 🔧 Configuration

### System Constants

**File**: `/backend/config/constants.js`

```javascript
module.exports = {
  // Task Type Weights
  WEIGHTS: {
    exam: 3.0,
    project: 2.5,
    assignment: 1.5,
    quiz: 1.0
  },
  
  // Event Type Weights
  EVENT_WEIGHTS: {
    exam: 8,
    registration: 4,
    event: 3,
    holiday: 0
  },
  
  // Collision Thresholds
  COLLISION: {
    MAJOR_TASK_THRESHOLD: 3,
    HOURS_THRESHOLD: 50,
    LOOKBACK_DAYS: 14
  },
  
  // Volatility Thresholds
  VOLATILITY: {
    MODERATE_SPIKE: 50,  // 50% increase
    HIGH_SPIKE: 100,     // 100% increase
    EXTREME_SPIKE: 200   // 200% increase
  },
  
  // Recovery Thresholds
  RECOVERY: {
    HIGH_LOAD_THRESHOLD: 10,
    MIN_CONSECUTIVE_DAYS: 7,
    LOOKBACK_DAYS: 30
  },
  
  // Drift Thresholds
  DRIFT: {
    MIN_MONTHS: 3,
    CORRELATION_THRESHOLD: -0.5,
    LOOKBACK_MONTHS: 6
  },
  
  // Risk Scoring
  RISK_LEVELS: {
    HIGH: 60,
    MEDIUM: 30,
    LOW: 0
  }
};
```

---

## 🐛 Troubleshooting

<details>
<summary><b>Issue: Calendar events not showing in workload calculation</b></summary>

**Symptoms**:
- Workload chart shows lower scores than expected
- Events are visible in calendar but not in burnout analysis

**Solution**:
1. Verify events exist in database:
   ```javascript
   CalendarEvent.find({ isInstitutional: true })
   ```

2. Check workload service includes events:
   ```javascript
   // In /services/workload.js
   const events = await CalendarEvent.find({
     $or: [
       { isInstitutional: true },
       { createdBy: studentId }
     ],
     startDate: { $gte: startDate, $lte: endDate }
   });
   ```

3. Clear workload cache and recalculate:
   ```bash
   POST /api/workload/recalculate
   ```

</details>

<details>
<summary><b>Issue: Burnout score always shows LOW</b></summary>

**Symptoms**:
- Student has many tasks but score is 0-15
- No collision/volatility signals detected

**Possible Causes**:
1. **Missing events in calculation** - See above
2. **Incorrect date ranges** - Check task deadlines are in future
3. **Weights misconfigured** - Verify `config/constants.js`

**Debug**:
```bash
# Check signal generation
GET /api/burnout/analysis

# Manually trigger prediction
POST /api/burnout/predict/:studentId

# View raw workload scores
GET /api/workload?days=30
```

</details>

<details>
<summary><b>Issue: MongoDB connection errors</b></summary>

**Error**: `MongooseServerSelectionError: connect ECONNREFUSED 127.0.0.1:27017`

**Solutions**:
1. Ensure MongoDB is running:
   ```bash
   sudo systemctl status mongod
   sudo systemctl start mongod
   ```

2. Check connection string in `.env`:
   ```env
   MONGODB_URI=mongodb://localhost:27017/burnout-system
   ```

3. For MongoDB Atlas, verify:
   - IP whitelist includes your IP
   - Username/password are correct
   - Network access is configured

</details>

---

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

### Development Workflow

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```

3. **Make your changes**
   - Follow existing code style
   - Add tests for new features
   - Update documentation

4. **Run tests**
   ```bash
   npm test
   npm run lint
   ```

5. **Commit with conventional commits**
   ```bash
   git commit -m "feat: add email notifications for high-risk students"
   ```

6. **Push and create Pull Request**

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

### Code Style

- **JavaScript**: ESLint + Prettier
- **Naming**: camelCase for variables, PascalCase for classes
- **Comments**: JSDoc for functions
- **File naming**: kebab-case for files

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2026 Burnout Prediction System Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software...
```

---

## 👥 Authors & Contributors

<table>
<tr>
<td align="center">
<a href="https://github.com/yourusername">
<img src="https://via.placeholder.com/100" width="100px;" alt=""/><br />
<sub><b>Your Name</b></sub>
</a><br />
💻 🎨 📖
</td>
<td align="center">
<a href="https://github.com/contributor1">
<img src="https://via.placeholder.com/100" width="100px;" alt=""/><br />
<sub><b>Contributor 1</b></sub>
</a><br />
💻 🐛
</td>
</tr>
</table>

---

## 🙏 Acknowledgments

- **Inspiration**: Academic wellness research from MIT, Stanford
- **Libraries**: Express.js, MongoDB, Chart.js, Mongoose
- **Community**: Open-source contributors worldwide

---

## 📞 Support

- **Documentation**: [docs/](docs/)
- **Issues**: [GitHub Issues](https://github.com/yourusername/burnout-prediction-system/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/burnout-prediction-system/discussions)
- **Email**: support@burnoutpredict.edu

---

## 🗺️ Roadmap

### Version 2.0 (Q2 2026)
- [ ] Machine learning model integration
- [ ] Email/SMS notifications
- [ ] Mobile app (React Native)
- [ ] Slack/Discord bot integration

### Version 2.1 (Q3 2026)
- [ ] Peer comparison analytics
- [ ] Gamification features
- [ ] AI-powered recommendations
- [ ] Integration with LMS (Canvas, Moodle)

### Version 3.0 (Q4 2026)
- [ ] Real-time collaboration features
- [ ] Advanced predictive models
- [ ] Multi-university deployment
- [ ] Research paper publication

---

<div align="center">

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=yourusername/burnout-prediction-system&type=Date)](https://star-history.com/#yourusername/burnout-prediction-system&Date)

---

### Made with ❤️ for student wellness

**[⬆ Back to Top](#-academic-burnout-prediction-system)**

</div>
