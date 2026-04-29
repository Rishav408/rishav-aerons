# Part 1: Project Overview & Architecture

### 1. Architecture walkthrough (Flask → OWM → Frontend flow)
The weather utility relies on a simple client-server architecture:
- **Frontend (Client):** The user enters a city on the webpage or clicks a button. The browser (via Vanilla JS) makes an asynchronous HTTP GET request to our Flask backend (e.g., `/weather?city=Kolkata`).
- **Flask (Backend):** The Flask route `@app.route("/weather")` catches this request. It calls the internal `_owm_get` function, appending our `OWM_KEY` and setting `units=metric`.
- **OpenWeatherMap (External API):** Flask sends a synchronous `requests.get` to the OWM API. OWM processes the request and returns the weather data as JSON.
- **Data Processing & Return:** Flask parses this JSON, extracts only the necessary fields (temperature, humidity, condition, calculating IST times, etc.), and returns a clean, structured JSON payload back to the frontend.
- **Frontend UI Update:** The JavaScript on the frontend receives this JSON and dynamically updates the DOM and chart components with the new data.

### 2. Why Flask over FastAPI or Django
- **Why not Django:** Django is a "batteries-included" framework suited for large, complex applications that require an ORM, database, and admin panel out of the box. For a lightweight weather dashboard, it's overkill and adds unnecessary bloat.
- **Why not FastAPI:** While FastAPI is excellent for asynchronous execution and automatic API documentation, our weather utility relies heavily on traditional server-side rendered HTML templates using Jinja (`render_template`). Flask is synchronous, straightforward, and provides the simplest paradigm for binding backend logic to HTML templates for this specific use case.

### 3. How OWM API integration works
Integration is centralized in the `_owm_get(endpoint, params)` helper function:
- It securely fetches the `OWM_KEY` from the environment.
- It builds the payload by injecting `appid` and `units="metric"`.
- It uses the Python `requests` library to make the HTTP call to the base URL `https://api.openweathermap.org/data/2.5/`.
- It includes robust error handling: gracefully catching request exceptions, handling 404s (e.g., if a user types an invalid city name), and preventing the app from crashing by returning a clean error tuple `(None, error_msg)` back to the route.

### 4. Chart.js data binding from API response
In the `/forecast` route, the backend parses the OWM 5-day/3-hour forecast API response. It extracts the times (`labels`) and temperatures (`temps`). These are sent to the frontend as a JSON object: `{"labels": [...], "temps": [...]}`. 
On the frontend, JavaScript passes these arrays directly into the Chart.js configuration object (e.g., binding `labels` to the x-axis and `temps` to the `data` array in the datasets). This maps the numerical and temporal data seamlessly to the visual graph.

### 5. Gemini AI integration — prompt engineering decisions
In `_generate_ai_text`, the prompt is highly constrained to ensure structured, predictable outputs:
- **Context Injection:** It dynamically passes the city, temp, condition, and humidity.
- **Strict Formatting:** It explicitly asks for "exactly 2 practical sentences, 3 short recommendations, 3 signal bullets, and 3 checklist bullets".
- **Structured JSON Output:** It commands Gemini to "Return strict JSON with keys: insight, tips, signals, checklist" and uses `responseMimeType: "application/json"`. 
This allows the backend to use `json.loads()` safely without worrying about the model wrapping the response in markdown or conversational fluff, ensuring the frontend always receives the expected object structure.

### 6. Fallback mechanism when Gemini key missing
If the `GEMINI_KEY` is not present in `.env`, or if the API request times out or fails (e.g., rate limits), the `_generate_ai_text` function intercepts the error and returns a hardcoded `fallback` dictionary. This fallback dynamically inserts the current weather data into pre-written text, ensuring the user still gets a seamless "AI Insight" experience without the app breaking or showing an error.

### 7. IST timezone conversion logic
The OWM API returns sunrise and sunset times as UTC Unix timestamps. The `unix_to_ist_str` function handles the conversion without requiring complex external libraries like `pytz`:
1. `datetime.utcfromtimestamp(ts)` converts the Unix integer into a UTC Python datetime object.
2. It adds `IST_OFFSET` (defined as `timedelta(hours=5, minutes=30)`).
3. `.strftime("%I:%M %p")` formats it into a human-readable 12-hour string (e.g., "05:45 PM").

### 8. Why Leaflet.js for maps
Leaflet is a lightweight, open-source JavaScript library for interactive maps. Unlike Google Maps, it doesn't require a credit card or strict API billing logic for basic usage. It makes it incredibly easy to load open-source tile layers (like OpenStreetMap) and overlay custom weather tile layers (like OWM's precipitation maps) using a simple initialization script.

### 9. .env pattern — why not hardcode keys
Hardcoding API keys (like `OWM_KEY` and `GEMINI_KEY`) directly into `app.py` is a critical security vulnerability. If pushed to GitHub, bots can scrape these keys and use your quota, leading to unexpected billing or account suspension. Using a `.env` file (which is ignored via `.gitignore`) allows keys to exist only locally, keeping the source code safe and easily deployable in production environments (where env vars are set via the hosting platform).

