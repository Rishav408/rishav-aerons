# Part 1: AI & ML Fundamentals

### 1. Supervised vs Unsupervised vs Reinforcement Learning
- **Supervised Learning:** Learning from labeled data. You provide input data $X$ and correct output $Y$, and the model learns the mapping (e.g., predicting house prices, classifying images).
- **Unsupervised Learning:** Learning from unlabeled data. The model tries to find hidden structures or patterns within the data on its own (e.g., customer segmentation/clustering, anomaly detection).
- **Reinforcement Learning:** Learning via trial and error. An agent takes actions in an environment to maximize cumulative reward (e.g., playing chess, self-driving cars).

### 2. Overfitting, underfitting, bias-variance tradeoff
- **Overfitting (High Variance):** The model learns the training data *too well*, including the noise. It performs great on training data but poorly on unseen test data.
- **Underfitting (High Bias):** The model is too simple and fails to capture the underlying pattern in the training data. It performs poorly on both training and test data.
- **Bias-Variance Tradeoff:** The goal is to find the sweet spot between a model that is too simple (high bias) and one that is too complex (high variance), minimizing total error on unseen data.

### 3. Train/validation/test split — why each exists
- **Training Set:** Used to train the model and update its weights.
- **Validation Set:** Used during training to tune hyperparameters (like learning rate) and evaluate if the model is overfitting. The model doesn't learn directly from it, but we make decisions based on it.
- **Test Set:** A totally unseen dataset used only *once* at the very end to give a final, unbiased estimate of how the model will perform in the real world.

### 4. Loss functions — MSE, Cross-entropy
Loss functions measure how far off a model's prediction is from the actual truth.
- **MSE (Mean Squared Error):** Used for **regression** (predicting continuous numbers). It squares the difference between predicted and actual values, penalizing large errors heavily.
- **Cross-Entropy Loss:** Used for **classification** (predicting categories). It measures the difference between two probability distributions (the predicted probabilities vs. the actual true class, which is a 100% probability for the correct class).

### 5. Gradient descent — basic intuition
Imagine standing blindfolded on a mountain and wanting to reach the lowest valley. You feel the slope of the ground with your feet and take a step downhill. **Gradient descent** is exactly that: an optimization algorithm where the model calculates the gradient (slope) of the loss function with respect to its weights, and updates the weights in the opposite direction of the gradient to minimize the loss.

### 6. Activation functions — ReLU, Sigmoid, Softmax
Activation functions introduce non-linearity into a neural network, allowing it to learn complex patterns.
- **Sigmoid:** Squashes output between 0 and 1. Historically used for binary classification, but suffers from vanishing gradients in deep networks.
- **ReLU (Rectified Linear Unit):** Outputs $x$ if $x > 0$, else 0. Standard default for hidden layers because it's fast to compute and mitigates the vanishing gradient problem.
- **Softmax:** Used in the final layer for multi-class classification. It converts raw output scores (logits) into a probability distribution that sums to 1.

### 7. Neural network forward pass intuition
A forward pass is how a neural network makes a prediction. Input data enters the first layer, gets multiplied by weights, has biases added, and passes through activation functions. This transformed data becomes the input for the next layer. This propagates forward through the hidden layers until it reaches the output layer, producing the final prediction.

### 8. What a transformer is (high level)
A Transformer is a neural network architecture (introduced in "Attention Is All You Need") designed for handling sequential data like text. Unlike older models (RNNs/LSTMs) that process data word-by-word, transformers process entire sequences in parallel. They rely on the **Self-Attention mechanism**, which allows the model to look at other words in the sentence to understand context (e.g., knowing "bank" means a river bank vs. financial bank based on surrounding words).

### 9. What an LLM is — token prediction intuition
A Large Language Model (like Gemini or GPT-4) is fundamentally a massive Transformer trained on a vast amount of text. Its core objective is startlingly simple: **predict the next token** (word or sub-word). By repeatedly predicting the most likely next token given the context of all previous tokens, it generates coherent, context-aware sentences that mimic human intelligence.

### 10. Prompt engineering — zero-shot, few-shot, chain-of-thought
- **Zero-shot:** Asking the model to perform a task without giving it any examples (e.g., "Translate 'Hello' to French").
- **Few-shot:** Providing a few examples of the task within the prompt before asking the final question, helping the model understand the exact format or logic you want.
- **Chain-of-Thought (CoT):** Asking the model to "think step-by-step". By forcing the model to generate intermediate reasoning steps before arriving at an answer, its logic and math performance improve significantly.

### 11. RAG — what it is and why it matters
**Retrieval-Augmented Generation (RAG)** is a technique to improve LLM responses by giving them access to external knowledge. Instead of relying solely on what the model memorized during training, a RAG system first *retrieves* relevant documents from a database based on the user's query, and then *augments* the prompt with those documents. It matters because it solves **hallucinations** and allows the model to answer questions about private, proprietary, or recent data.

