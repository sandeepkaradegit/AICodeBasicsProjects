🏋️ Workout Plan Generator

A single-page Streamlit app that takes structured inputs about a user's fitness goals and generates a personalized weekly workout plan using an LLM via the Groq API.

Built for the AI Engineering Cohort – Build a Workout Plan Generator assignment (Session 2: LLMs, Embeddings & Transformer Architecture).

What this app does
Collects structured inputs (not a single text box): fitness goal, experience level, days/week, equipment access, and optional injuries/limitations.
Builds a well-designed prompt from those inputs (build_prompt) and sends it to Groq via a typed function (generate_workout_plan).
Displays a structured, day-by-day plan (exercises, sets, reps, coaching cues).
Handles bad input and API failures gracefully — the app never crashes.
Lets you regenerate a variation, and download the plan as a .md file.
Project structure
workout-plan-generator/
├── app.py              # Streamlit UI + prompt builder + Groq call + error handling
├── requirements.txt    # Python dependencies
├── .env.example         # Template for your API key (copy to .env)
├── .gitignore
└── README.md
Step-by-step setup
1. Clone the repo
bash
git clone <your-repo-url>
cd workout-plan-generator
2. Create a virtual environment (recommended)
bash
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
3. Install dependencies
bash
pip install -r requirements.txt
4. Get a free Groq API key
Go to console.groq.com/keys
Sign in and click Create API Key
Copy the key (starts with gsk_...)
5. Add your API key

Copy the example env file and paste in your key:

bash
cp .env.example .env

Edit .env:

GROQ_API_KEY=gsk_your_actual_key_here

Alternatively, you can skip .env entirely and paste the key directly into the sidebar field when the app is running — it's used only for that session and never saved.

6. Run the app
bash
streamlit run app.py

This opens the app at http://localhost:8501.

7. Use it
Pick a fitness goal, experience level, days/week, and equipment.
Optionally describe any injuries or limitations.
Click Generate Plan.
Click Regenerate for a different variation of the same inputs, or Download plan (.md) to save it.
Design notes (the actual point of the assignment)
Prompt design lives in build_prompt() and SYSTEM_PROMPT, separate from the API call, so it can be iterated on independently.
The system prompt gives the model:
A role (certified personal trainer).
Hard constraints it must respect (equipment, days/week, injuries) instead of ignoring them.
An output contract: one Day N: block per requested day, with sets/reps/cues, plus a closing Notes section — never a wall of text.
Scope limits: no medical claims, no diagnoses, and a short disclaimer whenever injury/limitation input is provided.
I iterated on this prompt against edge cases like "1 day/week + Build muscle" (mismatched goal vs. time) and "bad knees + Full gym" (constraint the model could easily ignore) to make sure it adapts instead of producing a generic plan.
Error handling
Scenario	Behavior
0 days / missing goal / missing equipment	Friendly warning, no API call made
Missing/invalid Groq API key	Friendly warning, no crash
Network failure	Friendly warning, no crash
Rate limit hit	Friendly warning, no crash
Empty or malformed model response	Friendly fallback message
Any unexpected exception	Caught, friendly message shown

generate_workout_plan() never raises — it always returns either a valid plan string or a user-facing ⚠️ message, so the Streamlit app itself has no try/except sprinkled through the UI code.

Stretch goals implemented
✅ Regenerate button (higher temperature, same inputs, new variation)
✅ st.session_state persists the last plan across reruns
✅ Download the plan as a .md file
⬜ "Swap this exercise" mini-feature (not implemented — noted as a natural next step)
Tech stack
Python (type hints, try/except, function separation)
Streamlit (UI)
Groq API (llama-3.3-70b-versatile) for plan generation
Submission

Push this repo to a public GitHub repository and submit the link in Session 2 – LLMs, Embeddings & Transformer Architecture using the Submit button.

bash
git init
git add .
git commit -m "Workout Plan Generator - AI Engineering Cohort assignment"
git branch -M main
git remote add origin <your-public-repo-url>
git push -u origin main

Double-check .env is not committed (it's already in .gitignore) — only .env.example should be in the repo.
