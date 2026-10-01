# FitBuddy – AI Fitness Plan Generator using Gemini Models

## 1. Project Overview

**FitBuddy** is a BCA final-year web application that generates a structured fitness routine from a user's basic profile. The application uses **Google Gemini models** as the AI layer, **Python Flask** as the backend, **HTML/CSS/JavaScript** for the frontend, and **SQLite** for storing recent generated plans.

The project is intentionally designed to be simple enough for a student project demonstration while still showing a complete AI + web + database workflow.

### Main workflow

```text
User Profile
     ↓
Web Form (HTML/CSS/JS)
     ↓
Flask REST API
     ↓
Gemini Model ───────→ Structured JSON fitness plan
     ↓                         ↓
SQLite Database          Web UI displays plan
```

If no Gemini API key is configured, the application automatically uses a built-in **demo mode**. This makes the project runnable during a college demonstration even when internet/API access is unavailable.

---

## 2. Objectives

1. Build an easy-to-use fitness planning web application.
2. Demonstrate integration of a generative AI model.
3. Collect user requirements through a web form.
4. Generate a structured weekly activity plan.
5. Store generated plans using SQLite.
6. Provide a responsive interface for desktop and mobile screens.
7. Demonstrate API communication between frontend and backend.
8. Include basic safety-oriented prompt rules and validation.

---

## 3. Technologies Used

| Technology | Purpose |
|---|---|
| Python | Backend programming |
| Python standard library | Web server / REST API |
| Gemini API | AI plan generation |
| HTML5 | Page structure |
| CSS3 | Responsive UI design |
| JavaScript | Form handling and API calls |
| SQLite | Local database |
| JSON | Data exchange |
| Git | Version control (optional) |

---

## 4. System Requirements

### Hardware

- Any modern laptop/desktop
- Minimum 4 GB RAM
- Approximately 200 MB free project space
- Internet connection only when using Gemini API

### Software

- Python 3.10 or newer recommended
- Web browser such as Chrome, Edge, or Firefox
- VS Code / PyCharm / any text editor

---

## 5. Project Folder Structure

```text
FitBuddy_Project/
│
├── app.py
├── requirements.txt
├── .env.example
├── README.md
├── fitbuddy.db              # created automatically after first run
│
├── templates/
│   └── index.html
│
├── static/
│   ├── style.css
│   └── app.js
│
└── screenshots/
    └── fitbuddy-demo.png    # optional screenshot
```

---

## 6. Installation

### Step 1 – Open the project folder

```bash
cd FitBuddy_Project
```

### Step 2 – Create a virtual environment

Windows:

```bash
python -m venv venv
venv\\Scripts\\activate
```

Linux/macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 3 – Install dependencies

```bash
pip install -r requirements.txt
```

---

## 7. Run the Project Without Gemini

This is the fastest way to demonstrate the project.

```bash
python app.py
```

Open:

```text
http://127.0.0.1:5000
```

The application automatically switches to **Demo Mode** when `GEMINI_API_KEY` is not configured.

---

## 8. Configure Gemini AI

To use actual Gemini generation, obtain a Gemini API key from Google's official AI developer platform and keep the key private.

Create a local environment variable.

### Windows PowerShell

```powershell
$env:GEMINI_API_KEY="YOUR_API_KEY"
$env:GEMINI_MODEL="gemini-2.5-flash"
python app.py
```

### Linux/macOS

```bash
export GEMINI_API_KEY="YOUR_API_KEY"
export GEMINI_MODEL="gemini-2.5-flash"
python app.py
```

The application sends the user's non-sensitive profile fields to the configured Gemini model and requests a JSON response.

**Do not commit API keys to GitHub.**

---

## 9. How the Application Works

### 9.1 Frontend

`templates/index.html` contains:

- Project title and dashboard header
- User profile form
- Age input
- Fitness goal selector
- Fitness level selector
- Days-per-week selector
- Equipment selector
- Generate button
- AI plan display area
- Recent plan history

### 9.2 JavaScript

`static/app.js`:

1. Reads the form fields.
2. Creates a JSON object.
3. Sends the object to `/api/generate` using `fetch()`.
4. Receives the generated plan.
5. Displays the weekly schedule dynamically.
6. Refreshes the recent history list.

### 9.3 Python Backend

`app.py` provides:

- `/` – main application page
- `/api/generate` – generates and saves a plan
- `/api/history` – returns recent plans
- `/api/health` – basic application health status

### 9.4 Gemini Integration

The backend builds a controlled prompt containing the selected profile information and requests a fixed JSON structure.

The expected AI output contains:

- Summary
- Weekly schedule
- Exercises
- Warm-up
- Recovery tips
- Safety note
- Motivation

If the Gemini request fails, FitBuddy falls back to the built-in demo generator so the application remains usable for demonstration.

### 9.5 SQLite Database

The database table is named `plans`.

Important fields:

```text
id
name
age
goal
fitness_level
days_per_week
equipment
plan_json
created_at
```

---

## 10. API Documentation

### POST `/api/generate`

Example request:

```json
{
  "name": "Student",
  "age": 20,
  "days_per_week": 3,
  "goal": "General fitness",
  "fitness_level": "Beginner",
  "equipment": ["None / bodyweight", "Yoga mat"]
}
```

Example response structure:

```json
{
  "id": 1,
  "mode": "gemini",
  "plan": {
    "summary": "...",
    "weekly_schedule": [],
    "warmup": [],
    "recovery_tips": [],
    "safety_note": "...",
    "motivation": "..."
  }
}
```

