# LLM Showcase for "generate-node-code-with-llm", using Gradio

A demonstration of executing a Large Language Model (LLM) with PyTorch, Gradio, and Hugging Face libraries. This arrangement features:

- **Model Ingestion & Prediction** (`llm.py`)
- **Assessment Metrics** (`evaluate.py`)
- **Prompt Guidelines** (`prompts.py`)
- **Gradio User Interface** (`gradio_app.py`)
- **Dockerfile** for container-based deployment

---

## Project Organization

```
.
├── app
│   ├── llm.py          # Model loading & inference
│   ├── evaluate.py     # BLEU, ROUGE, and MED evaluation
│   ├── gradio_app.py   # Main Gradio UI
│   └── prompts.py      # Prompt instructions & node lists
├── Dockerfile          # Docker build instructions
├── requirements.txt    # Python dependencies
└── .env                # Environment variables (includes HF_TOKEN)
```

> **Important:** The `.env` file holds a `HF_TOKEN` variable (your Hugging Face access token) utilized for authentication with private model repositories.

---

## Preconditions

1.  **Docker** (for containerized setup).
2.  **NVIDIA GPU** with appropriate drivers (and CUDA support if preferred).
3.  **Hugging Face Token** (if you plan to use private or restricted models).

---

## Rapid Launch

Below are directions to build and operate this project within a Docker container.

```bash
git clone https://github.com/Dev-Sphere111/generate-node-code-with-llm
```

_(Assuming you have this step completed or know where the source code is)_

### 2\. Construct the Docker Image

```bash
docker build -t gradio-llm .
```

### 3\. Generate or Modify `.env` (Optional)

If you possess a Hugging Face token (e.g., for models like `google/gemma-2-2b-it`), verify you have an `.env` file at the project root containing:

```
HF_TOKEN=your_hugging_face_token
```

### 4\. Execute the Docker Container

Substitute `\path\to\.cache\huggingface\hub` with your chosen local directory for caching models. Modify `--gpus all` if your GPU configuration differs or if you intend to use CPU only.

```bash
docker run --name gradio-llm \
  -p 7860:7860 \
  --env HF_TOKEN=your_hugging_face_token \
  -v \path\to\.cache\huggingface\hub:/model_storage \
  --gpus all \
  gradio-llm
```

> **Details:**
>
> - The `--env HF_TOKEN=...` can be omitted if the `.env` file is present for automatic loading.
> - The `-v` flag links your local Hugging Face cache to `/model_storage` inside the container, ensuring persistent model storage.

The Gradio application will be accessible at [http://localhost:7860](https://www.google.com/search?q=http://localhost:7860).

---

## Gradio App Operation

1.  **Navigate** your browser to [http://localhost:7860](https://www.google.com/search?q=http://localhost:7860).
2.  **Input** a text prompt into the _Input Text_ field.
3.  **Select** **Generate Response** to view the LLM’s output.
4.  (Optional) **Provide** an _Expected Output_ to assess using BLEU, ROUGE, and MED metrics.

---

## File Descriptions

### `app/llm.py`

- **load_model(model_name)**: Fetches a Hugging Face model (with 4-bit quantization capability).
- **llm_inference(user_request, model, tokenizer)**: Produces text from a user query employing a prompt template.

### `app/evaluate.py`

- **compute_metrics(predictions, references)**: Calculates BLEU, ROUGE, and Minimum Edit Distance (MED) for contrasting model outputs with reference texts.

### `app/prompts.py`

- **prompt**: An extensive instruction string directing the LLM on response formulation. Includes a catalog of permissible nodes and illustrative examples.

### `app/gradio_app.py`

- **Gradio UI**: Establishes text areas for input, predicted output, expected output, and a metrics display zone. Also incorporates buttons for generating responses, assessing metrics, and clearing input fields.

---

## Verification

Basic **unit tests** and **integration tests** are provided.
To execute them, utilize:

```bash
pytest src/tests
```

If memory constraints arise, run them individually using:

```bash
pytest .\src\tests\test_integration.py

pytest .\src\tests\test_unit.py
```

---

## AI Support Tools:

ChatGPT (with variants like `chatgpt-o1`, `chatgpt-4o`) and GitHub Copilot were employed for debugging and receiving suggestions.

---

## Future Steps and Possible Enhancements

1.  Augment test coverage
2.  Boost processing speed by utilizing more compact models
3.  Refine the presentation of evaluation results in Gradio
4.  Explore alternative evaluation methodologies
5.  Reorganize model name and configuration settings
