# Aeron Systems — AI Prototyping Intern Interview Prep

---

## 1. Behavioral & Soft Skills

### STAR Method — Situation, Task, Action, Result

Use this framework for every behavioral question. Structure every answer as:

- **Situation** — Set the context briefly
- **Task** — What was your responsibility
- **Action** — What you specifically did
- **Result** — What was the outcome, ideally measurable

**Example for this assignment:**
> "I had 48 hours (S), needed to build a live weather dashboard with AI insights (T), used Claude to scaffold the Flask backend and Chart.js integration, then manually fixed the OWM forecast grouping logic and IST timezone conversion (A), resulting in a fully functional multi-page dashboard submitted on time with all bonus criteria addressed (R)."

---

### "Tell Me About Yourself" — 60-Second Structured Pitch

Structure:
1. **Who you are** — T.Y. B.Tech CSE (AI & Data Science), MIT-WPU, Pune
2. **What you've built** — WeatherIntel, VeeraServe AI, CSV Data Utility
3. **How you work** — AI-assisted prototyping, full-stack comfort, design awareness
4. **Why you're here** — Aeron Systems sits at the intersection of AI and real-world systems

Keep it under 60 seconds. End with: *"I'm excited about this role because..."*

---

### Why Aeron Systems Specifically

Prepare a genuine, specific answer. Points to reference:
- Aeron Systems works in aerospace/defense embedded systems — a domain where AI prototyping has real-world physical consequence
- The role combines AI tooling with practical engineering — not just model training but building things that work
- You want to work where software interacts with hardware and real sensor data
- The NMEA decoder option in Part 2 of the assignment was a strong signal — GPS data, embedded parsing, structured output

Avoid generic answers like *"great company culture"* — they want specificity.

---

### Where Do You See Yourself in 2 Years

Honest, grounded answer:
- Deeper into applied AI — not just prototyping but understanding the systems these prototypes feed into
- Contributing to real aerospace/defense projects where the stakes of bad data or bad AI output are tangible
- Growing from prototyping to being able to evaluate, optimize, and deploy AI components in production contexts

Tie it back to what Aeron does.

---

### How You Handle Ambiguity

The assignment itself is evidence:
- No tech stack was specified — you chose Flask + OWM + Gemini + Leaflet independently
- No UI design was given — you researched, iterated, and refined
- No fixed timeline for each feature — you prioritized functionality first, polish second

Frame it as: *"I break ambiguity into the smallest decision I can make with confidence right now, execute it, and use the result to inform the next decision."*

---

### Teamwork vs Solo Work Preference

Be honest. This was a solo assignment, but show awareness of both:
- Solo: You move fast, make decisions without committee, own every line
- Team: Code review catches blind spots, pair programming accelerates learning
- Your preference: solo for prototyping speed, team for production quality — which maps exactly to this role

---

### How You Learn New Technologies Quickly

Use your project as direct evidence:
- Had never built a full Flask + OWM + Gemini + Leaflet stack before this assignment
- Learned Streamlit specifically for the CSV utility
- Used AI tools not as a crutch but as a fast onboarding path — generate → understand → modify
- Key habit: read the source/docs for any function you didn't write yourself before shipping it

---

## 2. Things Specific to AI Prototyping Role

### Prototype vs Production Code — Know This Cold

| Dimension | Prototype | Production |
|---|---|---|
| Error handling | Basic try/except | Comprehensive, logged, alerted |
| Auth | `.env` file | Secrets manager, OAuth, RBAC |
| Data storage | In-memory / flat files | Database with migrations |
| Scalability | Single process | Load balanced, async workers |
| Testing | Manual browser testing | Unit + integration + e2e tests |
| Monitoring | `print()` statements | APM, logging pipelines |
| API keys | Hardcoded in `.env` | Rotated, scoped, audited |

Your WeatherIntel is a prototype. Know exactly where the production gaps are.

---

### What "Iterating with AI" Means Practically

The actual loop you used:

```
1. Write a precise prompt describing exactly what you need
2. Get generated code
3. Run it — does it work?
4. If yes → understand every line before keeping it
5. If no → identify the specific failure, refine the prompt or fix manually
6. Commit working state before next iteration
```

