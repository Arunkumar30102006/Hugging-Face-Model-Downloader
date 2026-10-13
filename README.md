# 🤗 Hugging Face Model Downloader & Local Runner

A lightweight mini-project demonstrating how to download AI models directly from the [Hugging Face Hub](https://huggingface.co/models), store them organized locally, and run inference both via an interactive **Terminal Chat** and a **FastAPI** web service.

---

## 📌 Features

- **Any Hugging Face Model**: Download any compatible open-source model using its Hugging Face repository ID.
- **Smart Directory Organization**: Automatically formats and saves models into clean, separate local folders (`model/<author>_<model_name>`).
- **Interactive Terminal Chat**: Chat with the downloaded model directly in your command line.
- **FastAPI Web Service**: Query the model via standard HTTP REST endpoints.
- **GitHub Ready**: Includes a configured `.gitignore` so gigabytes of model checkpoints and weights (`.safetensors`, `.bin`) are **never** accidentally pushed to GitHub.

---

## 📁 Project Structure

```text
Hugging_Face_Model/
├── .gitignore              # Excludes model weights, virtual environments, and cache
├── requirements.txt        # Required Python dependencies
├── download_and_run.py     # Downloads model and starts interactive terminal chat
├── api.py                  # Serves the downloaded model via FastAPI
├── model/                  # [Ignored by Git] Contains downloaded model folders
│   └── HuggingFaceTB_SmolLM2-135M-Instruct/
└── README.md               # Project documentation
```

---

## ⚙️ Requirements & Installation

### 1. Prerequisites
- **Python**: 3.9 or higher
- **Git**

### 2. Set Up a Virtual Environment (Recommended)

Open your terminal in the project directory:

```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

Install all required packages from `requirements.txt`:

```bash
pip install -r requirements.txt
```

> **Packages installed:**
> - `huggingface_hub`: For downloading repositories and models from Hugging Face.
> - `transformers`: Hugging Face pipeline and model loading engine.
> - `torch`: PyTorch backend for running tensor computations.
> - `fastapi` & `uvicorn`: High-performance web framework for the API endpoint.

---

## 🚀 How to Download & Run Any Model

### Step 1: Find a Model on Hugging Face
1. Go to [Hugging Face Models](https://huggingface.co/models).
2. Filter or search for text-generation models (e.g., small instruction-tuned models like `HuggingFaceTB/SmolLM2-135M-Instruct`, `Qwen/Qwen2.5-0.5B-Instruct`, or `TinyLlama/TinyLlama-1.1B-Chat-v1.0`).
3. Click the **Copy** icon next to the model name to copy its ID.

---

### Step 2: Configure `download_and_run.py`
Open `download_and_run.py` and replace `model_name` with your chosen model ID:

```python
from huggingface_hub import snapshot_download
from transformers import pipeline

# 1. Specify your target model from Hugging Face:
model_name = "HuggingFaceTB/SmolLM2-135M-Instruct"

# 2. The script replaces '/' with '_' so the folder name is safe and organized:
model_folder = "model/" + model_name.replace("/", "_")
```

#### How the Folder Path Works:
| Model Name | Local Folder Path |
| :--- | :--- |
| `HuggingFaceTB/SmolLM2-135M-Instruct` | `model/HuggingFaceTB_SmolLM2-135M-Instruct` |
| `Qwen/Qwen2.5-0.5B-Instruct` | `model/Qwen_Qwen2.5-0.5B-Instruct` |
| `TinyLlama/TinyLlama-1.1B-Chat-v1.0` | `model/TinyLlama_TinyLlama-1.1B-Chat-v1.0` |

---

### Step 3: Run the Download and Interactive Chat
Run the script in your terminal:

```bash
python download_and_run.py
```

1. The script downloads the model weights and tokenizer into the local folder.
2. It loads the model into a `transformers` pipeline.
3. It launches an interactive chat session:

```text
Downloading HuggingFaceTB/SmolLM2-135M-Instruct ...
Model saved in model/HuggingFaceTB_SmolLM2-135M-Instruct

You: What is machine learning?
AI: Machine learning is a branch of artificial intelligence where computers learn patterns from data...
You: exit
```

Type `exit` anytime to end the chat.

---

## 🌐 Running as a FastAPI Web Service

You can also expose your downloaded model over an HTTP API.

1. Open `api.py` and verify `model_name` points to the model you downloaded:
   ```python
   model_name = "HuggingFaceTB/SmolLM2-135M-Instruct"
   model_folder = "model/" + model_name.replace("/", "_")
   ```

2. Start the FastAPI server:
   ```bash
   python api.py
   ```

3. Test the endpoint:
   - **Browser / cURL**: Open `http://127.0.0.1:8000/ask?question=Hello%20who%20are%20you`
   - **Interactive API Docs (Swagger UI)**: Visit `http://127.0.0.1:8000/docs`

#### Sample Response:
```json
{
  "answer": "Hello! I am SmolLM, an AI assistant ready to help you."
}
```

---

## 🛡️ Git & GitHub Protection (Important)

Model files can be hundreds of megabytes or several gigabytes. GitHub has a strict file size limit (100MB per file).

This repository includes a `.gitignore` file that automatically excludes:
- `model/` (all downloaded weights, safetensors, and checkpoints)
- `.venv/` (Python virtual environment)
- `__pycache__/` (compiled bytecode)

Only your source code and documentation will be tracked and pushed to GitHub!

---

## 📄 License
This mini-project is open-source and free to use for learning and experimentation.