### GET `/api/history`

Returns the latest saved plans.

### GET `/api/health`

Returns a simple status response and whether a Gemini key is configured.

---

## 11. Database Design

### Entity: Plan

| Field | Type | Description |
|---|---|---|
| id | INTEGER | Primary key |
| name | TEXT | User name |
| age | INTEGER | User age |
| goal | TEXT | Selected fitness goal |
| fitness_level | TEXT | Beginner/intermediate/advanced |
| days_per_week | INTEGER | Number of active days |
| equipment | TEXT | Selected equipment |
| plan_json | TEXT | Generated plan in JSON format |
| created_at | TEXT | Creation timestamp |

For this project, a single-table design is sufficient because the goal is to demonstrate the AI workflow rather than build a large multi-user production database.

---

## 12. Safety and Validation

The project includes simple validation and safety-oriented prompt rules.

- Age is limited to 13–100 in the demo application.
- Days per week are limited to 1–7.
- Empty goals are rejected.
- The AI prompt avoids extreme exercise and restrictive diet instructions.
- The generated plan includes recovery and safety reminders.
- The project does not diagnose medical conditions.
- Users are told to stop when they experience pain, dizziness, or unusual discomfort.

This application is an educational software project and is **not a substitute for professional medical or fitness advice**.

---

## 13. Testing

### Test Case 1 – Valid profile

Input:

```text
Name: Student
Age: 20
Goal: General fitness
Level: Beginner
Days: 3
```

Expected result: A weekly plan is displayed and saved in SQLite.

### Test Case 2 – Invalid age

Input:

```text
Age: 10
```

Expected result:

```text
Please enter an age from 13 to 100.
```

### Test Case 3 – No API key

Expected result: Demo mode creates a plan without calling Gemini.

### Test Case 4 – Gemini API available

Expected result: The status changes to `Gemini connected` and the model response is displayed.

### Test Case 5 – History

Generate two plans and press **Refresh**.

Expected result: Recent generated plans appear in the history section.

---

## 14. Advantages

1. Simple user interface.
2. AI-powered personalization.
3. Fast plan generation.
4. Works in demo mode without an API key.
5. SQLite makes the project easy to run locally.
6. Responsive design.
7. Clear separation between frontend and backend.
8. Easy to extend with authentication and analytics.

---

## 15. Limitations

1. The application is not a medical system.
2. SQLite is suitable for a student/local project but not ideal for a large production deployment.
3. AI output can vary between generations.
4. Gemini API usage requires internet access and an appropriate API configuration.
5. The current version has no user login system.
6. The application does not track actual exercise performance through wearable devices.

---

## 16. Future Enhancements

Possible future improvements include:

- User registration and login
- User profile dashboard
- Exercise video library
- Progress charts
- Workout completion tracking
- Calendar integration
- Wearable/device integration
- Personalized reminders
- Admin dashboard
- PostgreSQL/MySQL support
- More detailed exercise metadata
- Multilingual interface
- Cloud deployment

---

## 17. Suggested Project Modules for Viva

### Module 1 – User Interface
Collects user requirements through a responsive form.

### Module 2 – Request Processing
The Python backend validates the input and prepares the AI request.

### Module 3 – Gemini AI Module
Gemini receives the structured prompt and generates the fitness plan.

### Module 4 – Plan Display
JavaScript converts the returned JSON into a readable weekly schedule.

### Module 5 – Database Module
SQLite stores generated plans and timestamps.

### Module 6 – Fallback Module
The demo generator allows the application to work when Gemini is unavailable.

---

## 18. Viva Questions and Short Answers

**Q1. What is FitBuddy?**  
FitBuddy is an AI-based web application that generates structured fitness routines from user inputs.

**Q2. Why is Gemini used?**  
Gemini is used to generate natural-language, personalized fitness-plan content from a structured prompt.

**Q3. Why Flask?**  
Flask is lightweight, simple, and suitable for building a Python web application and REST API.

**Q4. Why SQLite?**  
SQLite is easy to configure because it stores the database in a local file and requires no separate database server.

**Q5. What happens if Gemini is unavailable?**  
The application uses a built-in demo generator and still displays a valid plan.

**Q6. What is JSON?**  
JSON is a lightweight data format used to exchange structured information between the frontend, backend, and AI service.

**Q7. What is the role of JavaScript?**  
JavaScript sends API requests and dynamically updates the web page with the generated plan.

**Q8. What is the future scope?**  
Authentication, progress tracking, wearable integration, cloud deployment, reminders, and advanced analytics can be added.

---

## 19. Demonstration Steps

1. Start the application with `python app.py`.
2. Open `http://127.0.0.1:5000`.
3. Enter a name and age.
4. Select a fitness goal.
5. Select fitness level.
6. Select the number of days.
7. Choose available equipment.
8. Click **Generate My Plan**.
9. Show the generated weekly schedule.
10. Scroll down and show the saved history.
11. If available, demonstrate the same flow with `GEMINI_API_KEY` configured and show `Gemini connected`.

---

## 20. Conclusion

FitBuddy demonstrates how a modern web application can combine a Python backend, a generative AI model, a responsive frontend, and a local database into one practical student project. The application provides a clear example of API integration, prompt engineering, JSON processing, database storage, input validation, and dynamic UI rendering.

**Project Title:** FitBuddy – AI Fitness Plan Generator using Gemini Models

**Project Type:** BCA Final Year Project

**Primary Stack:** Python + Gemini + HTML/CSS/JavaScript + SQLite
