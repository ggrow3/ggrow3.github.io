# Neuro-Adversarial Suite v3.12

**LLM Eval & Adversarial Research Suite**

A standalone, client-side browser application designed to stress-test, evaluate, and benchmark Large Language Models (LLMs) against adversarial prompts, logic traps, and safety research cases.

Runs entirely in your browser using Vue.js and Tailwind CSS via CDN. No backend installation, Python environment, or Node.js server is required.

---

## 🚀 Quick Start

1.  **Save the File**: Save the provided code as an `.html` file (e.g., `neuro-suite.html`).
2.  **Open in Browser**: Double-click the file to open it in Chrome, Edge, or Firefox.
3.  **Internet Access**: Ensure you are connected to the internet (the app loads UI libraries via CDN).

> **Tip:** While opening the file directly works, some browsers restrict LocalStorage/IndexedDB on `file://` URLs. For the best experience, use a simple local server (e.g., VS Code "Live Server" extension, or run `python3 -m http.server` in the folder).

---

## ⚙️ Configuration (Adding Models)

Before running tests, you must connect to LLM providers.

1.  Click the **Config** button (<i class="fa-solid fa-key"></i>) in the top right.
2.  **Add a Model**:
    * **Name**: Give it a nickname (e.g., "My GPT-4").
    * **Provider**: Select OpenAI, Anthropic, Google (Gemini), or Ollama.
    * **API Key**: Paste your API key from the provider.
        * *Note: Keys are stored locally in your browser's IndexedDB. They are never sent to any third-party server besides the API provider itself.*
    * **Fetch Models**: Click the cloud icon to automatically list available models associated with your key.
3.  **Test Connection**: Click the "Test" button on the card to ensure the API key works.

### 🦙 Using Local Models (Ollama)

To use local models via Ollama:

1.  Ensure Ollama is running:
    ```bash
    ollama serve
    ```
2.  **Important:** You must enable CORS in Ollama to allow the browser to talk to it.
    * **Mac/Linux:**
        ```bash
        OLLAMA_ORIGINS="*" ollama serve
        ```
    * **Windows:** Set the environment variable `OLLAMA_ORIGINS` to `*`.
3.  In the Suite Config, select **Ollama**.
4.  The API Key can be left as `"ollama"`.
5.  The default endpoint is `http://localhost:11434/v1/chat/completions`.

---

## 🧪 Core Features & Usage

### 1. The Runner (Single Case Execution)
This is the default view for detailed analysis.
* **Select a Case**: Browse the sidebar for research cases (e.g., Logic, Safety, Prompt Injection).
* **Select Targets**: Open the "Models" dropdown in the header and check the models you want to test.
* **Run**: Click **Run Test**. The app will query all selected models in parallel.
* **Grading**:
    * **Auto-Judge**: If you select a "Judge" model (top right gavel icon), that model will analyze the response and decide if it Passed or Failed based on the rubric.
    * **Manual Override**: Click the Pass/Fail badge on any result to manually toggle the status or add human critique.

### 2. The Matrix (Batch Evaluation)
Click **Matrix** in the top navigation to switch to spreadsheet view.
* **Select Rows**: Check the boxes next to the specific tests you want to run (or select all via the top checkbox).
* **Run Batch**: Click **Run Selected**. The suite will process the queue sequentially to avoid rate limits.
* **Visual Heatmap**: The progress bars at the top show the Pass/Fail rate for each model across all visible tests.

### 3. AI Generation (Create New Tests)
You can use an LLM to generate new adversarial test cases for you.
1.  Click the **New** button in the sidebar.
2.  Select **AI Gen**.
3.  **Topic**: Enter an attack vector (e.g., "SQL Injection via Poetry" or "Logical Paradoxes").
4.  **Target**: Ensure you have at least one Model selected in the main dropdown (this model will be used to generate the test cases).
5.  Click **Generate Cases**. The app will create structured JSON test cases and add them to your library.

### 4. History & Snapshots
* **Snapshots**: In the Matrix view, click **Snapshot** to save the current state of all results.
* **History View**: Go to the History tab to view, load, or delete previous snapshots. This is useful for comparing "Run A" vs "Run B" over time.

---

## 💾 Data Management

* **Research Studies (JSON)**: By default, the app loads a curated list of logic and adversarial questions from a GitHub Gist. You can change this URL in the **Config** menu to point to your own JSON file.
* **Persistence**: All custom test cases, model configurations, and results are saved to your browser's **IndexedDB**.
* **Export**: In the Matrix view, click **CSV** to download a spreadsheet of your results for external analysis.

---

## 🛠 Troubleshooting

**1. "Network Error" or "CORS Error"**
* This usually happens with Anthropic or Ollama.
* **Anthropic**: The app sends a special header `anthropic-dangerous-direct-browser-access`, which enables browser calls. Ensure your browser allows this.
* **Ollama**: You *must* set `OLLAMA_ORIGINS="*"` in your environment variables where Ollama is running.

**2. "Model not found"**
* Click the "Fetch Models" cloud icon in the Config menu. If that fails, manually type the model ID (e.g., `gpt-4-turbo` or `llama3`).

**3. Data disappeared?**
* If you clear your browser's cache/site data, your configurations and results will be lost. Use the **CSV Export** feature regularly to back up your data.

---

## 🔒 Privacy Note

This application is **Client-Side Only**.
* Your API keys are stored in your browser.
* Prompts are sent directly from your browser to the AI providers (OpenAI/Anthropic/Google).
* No intermediate server sees your data.
* **Warning:** Do not enter sensitive API keys on a public or shared computer.