Key insight to communicate: *"AI generates fast, but I verify before trusting. I never ship code I can't explain."*

---

### When AI-Generated Code Is Trustworthy vs When to Verify

**Trust more when:**
- Boilerplate patterns (Flask routes, Chart.js config, CSS layout)
- Well-documented APIs (OWM response parsing)
- Simple utility functions with clear inputs/outputs

**Verify carefully when:**
- Security-adjacent code (key handling, auth flows)
- Time/timezone logic — bugs are subtle (your IST conversion was one such case)
- Data transformation logic — normalization, type coercion
- Anything that touches external services with rate limits or costs

---

### Limitations of Free Tier APIs in Real Systems

From your own project experience:
- **OWM free tier**: No historical hourly data, limited forecast resolution (3h slots not 1h)
- **Gemini free tier**: Rate limits, context window constraints, no guaranteed SLA
- **Leaflet + OWM tiles**: Free tiles have usage caps, no offline fallback

In production you'd replace with:
- OWM Professional or Tomorrow.io for weather
- Gemini Pro / Claude API with proper key management for AI
- Mapbox or Google Maps for tiles with SLA guarantees

---

### Technical Debt You Knowingly Took

Be upfront about these — they show maturity:
- UV Index is hardcoded as `0` — real UV requires a paid OWM endpoint
- Precipitation bar in the UI is a static placeholder — minute-forecast API not wired
- No input sanitization on city search — SQL injection not a risk here but XSS is
- Streamlit CSV utility has no file size limit — large CSVs will crash it
- No caching layer — every search hits the OWM API fresh

---

## 3. About the Assignment PDF Specifically

### Why Weather Viewer Over File Upload Portal

Strong reasons to give:
- Weather viewer demonstrated more technical breadth — API integration, data visualization, AI inference, maps
- Aligned with your existing interest in full-stack AI applications
- More opportunity to show design sensibility alongside engineering
- File upload portal is a CRUD app — less differentiation opportunity

---

### Why CSV Plotter Over NMEA Decoder

Honest reasons:
- CSV plotter allowed integration of Gemini AI for data insights — showed AI usage in Part 2 as well
- More broadly applicable as a utility demonstration
- NMEA decoder is closer to embedded/systems work — relevant to Aeron's core but you chose to show AI breadth instead

**Note:** If asked, show you *understand* NMEA — GPGGA is GPS fix data (lat/lon/altitude), GPRMC is recommended minimum navigation data (speed/course). This shows you read the assignment fully.

---

### Bonus Criteria — Which You Addressed

| Bonus Criterion | Your Coverage |
|---|---|
| Ease of Installation (+5%) | `requirements.txt` per utility, `.env.example`, clear README setup steps |
| Ease of Use (+5%) | Single search input, immediate feedback, no login required |
| Scalability & Extensibility (+5%) | No hardcoded values, config via `.env`, modular route structure |

---

### Functionality 40% — Demo Readiness

Before the interview, verify these work without errors:
- [ ] Search "Mumbai" → weather loads correctly
- [ ] Search an invalid city → error message shows, app doesn't crash
- [ ] Hourly/Weekly toggle → chart updates
- [ ] Insights page → AI text loads (or fallback text shows)
- [ ] Maps page → Leaflet renders without blank screen
- [ ] CSV upload → columns populate correctly
- [ ] Chart type toggle → graph changes correctly
- [ ] Run from cold start in under 5 minutes

---

### Every Manual Change You Made — Be Ready to Explain

From your README, the key manual changes:
- `_build_weekly_from_slots()` — why midday entry, not first slot of the day
- IST timezone conversion using `timedelta(hours=5, minutes=30)` — why not just `+5`
- `normalize_dataframe()` — encoding fallback chain (`utf-8` → `latin-1` → `cp1252`)
- `fetchForecast()` using `?mode=` instead of `?cnt=` — why the original cnt approach failed
- Gemini fallback mechanism — what happens when `GEMINI_KEY` is absent

---

## 4. Live Coding Preparedness

