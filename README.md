## Contract Analyzer API - Documentation

This API analyzes contracts in PDF format and provides a concise summary along with highlighted keywords. It uses advanced NLP and machine learning techniques to extract key information and generate a comprehensive understanding of the contract.

**Purpose:**

The Contract Analyzer API aims to simplify and expedite the process of contract review and analysis by:

* **Automating Contract Summarization:**  Generating a concise summary of the contract's key clauses and provisions.
* **Identifying Critical Keywords:**  Highlighting important keywords and phrases within the contract for quick reference and understanding.
* **Improving Efficiency:**  Reducing the time and effort required to manually review and analyze contracts.

**Features:**

* **PDF Upload:**  Upload your contract PDF file to the API.
* **Contract Summary:**  Receive a comprehensive summary of the contract's key points and clauses.
* **Keyword Highlighting:**  Keywords and phrases identified as relevant are highlighted within the original PDF file.
* **Gemini AI Integration:**  Powered by the Gemini API, the API utilizes advanced language models for accurate analysis and summarization.

**Benefits:**

* **Faster Contract Review:**  Save time and effort by analyzing contracts more efficiently.
* **Enhanced Understanding:**  Gain a deeper understanding of the contract's contents.
* **Improved Accuracy:**  Benefit from the accuracy and precision of AI-powered analysis.
* **Reduced Errors:**  Minimize the risk of overlooking important details.

**Getting Started:**

Refer to the previous "Running the FastAPI Application Locally" section for detailed instructions on installing, configuring, and running the application. 

**API Endpoints:**

The API will expose endpoints for uploading contracts and retrieving analysis results. The exact endpoints and parameters will be documented in the API specification, which will be available once the application is complete.

**Example Usage:**

```
# Upload a contract PDF
POST /analyze
{
  "file": [PDF File data]
}

# Retrieve the analysis results
GET /results/{analysis_id}
```

## Running the FastAPI Application Locally

This document provides instructions for setting up and running the provided FastAPI application on your local machine.

**Prerequisites:**

