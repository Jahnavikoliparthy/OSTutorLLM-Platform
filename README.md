# OSTutorLLM Web Platform

A classroom demo connecting your existing fine-tuned Qwen3-8B LoRA adapter to a browser frontend, with optional PDF question answering (RAG-style retrieval).

## Project files
- `frontend/index.html` — responsive web UI with prompt entry, PDF upload, answer display and source-page indicators.
- `colab/OSTutorLLM_Colab_API.ipynb` — separate Colab notebook to load your saved adapter, serve the API, index PDFs and generate answers.
- `README.md` — setup instructions.

## Architecture
Browser frontend → FastAPI endpoint exposed by ngrok → Qwen3-8B base model + saved LoRA adapter.
When a PDF is uploaded, the API extracts text using pypdf, splits it into chunks, indexes chunks with TF-IDF, retrieves the top matching passages, and supplies them as context to the fine-tuned model.

## Prerequisites
1. Run your existing fine-tuning notebook and save the adapter in Google Drive at:
   `/content/drive/MyDrive/OSTutorLLM/ostutor_qwen3_8b_300_final`
2. Ensure the adapter directory exists and contains the PEFT adapter files.
3. Use a Colab GPU runtime with enough available memory to load Qwen3-8B in 4-bit mode.
4. Create an ngrok account and obtain an auth token.

## Run the platform
1. Upload `colab/OSTutorLLM_Colab_API.ipynb` to Google Colab.
2. Select a GPU runtime (Runtime → Change runtime type, if available).
3. Run every cell in order. Mount Google Drive when prompted.
4. The notebook loads `Qwen/Qwen3-8B` in 4-bit mode and attaches the adapter saved by your training notebook.
5. When prompted, paste your ngrok token into the hidden input. Do not share it.
6. Copy the printed public API URL.
7. Open `frontend/index.html` in your browser.
8. Paste the API URL into the “Colab API URL” field.
9. Ask an OS question, or upload a text-based PDF and then ask a question about it.

## Notes and limitations
- This notebook serves the model that was already fine-tuned; it does not run training again.
- The adapter path and base model name must match the training notebook.
- The notebook uses a public ngrok tunnel for a demo. Anyone who obtains the URL may be able to call the API. Do not use confidential information. For real deployment, add authentication, request limits, origin restrictions and persistent hosting.
- Colab can disconnect or stop; when it does, the API URL/model may stop working and need to be restarted.
- Uploaded documents and indexes are held in memory and are lost when the Colab runtime restarts.
- PDF text extraction works for PDFs with selectable text. Scanned PDFs need OCR, which is not included.
- Retrieval uses TF-IDF as a simple baseline; it is not a semantic embedding search system.
- Sources are page-level excerpts retrieved from the uploaded PDF. Always verify important answers against the source document.
- The browser's frontend URL is not an authentication boundary. CORS is not a security system.
- This is a demo scaffold. Test it in your Colab environment and adapt it before public or production use.