### Functions They Might Ask You to Walk Through

Know these line by line:

**`app.py` — `/weather` route**
- What `requests.get()` returns
- Why `.json()` and not `.text`
- What `data["main"]["temp"]` path means in OWM response structure
- Why `round()` on temperature

**`app.py` — `unix_to_ist_str()`**
- What a Unix timestamp is
- Why `timezone(timedelta(hours=5, minutes=30))`
- What `strftime("%I:%M %p")` produces

**`static/js/app.js` — `updateWeatherForCity()`**
- The `async/await` chain
- What happens if `fetchWeather()` throws
- Why `window._lastForecastData` is set globally

**`data_utility/app.py` — `normalize_dataframe()`**
- Why multiple encoding attempts
- What `errors='replace'` does
- Why `pd.to_numeric(errors='coerce')` over direct cast

---

### Small Live Change They Might Ask For

**"Add wind direction to the weather card"**

You'd need to:
1. Add `"wind_dir": data["wind"].get("deg", 0)` to `/weather` JSON in `app.py`
2. Add `setText("windDirValue", data.wind_dir + "°")` in `app.js`
3. Add a `<span id="windDirValue">` in the relevant metric card in `index.html`

Know this flow cold — API → Flask → JS → DOM.

---

### File Location Knowledge

Know instantly where these live:
- Flask routes → `weather_utility/app.py`
- Chart initialization → `weather_utility/static/js/charts.js`
- Weather API calls → `weather_utility/static/js/weather.js`
- DOM updates → `weather_utility/static/js/app.js`
- Main dashboard template → `weather_utility/templates/index.html`
- CSV utility entry → `data_utility/app.py`
- Environment variables → `.env` (never committed)

---

### Running From Cold Start in 5 Minutes

Practice this sequence until it's automatic:

```bash
git clone https://github.com/YOUR_USERNAME/weatherintel.git
cd weatherintel/weather_utility
python -m venv .venv
source .venv/bin/activate        # Linux/Mac
# OR .venv\Scripts\activate      # Windows
pip install -r requirements.txt
cp .env.example .env
# Edit .env — add OWM_KEY and GEMINI_KEY
python app.py
# Open http://127.0.0.1:5000
```

For data utility:
```bash
cd ../data_utility
pip install -r requirements.txt
streamlit run app.py
```

---

### `.env` Values — Have These Ready

Keep your actual API keys saved somewhere accessible offline (phone notes, printed sheet). On their machine you'll need to type them into `.env` manually. Don't rely on copy-paste working.

---

## 5. Confidence for First Interview

### "I Don't Know But Here's How I'd Find Out"

This is the correct answer when you genuinely don't know something. Say:

> *"I haven't worked with that specifically, but based on what I know about [related concept], I'd approach it by [logical reasoning]. I'd verify by checking [docs/source/experiment]."*

This shows intellectual honesty + problem-solving instinct — both valued over bluffing.

---

### Think Out Loud

Interviewers want to see your reasoning process, not just the answer. When given a problem:
- State your assumptions out loud
- Walk through your approach before coding
- Say "I'm considering X vs Y because..." before deciding

Silence reads as confusion. Narrated thinking reads as competence.

---

### Your Project Is Already the Proof

You built a multi-page Flask app with live API integration, Chart.js visualization, Leaflet maps, Gemini AI, and a separate Streamlit data utility — in 48 hours. That's the bar cleared. The interview is about confirming you understand what you built, not proving you can build it.

---

### Culture Fit Signals They're Looking For

- Curiosity — do you ask good questions back at them?
- Ownership — do you talk about "I built" vs "the AI built"?
- Awareness — do you know what your code does and doesn't do well?
- Communication — can you explain technical decisions to a non-technical person?

---

### Slow Down When Nervous

Practical technique: before answering any question, take one full breath and repeat the question back briefly — *"So you're asking about how the forecast data is structured..."* — this gives you 3–4 seconds to think and signals active listening.

Speaking 20% slower than feels natural reads as calm and confident to the interviewer.

---

*Good luck. You've built something genuinely strong — walk in knowing that.*