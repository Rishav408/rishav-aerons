# Part 1: Questions About YOU

*(Note: These are strong template answers. Be sure to personalize them to match your exact experience!)*

### 1. Why AI/ML as a career path
**Sample Answer:** "I chose AI/ML because I am fascinated by how data can be transformed into actionable intelligence. While traditional software engineering is about writing explicit rules, AI is about teaching systems to find patterns we can't easily see. I want to build systems that solve complex, real-world problems—like optimizing sensor data in aerospace or predicting environmental changes—where human intuition alone isn't enough."

### 2. What you found hardest in the project and how you solved it
**Sample Answer:** "The most challenging part was ensuring the Gemini AI integration was robust and didn't break the application if the API failed or returned strangely formatted text. 
**How I solved it:** I implemented strict prompt engineering (telling it exactly what JSON keys to return) and wrapped the API call in a `try/except` block with a hardcoded fallback dictionary. This ensured the user always received a seamless experience, even during rate limits or network failures."

### 3. How you used AI tools — what you generated vs what you wrote manually
**Sample Answer:** "I treat AI tools (like Copilot/ChatGPT) as advanced pair programmers, not as a replacement for understanding. 
- **What I wrote manually:** I designed the architecture, decided on the Flask/Streamlit separation, wrote the core business logic, structured the API fetching, and handled the error edge-cases.
- **What I generated:** I used AI to help write boilerplate code faster (like standard CSS templates or Pandas dataframe setup), to explain cryptic Python errors, and to brainstorm the optimal prompt structure for the Gemini integration."

### 4. What you would improve if given more time
**Sample Answer:** "If I had more time, I would improve three things:
1. **Caching:** I would implement Redis or Flask-Caching to store weather API responses for 10 minutes to reduce OWM API calls and speed up load times.
2. **Testing:** I would write unit tests using `pytest` to automatically verify that my data normalization and JSON parsing functions work correctly under edge cases.
3. **Database Integration:** Instead of just a stateless dashboard, I would add a SQLite or PostgreSQL database to save user preferences, search history, and let them upload multiple CSVs for comparison."

### 5. How you'd scale this to handle 1000 concurrent users
**Sample Answer:** "To scale from a local prototype to 1000 concurrent users:
1. **Web Server:** I would replace the built-in Flask development server with a production WSGI server like Gunicorn, running multiple worker processes.
2. **Load Balancing:** I would deploy the app behind a load balancer (like Nginx) to distribute traffic across multiple containerized instances (using Docker).
3. **Caching & Async:** I would cache external API calls heavily (so 1000 people asking for 'Kolkata' only hits the OWM API once every 10 minutes). For Streamlit, I would optimize data processing by utilizing chunking for large CSVs or pushing heavy Pandas logic to a background task queue like Celery."

### 6. What is the difference between what you built and a production system
**Sample Answer:** "What I built is a functional prototype. A true production system requires:
- **Security:** HTTPS encryption, rate limiting on my own endpoints, and secure secret management (like AWS Secrets Manager instead of a local `.env`).
- **Observability:** Centralized logging and monitoring (like Sentry or Datadog) to alert me when the app crashes or the API fails.
- **CI/CD Pipeline:** Automated testing and deployment scripts (like GitHub Actions) so that pushing new code automatically tests it and safely deploys it to the cloud without downtime.
- **Robust Database:** A proper relational or NoSQL database instead of holding data purely in memory."

---

# Part 2: What's NOT Important

It's just as important to know what *not* to stress about. Based on the role (AI Intern) and the company context (Aeron Systems - embedded/hardware focus), here is why you shouldn't worry about these topics:

### 1. Deep DSA — no LeetCode hard problems expected
- **Why it's not important:** You are interviewing for an AI/Prototyping Intern role at a deep-tech/hardware company, not a backend SWE role at Google. They care if you can wrangle data, use APIs, and build functional prototypes.
- **What to focus on instead:** Basic array manipulation, understanding dictionaries (JSON parsing), and knowing how to write clean, readable Python code.

### 2. System design at Google/Amazon scale
- **Why it's not important:** You won't be asked to design Netflix's global video delivery architecture. Aeron Systems deals with hardware devices, edge computing, and localized dashboards. 
- **What to focus on instead:** Understand the basic Client-Server model (like your Flask app) and how local scripts process hardware/sensor data.

### 3. Deep statistics / probability theory
- **Why it's not important:** Unless the role specifically mentioned "Research Scientist," you don't need to prove complex statistical theorems on a whiteboard.
- **What to focus on instead:** Knowing what mean, median, standard deviation, and missing value imputation are (which you already covered in the Data Utility section).

### 4. Backpropagation math derivations
- **Why it's not important:** You are using pre-trained APIs (Gemini) and likely high-level libraries (Scikit-learn/TensorFlow). Nobody derives calculus by hand in an internship interview for an applied role.
- **What to focus on instead:** High-level intuition. Know what a loss function is and that gradient descent updates weights to minimize that loss.

### 5. CUDA / GPU programming
- **Why it's not important:** Writing low-level C++ code for Nvidia GPUs is a highly specialized skill for AI infrastructure engineers.
- **What to focus on instead:** Knowing the difference between Edge AI (running locally on small chips) vs Cloud AI (calling Gemini over the internet).

### 6. Kubernetes / Docker (unless you used it)
- **Why it's not important:** DevOps and container orchestration are beyond the scope of a standard AI internship project. If it wasn't in your `README`, they won't expect you to be an expert.
- **What to focus on instead:** Just understand the concept of *Virtual Environments* (`venv`) and `requirements.txt` to prove you know how to isolate and share project dependencies.

### 7. Advanced database design
- **Why it's not important:** Your project didn't use a database; it used real-time API fetches and uploaded CSVs. They won't ask you to normalize a SQL schema into 3rd Normal Form.
- **What to focus on instead:** Understand how JSON is structured and how Pandas DataFrames store tabular data in memory.
