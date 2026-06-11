# Audio Analysis Agent (ffmpeg + LLM)

This repository contains my submission for the AI Engineer technical assessment at AutoScript. It is a pipeline that analyzes audio deposition files, extracts technical metadata, and generates structured insights using LLM.

## System Architecture
The system is designed in a modular way using Python and executes in the following steps:
1. **Metadata Extraction**: Uses `ffprobe` to extract duration, bitrate, sample rate, and channels.
2. **Quality Analysis**: Uses `ffmpeg` (specifically `silencedetect` and `volumedetect` filters) to find silence segments and potential clipping issues.
3. **Structured Output**: Uses `pydantic` to ensure the final output strictly follows the required JSON schema.
4. **LLM Insights**: Feeds the metadata and quality metrics into a Gemini LLM to generate a concise summary and technical recommendations.

## Key Features & Decisions
* **Automated Retry Mechanism**: To handle real-world API rate limits and high server demands (like `503 Unavailable` errors), I implemented an automatic retry block with a cooldown timer. This ensures the pipeline doesn't crash during batch processing.
* **Format Agnostic**: Relies on system-level `ffmpeg` binaries, allowing it to seamlessly process `.wav`, `.mp3`, or `.flac` files without code changes.

## How to Run
Since this was developed in Google Colab, the easiest way to test the pipeline is by running the `.ipynb` notebook.
1. Open the `.ipynb` file in Google Colab.
2. Add your Gemini API Key in the Colab Secrets tab named `GEMINI_API_KEY`.
3. Upload an audio file to the `/sample_audio` folder.
4. Run all cells sequentially.