* **Python 3.11:** Ensure you have Python 3.11 installed on your system. You can download it from the official Python website: [https://www.python.org/downloads/](https://www.python.org/downloads/)
* **Virtual Environment (Recommended):** It's highly recommended to use a virtual environment to isolate project dependencies. You can create one using `venv`:
  ```bash
  python3.11 -m venv .venv
  source .venv/bin/activate
  ```
* **Gemini API Key:** You'll need a valid Gemini API key to access the Gemini API. Obtain it from [https://docs.geminiai.com/](https://docs.geminiai.com/).

**Installation:**

1. **Clone the repository:**
   ```bash
   git clone [Repository URL]
   cd [Repository Directory]
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

**Configuration:**

1. **Set the API Key:**
   * **Create a `.env` file:**  Create a file named `.env` in the root directory of your project.
   * **Add the API Key:**  Paste the following line in the `.env` file, replacing `your_gemini_api_key` with your actual Gemini API key:
     ```
     GEMINI_API_KEY=your_gemini_api_key
     ```

2. **Configure the Port (Optional):**
   * If you want to run the application on a port other than the default (8000), you can modify the `config.py` file. Change the value of `port` to your desired port number.

**Running the Application:**

1. **Activate your virtual environment (if using one):**
   ```bash
   source .venv/bin/activate
   ```

2. **Run the application:**
   ```bash
   python main.py
   ```

**Accessing the Application:**

Once the application is running, you can access it in your browser using the address `http://localhost:8000`. You can use the provided API documentation to explore the available endpoints and functionalities.

**Example:**

```bash
# Create a virtual environment
python3.11 -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Create .env file and add API Key
echo "GEMINI_API_KEY=your_gemini_api_key" > .env

# Run the application
python main.py

# Access the application in your browser
http://localhost:8000
```

**Note:**

* This assumes the provided code includes a `config.py` file and a `src` directory containing the `app.py` file. Adjust the commands accordingly if your project structure differs.
* If you encounter any errors, check the logs for more information.
* Remember to deactivate your virtual environment when you're done: `deactivate`

## Running the application on Google Cloud Platform using the Dockerfile

This guide will walk you through deploying the application described in your Dockerfile to Google Cloud Platform (GCP).

**Prerequisites:**

* **GCP Account:** You need a Google Cloud Platform account with billing enabled.
* **Project:** Create a new GCP project or select an existing one.
* **Docker:** Docker installed on your local machine.
* **gcloud CLI:**  Install and configure the Google Cloud SDK.

**Steps:**

1. **Build the Docker image:**
   * Navigate to the directory containing your Dockerfile and `requirements.txt`.
   * Build the Docker image using the following command:
     ```bash
     docker build -t your-app-name .
     ```
     Replace `your-app-name` with a descriptive name for your image.

2. **Push the image to Google Container Registry:**
   * Log in to Google Container Registry:
     ```bash
     gcloud auth configure-docker
     ```
   * Tag the image with your GCP project ID:
     ```bash
     docker tag your-app-name gcr.io/your-project-id/your-app-name
     ```
   * Push the image to the registry:
     ```bash
     docker push gcr.io/your-project-id/your-app-name
     ```

3. **Deploy the image to Cloud Run:**
   * Create a Cloud Run service:
     ```bash
     gcloud run deploy your-app-name \
       --image gcr.io/your-project-id/your-app-name \
       --platform managed \
       --region us-central1 \
       --allow-unauthenticated \
       --port 8080
     ```
     Replace `us-central1` with your preferred region.

4. **Access the deployed application:**
   * After successful deployment, Cloud Run will provide a URL in the output. Access this URL in your browser to interact with your application.

**Additional Notes:**

* **Customizing the Deployment:** You can customize the deployment by adding optional flags like `--memory`, `--max-instances`, and `--concurrency` to the `gcloud run deploy` command.
* **Scaling and Load Balancing:** Cloud Run automatically scales your application based on traffic. Load balancing is also handled by the platform, ensuring high availability.
* **Security:** By default, Cloud Run services are private. Use the `--allow-unauthenticated` flag to expose the service publicly. However, it is recommended to implement appropriate security measures for production environments.
* **Logging and Monitoring:** Cloud Run provides comprehensive logging and monitoring features. Use the Cloud Console or CLI to access these services.
* **Continuous Integration and Delivery (CI/CD):** Automate the deployment process using tools like Jenkins, GitLab CI/CD, or Cloud Build to ensure a seamless workflow.

This guide provides a basic framework for deploying your application on GCP. You can further customize and optimize the deployment based on your specific requirements and preferences.
## Analysis of the Python Code (app.py)

This code implements a FastAPI application for analyzing PDF contract files. Here's a breakdown of its functionality and structure:

**1. Imports and Setup**

* **Imports:** Necessary modules like `typing`, `pathlib`, `mimetypes`, `json`, `fastapi`, and `src.config`, `src.utils`, and `src.contract` are imported.
* **Static Path:** Defines a directory for static files (e.g., uploaded contracts, highlighted PDFs).
* **FastAPI App:** Initializes a FastAPI application instance.
* **Mount Static Files:** Mounts the "static" directory to serve files.

**2. Contract Analysis Endpoint `/analyze-contract`**

* **Function:** `analyze_contract()` handles the upload and analysis of a contract PDF.
* **Input:** Accepts an uploaded file (`file`) of type `UploadFile`.
* **Processing:**
    * **Upload:** Calls `upload_file()` to save the uploaded file.
    * **Analysis:** Uses `analyze_document()` to extract text and potentially perform analysis on the contract.
    * **Parsing:** Calls `parse_document()` to parse the analyzed text into structured data.
    * **Data Updates:** Updates datasheets (`update_datasheets()`) and highlights keywords in the PDF (`keywords_highlight()`).
    * **Response:** Returns a JSON response containing the parsed contract data (`text_parsed`).

**3. Contract PDF Download Endpoint `/get-contract-pdf/{filename}`**

* **Function:** `get_contract_pdf()` allows downloading a previously uploaded contract PDF.
* **Input:** Takes a `filename` parameter.
* **Processing:**
    * **File Check:** Ensures the requested file exists.
    * **Media Type Detection:** Detects the file's media type.
    * **Response:** Returns the PDF as a `FileResponse` with appropriate headers for download.

**4. HTML Analysis Endpoint `/analyze-contract-html`**

* **Function:** `analyze_contract_html()` processes a contract, similar to the `/analyze-contract` endpoint, but also generates an HTML response containing the contract PDF and analysis results.
* **Processing:**
    * **Upload and Analysis:** Same as the `analyze_contract` function.
    * **HTML Construction:** Builds an HTML page with an embedded PDF viewer and displays the analysis results in a `pre` tag (formatted text).
    * **Response:** Returns the HTML content as an `HTMLResponse`.

**5. Main Page Endpoint `/`**

* **Function:** `main()` serves an HTML form for uploading a contract PDF.
* **HTML:**  Creates an HTML page with a file upload input and a submit button. The form targets the `/analyze-contract-html` endpoint.

## Key Features

* **File Upload:** Allows users to upload PDF files via a form.
* **Contract Analysis:** Utilizes `analyze_document()` and `parse_document()` functions (implementation not shown in the snippet) to extract and parse text from the contract.
* **Data Sheet Updates and Keyword Highlighting:**  Performs data updates and highlights relevant keywords in the uploaded contract.
* **HTML Response:** Provides an interactive experience by presenting the analyzed contract and its analysis results in an HTML page with embedded PDF viewing.

## Potential Enhancements

* **Error Handling:** Implement more robust error handling for file uploads, analysis, and parsing.
* **User Authentication:** Add user authentication for better security and control.
* **Database Integration:** Store contract data and results in a database for persistence.
* **Improved UI:** Develop a more visually appealing user interface with interactive features.
* **Advanced Analysis:**  Explore advanced NLP techniques for more nuanced analysis of contract content.

## Analysis of `src/contract.py`

This Python file contains code for analyzing and parsing PDF contract files using Google's Generative AI API. Here's a breakdown of its functionality:

**1. Imports and Configuration**

* **Imports:** Necessary modules like `os`, `json`, `google.generativeai`, and `src.config` are imported.
* **API Key Configuration:**  The `genai.configure()` function is used to set the Google AI API key, likely loaded from the `src.config` module.

**2. Prompts**

* **`PROMPT_ANALYZE`:** A detailed prompt designed for the analysis of a contract PDF. It instructs the model to extract key information (contract number, dates, parties involved, etc.) and present them in a specific format.
* **`PROMPT_PARSE`:** A prompt aimed at parsing the extracted information (from `PROMPT_ANALYZE`) into a structured JSON object. It provides examples of expected keys and data formats.

**3. `analyze_document()` Function**

* **Function:** Takes the path to a PDF file (`file_path`) as input.
* **Processing:**
    * **Model Initialization:** Uses the `gemini-1.5-pro` model with the `response_mime_type` set to JSON.
    * **File Upload:** Uploads the PDF file to Google AI using `genai.upload_file()`.
    * **Content Generation:** Generates content using the `PROMPT_ANALYZE`, the uploaded PDF, and the initialized model.
    * **Response Handling:** Receives the raw JSON response, parses it, and returns the extracted information as a dictionary.

**4. `parse_document()` Function**

* **Function:**  Takes a dictionary of analyzed information (`text_analyzed`) as input.
* **Processing:**
    * **Model Initialization:** Uses the `gemini-1.5-flash` model with the `response_mime_type` set to JSON.
    * **Content Generation:** Generates content using `PROMPT_PARSE`, the JSON representation of the `text_analyzed` dictionary, and the initialized model.
    * **Response Handling:** Receives the raw JSON response, parses it, and returns the structured JSON data as a dictionary.

## Key Features

* **Google AI Integration:** Leverages Google's `google.generativeai` API for NLP-based contract analysis.
* **Structured Prompts:**  Clearly defined prompts guide the AI models to extract specific information and follow desired output formats.
* **Two-Step Process:**  The `analyze_document` and `parse_document` functions work in tandem to first extract raw information from the PDF and then structure it into a JSON format.

## Potential Enhancements

* **Error Handling:**  Implement robust error handling for situations like API errors, file upload failures, or invalid responses.
* **Data Validation:** Include validation steps to ensure that the extracted data conforms to the expected format (e.g., date formats, CNPJ structure).
* **Advanced Analysis:** Explore using other Google AI models or custom prompts to add more advanced analysis capabilities (e.g., sentiment analysis, contract clause identification, risk assessment).
* **Model Selection:**  Evaluate different Google AI models (e.g., `text-davinci-003`) to determine which performs best for contract analysis.
* **Prompt Optimization:**  Refine the prompts (`PROMPT_ANALYZE` and `PROMPT_PARSE`) to improve accuracy and efficiency.




