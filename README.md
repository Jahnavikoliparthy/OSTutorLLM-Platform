# OSTutorLLM Platform

An Operating Systems tutoring web platform that connects a fine-tuned Qwen3-8B LoRA model to a browser-based interface. It also supports optional PDF question answering using TF-IDF-based retrieval.

## Project Files

- `index.html` — browser frontend for entering questions, uploading PDFs, and viewing answers.
- `OSTutorLLM_FineTuning.ipynb` — notebook for dataset preparation, Qwen3-8B fine-tuning with LoRA, and evaluation.
- `OSTutorLLM_API.ipynb` — notebook for loading the saved adapter and running the FastAPI inference service.
- `README.md` — project documentation.

## Architecture

**Browser frontend → FastAPI → ngrok tunnel → Qwen3-8B + LoRA adapter**

For PDF-based questions, the API extracts text using `pypdf`, divides it into chunks, retrieves relevant passages using TF-IDF, and supplies the retrieved context to the model.

## Technologies Used

- Python and Google Colab
- Qwen3-8B
- Hugging Face Transformers, PEFT and LoRA
- 4-bit NF4 quantization
- FastAPI and Uvicorn
- ngrok
- HTML, CSS and JavaScript
- pypdf and TF-IDF retrieval

## Setup and Execution

### 1. Fine-tune the model

1. Open `OSTutorLLM_FineTuning.ipynb` in Google Colab.
2. Configure the required dependencies and GPU runtime.
3. Prepare the training and test datasets.
4. Run the notebook cells to fine-tune Qwen3-8B using LoRA.
5. Save the adapter to Google Drive at:

   `/content/drive/MyDrive/OSTutorLLM/ostutor_qwen3_8b_300_final`

The dataset contains 300 records: 270 training examples and 30 held-out test examples.

### 2. Start the API

1. Open `OSTutorLLM_API.ipynb` in Google Colab.
2. Select a GPU runtime with sufficient available memory.
3. Run the notebook cells in order.
4. Mount Google Drive and load the saved adapter.
5. Enter your ngrok authentication token when prompted. Never commit or share this token.
6. Copy the generated public API URL.

### 3. Open the frontend

1. Open `index.html` in your browser, or use the deployed frontend if available.
2. Enter the current API URL in the frontend's API URL field.
3. Submit an Operating Systems question.
4. If PDF question answering is enabled, upload a supported PDF and ask a question about its content.

## Evaluation

The model was evaluated on 30 held-out questions using a manual semantic rubric:

- 2 points: fully correct
- 1 point: partially correct
- 0 points: incorrect

The recorded result was 59/60, or **98.33% manual semantic score**. This is a preliminary human-assessed score, not conventional classification accuracy or an automated semantic-similarity metric.

## Limitations

- Evaluation uses a small set of 30 questions.
- The semantic score depends on manual judgments.
- Colab and ngrok are temporary; the API may stop when the runtime disconnects.
- PDF extraction supports selectable text; scanned PDFs require OCR.
- TF-IDF retrieval is a baseline, not semantic embedding-based search.
- Answers generated from retrieved passages should be checked against the source document.
- The demo should not be used with confidential data.
- Public deployment requires appropriate authentication, request limits, origin restrictions, and secure hosting.

## Security

Do not commit API keys, ngrok tokens, passwords, private datasets, or model credentials. Keep secrets in runtime prompts or secure environment variables.

## Future Improvements

- Expand the OS dataset and held-out evaluation set.
- Implement reproducible automated evaluation metrics.
- Improve PDF retrieval using semantic embeddings.
- Add authentication and deploy the inference API to persistent hosting.

## Authors

- Koliparthy Venkata Jahnavi
- Dwarampudi Vaibhav Reddy

## Disclaimer

This project is an educational prototype intended for demonstration and experimentation. Model responses may contain errors and should be verified for important academic or technical use.
