# voice-playground-ide
IDE for prototyping voice models 
Project Context & Introduction:

You are building BERYL AGENTIC, a Fedora Silverblue-optimized AI agent IDE with integrated voice playground capabilities. This is a full-stack application combining a FastAPI backend, Streamlit frontend, LiveKit real-time voice agents, and enterprise-grade secrets management. The system must run entirely from an external Seagate drive path /mnt/chromeos/removable/Seagate/beryl-devkit/beryl-agentic to maintain isolation on immutable Linux systems.

Core Architecture:

The application follows a three-tier structure:

Backend Core (/core/main.py): FastAPI server running on port 8000, handles LLM inference, API routing, secrets validation, and health monitoring. Must initialize a Python virtual environment at /core/venv and keep all dependencies isolated.
Frontend UI (/ui/dashboard.py): Streamlit dashboard running on port 8501, provides IDE interface, navigation tabs, LiveKit voice playground iframe, real-time system metrics, and secrets management panel.
Voice Module (/voice/agents-playground/): Embedded LiveKit Agents Playground for real-time conversational AI, accessible via navigation tab.
Feature Requirements:

One-Click Installer: Create /INSTALL_BERYL.sh that detects Fedora Silverblue via rpm-ostree command, installs system deps using appropriate package manager, sets up isolated venv, clones LiveKit playground, and creates systemd service file.
Secrets Management: Auto-generate /secrets/.env template on first run. Implement APISecretManager class that validates ALL API keys (LiveKit, OpenAI, Deepgram, ElevenLabs) on first context load by making live HTTP calls to each provider's test endpoint. Display ✅/❌ status in UI with "Alive/Dead" indicators.
Voice Playground Integration: Embed LiveKit playground at http://localhost:3000 as fullscreen iframe accessible via "🎤 Voice Playground" navigation tab in Streamlit sidebar. Auto-launch playground server when tab is accessed.