### 12. Fine-tuning vs RAG — when to use which
- **Use RAG when:** You need the model to know specific, changing facts (like internal company documents, recent news). It's cheaper, faster to update, and allows tracing answers back to sources.
- **Use Fine-tuning when:** You need the model to learn a specific *style*, *tone*, or *format* (e.g., talking like Shakespeare, outputting strict JSON schema implicitly), or when the task is so niche that prompt engineering isn't enough. It's expensive and bad at memorizing facts.

### 13. Embeddings — what they represent
Embeddings are numerical representations of data (text, images) as dense vectors in a high-dimensional space. Words or sentences with similar meanings are placed closer together in this mathematical space (e.g., "King" and "Queen" are close; "King" and "Apple" are far apart). This is how computers understand the semantic meaning of human language.

### 14. Hallucination — why it happens
Hallucination is when an LLM confidently generates false or fabricated information. It happens because LLMs are fundamentally **probabilistic next-token predictors**, not fact databases. If the training data was conflicting, sparse, or if the model simply guesses a plausible-sounding sequence of words that string together logically but lack factual basis, it hallucinates.

---

# Part 2: Aeron Systems Context

### 1. What Aeron Systems does — aerospace/defense embedded systems
Aeron Systems is an Indian deep-tech company that specializes in building intelligent, rugged electronic systems. Their core focus areas include:
- **Inertial Navigation Systems (INS):** Highly accurate sensors for navigating drones, aircraft, and military vehicles without relying solely on GPS.
- **Internet of Things (IoT):** Connected devices for industrial automation, weather monitoring, and smart agriculture.
- **Defense & Aerospace:** Developing mission-critical embedded hardware designed to withstand extreme environments (vibrations, temperatures, radiation).

### 2. Why AI prototyping matters in aerospace context
In aerospace, hardware is incredibly expensive to build, test, and deploy. Mistakes can cost millions or lead to catastrophic failure. 
- **Prototyping** allows software and AI models to be tested in simulated environments (like testing a Python weather data dashboard or sensor dashboard) before burning them onto silicon or deploying them to a $10M drone.
- It allows rapid iteration of logic (like sensor fusion algorithms) in a high-level language before translating it into hyper-optimized C/C++ for the embedded hardware.

### 3. Edge AI vs Cloud AI — latency differences
- **Cloud AI (Like your Gemini integration):** Data is sent over the internet to a massive server, processed, and sent back. It allows for massive computing power but introduces high **latency** (delay) and requires a constant internet connection. Unacceptable for a missile or a self-driving car.
- **Edge AI:** The AI model runs *locally* directly on the hardware device (the "edge" of the network), like on a drone's internal computer. It has strict power and memory limits but offers **zero latency**, immediate decision-making, and works offline.

### 4. What embedded AI means
Embedded AI is the process of taking complex artificial intelligence models and "shrinking" them so they can run on microcontrollers (MCUs) or specialized chips (like FPGAs or NPUs) within larger mechanical or electronic systems. Think of AI running inside a smart temperature sensor, a robotic arm, or an aircraft's navigation unit, rather than on a laptop or a server.

### 5. Why Python is preferred for AI prototyping
While C and C++ are the kings of embedded systems (because they are fast and touch hardware directly), Python is the king of *prototyping* because:
- **Speed of Development:** You can build a proof-of-concept in hours rather than weeks.
- **Libraries:** Python has the absolute best ecosystem for AI, ML, and data processing (Pandas, TensorFlow, PyTorch, Scikit-learn).
- **Readability:** It allows data scientists and embedded engineers to communicate logic easily before rewriting the final, frozen logic into C++ for the microchip.

### 6. Sensor data processing — relevance to NMEA decoder
Aeron builds hardware that outputs raw sensor data. Processing this data is a core skill.
- **The Relevance:** Hardware devices (like GPS receivers or weather stations) spit out raw, often confusing, serial text data. Software must be built to read this stream, parse it, clean it (similar to your CSV cleaning), and visualize it so humans can make decisions. Understanding how to ingest and normalize data is exactly what you demonstrated in your Data Utility.

### 7. NMEA sentences — GPGGA and GPRMC fields
NMEA 0183 is the standard data format supported by all GPS manufacturers. The data is transmitted in "sentences" starting with `$`.
- **GPGGA (Global Positioning System Fix Data):** Provides 3D location and accuracy data.
  - *Key Fields:* UTC Time, Latitude, Longitude, Fix Quality (e.g., 0=Invalid, 1=GPS fix), Number of Satellites tracked, Altitude above sea level.
- **GPRMC (Recommended Minimum Specific GPS/TRANSIT Data):** Provides the essential 2D navigation data.
  - *Key Fields:* UTC Time, Status (A=Active, V=Void), Latitude, Longitude, Speed over ground (in knots), Track angle (direction of movement), Date.
