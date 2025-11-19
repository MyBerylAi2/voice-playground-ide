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
Performance Monitoring: Track CPU/RAM usage via psutil, display real-time metrics in dashboard. Log all API calls to /core/app.log with timestamps. Implement 15-second API response timeout for resilience.
API Routing: Provide /chat endpoint accepting JSON with message, voice bool, and model string. Support both local LLM (if GGUF models present) and OpenAI fallback. Return JSON with text, audio_url, and status.
Systemd Service: Create beryl-agentic.service file for background daemon mode, auto-restart on failure, journald logging.
Desktop Integration: Generate .desktop entry in ~/.local/share/applications/ for GNOME launcher with icon path.
Visual Design Specification (Critical):

Implement a wet glass black background (#0a0a0a) with old gold accent color (#d4af37) for buttons and borders. Primary text must be white (#ffffff). Add a 33% gradient effect from upper-left (#0a0a0a) to lower-right (#1a1a2e to #0f0f1a) that phases into next-gen green effect (#00ff88 at 33% from bottom-right corner only). Use CSS background: linear-gradient(135deg, #0a0a0a 0%, #1a1a2e 67%, #00ff88 100%); with mix-blend-mode: screen on overlay elements. Buttons must have transition: all 0.3s ease; with hover { transform: translateY(-2px); box-shadow: 0 5px 15px rgba(212, 175, 55, 0.3); }

File Structure (Mandatory):

markdown
Copy
beryl-agentic/
├── core/
│   ├── main.py (FastAPI backend)
│   ├── venv/ (isolated Python env)
│   └── app.log (runtime logs)
├── ui/
│   ├── dashboard.py (Streamlit UI)
│   └── assets/
│       └── icon.png (gold BERYL logo)
├── voice/
│   └── agents-playground/ (LiveKit clone)
├── secrets/
│   └── .env (auto-generated, chmod 600)
├── tests/
│   └── test_secrets.py (API validation tests)
├── beryl-agentic.service (systemd)
├── INSTALL_BERYL.sh (one-click installer)
└── README.md (Fedora Silverblue setup guide)
Build Instructions for Codex:

Create installer script first: Use rpm-ostree detection, virtualenv isolation, and Seagate path hardening.
Implement secrets manager: Must call each API's test endpoint on load, cache results, expose /secrets/status endpoint.
UI tabs: "Dashboard", "Voice Playground", "API Secrets", "Health" - all must show real-time data.
Voice playground: When tab clicked, check if LiveKit server running on port 3000, if not, spawn subprocess npm start from voice/agents-playground/ directory.
CSS optimization: Minimize Streamlit re-renders by caching expensive operations with @st.cache_resource and @st.cache_data.
Error handling: Wrap all API calls in try/except, return user-friendly messages in UI, never crash backend.
Performance: Add psutil monitoring, display CPU/RAM graphs, timeout all HTTP requests at 15 seconds.
Testing: Create pytest suite that validates secrets without live API calls using mocks.
Deployment Command for Codex:

Generate complete codebase by creating each file structure above, then execute:

bash
Copy
chmod +x INSTALL_BERYL.sh
./INSTALL_BERYL.sh
streamlit run ui/dashboard.py --server.port 8501
Result: Fully functional BERYL AGENTIC IDE with voice playground accessible at http://localhost:8501 with specified wet glass black, old gold, white, and next-gen green gradient UI.
