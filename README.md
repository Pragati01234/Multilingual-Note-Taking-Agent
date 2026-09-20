# Multilingual Note-Taking Agent

A portfolio-ready FastAPI application that turns an audio recording into:

1. Multilingual speech-to-text using Whisper
2. A concise extractive summary
3. A downloadable PDF containing the summary and transcript

## Architecture

```text
Browser
   |
   v
FastAPI /api/process
   |
   +--> Upload validation
   |
   +--> Faster-Whisper
   |       |
   |       +--> Transcript
   |       +--> Detected language
   |
   +--> Extractive summarizer
   |
   +--> ReportLab PDF
   |
   v
JSON response + PDF download
```

## Tech Stack

- Python 3.10+
- FastAPI
- Uvicorn
- Faster-Whisper
- ReportLab
- HTML/CSS/JavaScript

## Prerequisites

Install Python 3.10 or newer.

FFmpeg is recommended because Whisper may need it for some audio formats.

### Windows

Install FFmpeg and make sure `ffmpeg` is available from Command Prompt:

```bash
ffmpeg -version
```

## Setup

Create and activate a virtual environment:

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Linux/macOS:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Run

```bash
uvicorn app.main:app --reload
```

Open:

```text
http://127.0.0.1:8000
```

API documentation:

```text
http://127.0.0.1:8000/docs
```

## API Example

### POST /api/process

Upload an audio file as multipart form data.

Response:

```json
{
  "job_id": "example",
  "filename": "meeting.mp3",
  "language": "en",
  "transcript": "Example transcript...",
  "summary": "Example summary...",
  "pdf_url": "/api/download/example"
}
```

### GET /health

Returns:

```json
{"status": "ok"}
```

## Design Decisions

### Why Faster-Whisper?

It provides local speech recognition without requiring an external API key and supports multiple languages.

### Why an extractive summarizer?

The project is designed to be runnable without paid APIs. The summarizer selects important sentences based on word-frequency scoring. A production version could replace this component with a transformer-based summarization model or an LLM.

### Security considerations

- Uploaded filenames are not used directly as storage paths.
- Files are assigned random job IDs.
- Uploaded audio is deleted after processing.
- Generated PDFs are stored separately.
- File extensions are validated before processing.

## Future Improvements

- Speaker diarization
- Timestamped transcript
- Transformer/LLM-based abstractive summaries
- Action-item extraction
- Authentication
- Background task queue
- Docker deployment
- Automated tests and CI/CD
- Cloud object storage
- Rate limiting and file-size limits

## Portfolio Description

**Multilingual Note-Taking Agent — Python, FastAPI, Whisper**

Built an end-to-end audio processing application that automatically transcribes multilingual recordings, generates concise notes, and exports a structured PDF. Implemented REST APIs, upload validation, local speech recognition, summarization, PDF generation, temporary-file lifecycle management, and a browser-based interface.

## Important

Do not claim this project is publicly deployed unless you actually deploy it. For an application requiring a public URL, publish this repository on GitHub and optionally deploy the API using a suitable cloud platform.
