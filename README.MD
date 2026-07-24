# AI Roadmap Generator

DEPLOYMENT LINK: https://batterys-roadmap-generator.onrender.com/
## Overview
The AI Roadmap Generator is a Flask-based web application designed to create comprehensive, step-by-step learning paths for any given topic or interest. By leveraging the high-speed Groq API and advanced Large Language Models (LLMs), this tool provides users with structured, actionable, and timeline-based guides to master new skills.

## Key Features

*   **Intelligent Roadmap Generation:** Utilizes state-of-the-art LLMs to generate structured learning plans complete with clear headings, bullet points, and estimated timelines.
*   **Robust Model Fallback Mechanism:** Implements a sequential retry system. If the primary model fails or experiences rate limits, the application automatically attempts to process the request using alternative supported models, ensuring high reliability.
*   **Supported Models:**
    *   llama-3.1-8b-instant
    *   llama-3.3-70b-versatile
    *   meta-llama/llama-guard-4-12b
    *   openai/gpt-oss-120b
*   **Automated Storage:** Every successfully generated roadmap is automatically saved to a local `roadmaps/` directory as a text file, organized with precise timestamps for easy retrieval and historical tracking.
*   **Connectivity Diagnostics:** Includes a dedicated testing endpoint to verify API key validity and model connectivity without needing to trigger a full generation request.
*   **Comprehensive Error Logging:** Detailed server-side logging tracks initialization states, model fallbacks, and complete traceback data for seamless debugging.

## Project Structure

Based on the repository layout, the project maintains the following structure:

```text
AI_ROADMAP_GENERATOR/
├── roadmaps/               # Directory where generated roadmap text files are saved
├── templates/              
│   └── index.html          # Frontend user interface
├── .env                    # Environment variables (API keys)
├── .gitignore              # Ignored files for Git version control
├── app.py                  # Main Flask application and API logic
├── README.md               # Project documentation
└── requirements.txt        # Python dependencies
```

## Prerequisites
*   Python 3.8 or higher
*   A valid Groq API Key

## Installation and Setup

1.  **Clone the repository:**
    Ensure you have pulled the codebase to your local machine, whether you are running it on a standard OS or customizing it on a distribution like Arch Linux.

2.  **Install dependencies:**
    Install the required Python packages using pip:
    ```bash
    pip install -r requirements.txt
    ```
    *(Note: Ensure your `requirements.txt` includes `Flask`, `groq`, and `python-dotenv` based on the imports in the application).*

3.  **Environment Configuration:**
    The application relies on environment variables for secure credential management. Open the `.env` file in the root directory of the project and ensure your Groq API key is set:
    ```env
    GROQ_API_KEY=your_actual_api_key_here
    ```

## Usage

1.  **Start the application:**
    Run the Flask server from your terminal:
    ```bash
    python app.py
    ```
2.  **Access the web interface:**
    Open your web browser and navigate to `http://127.0.0.1:5000/`. Enter a topic of interest (e.g., "Full-stack JavaScript development" or "Machine Learning") to generate your custom roadmap.

## API Endpoints Reference

*   `GET /`: Renders the main `index.html` frontend.
*   `GET /test_groq`: A diagnostic endpoint that runs a minimal token request to confirm the Groq client is properly initialized and the primary model is responsive. Returns JSON with status and model details.
*   `POST /generate_roadmap`: The core generation endpoint. 
    *   **Requires:** Form data containing an `interest` field.
    *   **Behavior:** Processes the prompt, iterates through the fallback models if necessary, and saves the output to the `roadmaps/` folder.
    *   **Returns:** A JSON payload containing the roadmap text, the saved filename, and the specific model utilized.
