🎬 AI Video Assistant

Turn long videos and meetings into searchable, actionable knowledge.

AI Video Assistant is an AI-powered meeting/video intelligence tool built with Python + Streamlit. Give it a YouTube URL or a local audio/video file, and it can transcribe the content, generate a professional summary, extract action items and decisions, identify open questions, and let you chat with the transcript using RAG.

🚀 Live Demo

🔗 Try the Live Demo



✨ What can it do?

Feature

What it does

🎧 Audio Processing

Downloads YouTube audio or converts a local media file to WAV

📝 Transcription

Uses local Whisper for English transcription

🇮🇳 Hinglish Support

Uses Sarvam AI to transcribe/translate Hinglish into English

🏷️ Smart Title

Generates a short professional title from the transcript

📋 Summarisation

Produces a concise meeting summary using an LLM

✅ Action Items

Extracts tasks, owners, and deadlines when mentioned

🔑 Key Decisions

Identifies important decisions made during the meeting

❓ Open Questions

Finds unresolved questions and follow-up topics

🧠 RAG Chat

Ask questions about the transcript and retrieve relevant context

💻 Streamlit UI

Interactive dashboard with pipeline status and chat

🧠 How it works

             YouTube URL / Local Video
                       │
                       ▼
              ┌─────────────────┐
              │ Audio Processing│
              │ yt-dlp / pydub │
              └────────┬────────┘
                       │
                       ▼
                Audio → Chunks
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
       English Input        Hinglish Input
             │                   │
             ▼                   ▼
        OpenAI Whisper       Sarvam AI
             │                   │
             └─────────┬─────────┘
                       ▼
                   Transcript
                       │
          ┌────────────┼─────────────┐
          ▼            ▼             ▼
       Summary      Extraction     RAG Index
          │        / Decisions       │
          │        / Questions       │
          │                          ▼
          │                     ChromaDB
          │                          │
          └──────────────┐           ▼
                         │       Retriever
                         │           │
                         ▼           ▼
                    Streamlit UI ← Mistral
                         │
                         ▼
                  💬 Chat with Video

RAG pipeline

The transcript is split into smaller chunks and converted into embeddings using a Hugging Face embedding model. Those embeddings are stored in ChromaDB. When you ask a question, the system retrieves the most relevant transcript chunks and passes them to Mistral so the answer is grounded in the meeting context.

🛠️ Tech Stack

Frontend

Streamlit — interactive web interface

AI / LLM

OpenAI Whisper — local speech-to-text for English

Sarvam AI — Hinglish speech-to-text + translation

Mistral AI — summarisation, title generation, extraction, and RAG answers

RAG / NLP

LangChain — LLM and retrieval orchestration

ChromaDB — local vector database

Hugging Face / Sentence Transformers — transcript embeddings

Audio / Video

yt-dlp — YouTube audio extraction

pydub — audio conversion and chunking

FFmpeg — media processing backend

📁 Project Structure

AI-Video-Assistant/
│
├── app.py                     # Streamlit application
├── main.py                    # CLI pipeline entry point
├── test.py                    # Pipeline testing script
├── Requirements.txt           # Python dependencies
├── .gitignore
│
├── core/
│   ├── extractor.py           # Action items, decisions, questions
│   ├── rag_engine.py          # RAG pipeline and transcript Q&A
│   ├── summarizer.py          # Summary and title generation
│   ├── transcriber.py         # Whisper / Sarvam transcription
│   └── vector_store.py        # ChromaDB + embeddings
│
└── utils/
    └── audio_processor.py    # Download, conversion, and chunking

🚀 Getting Started

1. Clone the repository

git clone https://github.com/ar2727065-bit/Ai-videos-assist.git
cd Ai-videos-assist

2. Create a virtual environment

python -m venv venv

Windows:

venv\Scripts\activate

macOS / Linux:

source venv/bin/activate

3. Install Python dependencies

pip install -r Requirements.txt

4. Install FFmpeg

FFmpeg must be installed separately because the project uses it for audio/video processing.

Verify your installation:

ffmpeg -version

5. Configure environment variables

Create a .env file in the project root:

MISTRAL_API_KEY=your_mistral_api_key
SARVAM_API_KEY=your_sarvam_api_key

# Optional
WHISPER_MODEL=small
SARVAM_STT_MODEL=saaras:v2.5

🔐 Never commit your .env file or API keys to GitHub.

▶️ Run the Streamlit App

streamlit run app.py

Then open the local URL shown by Streamlit, usually:

http://localhost:8501

Input

In the sidebar, provide either:

YouTube URL

or

Local audio/video file path

Choose:

english

or:

hinglish

and click ⚡ Analyse.

💬 Example Questions

After the video is processed, you can ask questions such as:

What were the main decisions?

Who was responsible for the next task?

What deadlines were mentioned?

What problems were discussed?

What topics still need follow-up?

The RAG assistant is instructed to answer from the transcript context rather than inventing information that is not present in the meeting.

🧩 CLI Usage

The project also includes a command-line pipeline:

python main.py

It will ask for:

Enter YouTube URL or local file path:
Language (english/hinglish):

After processing, the CLI displays the title, summary, action items, decisions, and open questions, followed by an interactive transcript Q&A session.

🔄 Processing Pipeline

1. Input video/audio
        ↓
2. Extract audio
        ↓
3. Convert to WAV
        ↓
4. Split audio into chunks
        ↓
5. Speech-to-text
        ↓
6. Generate title
        ↓
7. Generate summary
        ↓
8. Extract action items
        ↓
9. Extract key decisions
        ↓
10. Extract open questions
        ↓
11. Build vector store
        ↓
12. Retrieve relevant context
        ↓
13. Ask questions with RAG

⚡ Why RAG instead of simply asking an LLM?

For a long meeting transcript, sending the entire transcript to an LLM for every question can be inefficient and may exceed context limits.

This project uses Retrieval-Augmented Generation (RAG):

Transcript
   ↓
Chunking
   ↓
Embeddings
   ↓
ChromaDB
   ↓
User Question
   ↓
Similarity Search
   ↓
Top Relevant Chunks
   ↓
Mistral LLM
   ↓
Grounded Answer

This makes the assistant better suited for asking targeted questions about longer transcripts.

⚠️ Notes

Whisper runs locally and can require significant CPU/GPU resources depending on the selected model.

Sarvam transcription requires a valid SARVAM_API_KEY when using the Hinglish mode.

Mistral features require a valid MISTRAL_API_KEY.

FFmpeg must be available on the system PATH.

Audio is processed in chunks to handle longer recordings.

The local ChromaDB data is stored in the vector_db directory.

🔒 Security

Keep credentials out of source control.

Your .env should remain local:

.env

If an API key is accidentally pushed to GitHub, revoke/rotate it immediately.

🎯 Project Goal

The goal of this project is to turn passive video/meeting content into structured, searchable knowledge — reducing the time spent watching long recordings and making it easier to find what was discussed, decided, and assigned.

📌 Future Ideas

Some natural next steps for the project:

🎙️ Speaker diarization

⏱️ Timestamp-based answers

📄 PDF / Markdown export

🔎 Search across multiple meetings

💾 Persistent meeting history

👥 Speaker-specific summaries

📊 Meeting analytics

☁️ Cloud deployment

🔐 User authentication

⭐ If you find this project useful

Give the repository a ⭐ and feel free to explore, improve, and experiment with the code.

Built with Python, Streamlit, LangChain, Whisper, Sarvam AI, Mistral AI, and ChromaDB. 🤖🎬