### 10. requirements.txt separation per utility
`weather_utility` and `data_utility` are fundamentally different applications requiring different packages (Flask + Requests vs. Streamlit + Pandas + Plotly). Keeping their `requirements.txt` separate:
- Prevents dependency conflicts.
- Reduces the environment footprint (so the weather app doesn't install heavy data science libraries).
- Adheres to microservice best practices, making each app independently containerizable (e.g., Docker) and deployable.

### 11. CSV Plotter — normalize_dataframe() purpose
In `data_utility/utils/preprocessing.py`, the `normalize_dataframe` function sanitizes the uploaded CSV before visualization:
- It drops columns that are completely empty.
- It iterates through text ("object") columns and attempts to parse them into proper Datetime or Numeric types using Pandas `to_datetime` and `to_numeric` with `errors="coerce"`.
- If 80% or more of the column can be successfully converted, it overwrites the column type. This allows the Plotly charts to properly plot data that was imported as strings but is actually numerical or temporal.

### 12. Streamlit vs Flask — when to use which
- **Use Streamlit** (like in `data_utility`) for data science, internal dashboards, and analytics tools. It requires zero frontend code (no HTML/JS/CSS) because it directly translates Python data structures (like Pandas dataframes and Plotly charts) into interactive web UIs.
- **Use Flask** (like in `weather_utility`) when building custom REST APIs, working with distinct HTML/CSS/JS frontend files, or when you need fine-grained control over HTTP routing, request handling, and server logic.

---

# Part 2: Software Engineering Basics

### 1. Git — commit, push, branch, clone
Git is a version control system used to track changes in code.
- **clone:** Downloads a copy of a remote repository (like from GitHub) to your local machine.
- **commit:** Takes a "snapshot" of the changes you've made to your files. Each commit has a message explaining *what* was changed and *why*.
- **branch:** A parallel version of the code. You create a branch to work on a new feature (e.g., `feature/weather-maps`) without affecting the main working code (`main`).
- **push:** Uploads your local commits to the remote repository so others can see your work or deploy it.

### 2. Why incremental commits matter
Instead of writing 1,000 lines of code and making one massive commit called "finished project", you should make many small, incremental commits (e.g., "added OWM API fetch", "fixed CSS button alignment").
**Why it matters:** 
1. **Debugging:** If a bug appears, you can trace it back to the exact small commit that broke the code.
2. **Revertibility:** You can easily undo a specific small change without losing days of work.
3. **Collaboration:** It makes it much easier for other developers to review your pull requests.

### 3. .gitignore — what to ignore and why
The `.gitignore` file tells Git which files or folders to *not* track.
**What to ignore:**
- `.env` files (contains secret API keys).
- `__pycache__` and `.pyc` files (compiled Python files, they are automatically generated and cause merge conflicts).
- `venv/` or `.venv/` folders (contains thousands of library files; developers should install dependencies themselves using `requirements.txt`).
- Operating system files (like `.DS_Store` on macOS).
**Why:** To prevent security leaks, keep the repository lightweight, and avoid sharing machine-specific configurations.

### 4. MVC pattern — Model View Controller
MVC is a software design pattern that separates an application into three interconnected parts.
- **Model:** Manages the data, logic, and rules of the application (e.g., your Python functions that process weather logic or clean Pandas DataFrames).
- **View:** The UI components that display the data to the user (e.g., your HTML/Jinja templates and Streamlit UI).
- **Controller:** The brain that connects the two. It accepts user input, talks to the Model, and passes the result to the View (e.g., your Flask `@app.route` functions).
*Note: Flask is often called "MVT" (Model-View-Template) but it follows the exact same logical separation.*

### 5. Separation of concerns — why CSS/JS/Python are separate
Separation of Concerns (SoC) is a design principle for separating a computer program into distinct sections.
- **HTML:** Defines the structure of the page.
- **CSS (`static/styles.css`):** Handles the visual design and layout.
- **JS (`static/script.js`):** Handles the frontend interactivity and API fetching.
- **Python (`app.py`):** Handles the backend business logic and secure API key management.
**Why:** It makes code significantly easier to read, test, and maintain. If you want to change the button color, you only look at the CSS. It also allows specialized developers (frontend vs backend) to work simultaneously without stepping on each other's toes.

### 6. Environment variables — dev vs production
Environment variables dynamically configure how your application runs based on where it is hosted.
- **Dev (Local):** You use a local `.env` file to store keys. The app might connect to a local development database and run with `debug=True`.
- **Production (Cloud):** There is no `.env` file. You inject the keys directly through the hosting provider's dashboard (like AWS or Heroku). The app connects to a production database and runs with `debug=False` for security and performance.
This ensures you don't accidentally wipe out production data while testing on your laptop.

### 7. README importance in open source
The `README.md` is the front page of your project. In open source or an interview portfolio, a codebase without a README is essentially useless to outsiders.
**Importance:**
1. It explains *what* the project is and *why* it exists.
2. It provides exact, copy-pasteable setup instructions (how to run the app, what Python version to use).
3. It outlines the environment variables needed.
Your comprehensive README demonstrates that you don't just write code, but you understand how to document and hand off software professionally.
