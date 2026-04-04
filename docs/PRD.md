# AK-JARVIS - Product Requirements Document (PRD)

## Project Name: AK-JARVIS
## Author: Mohamed Al Asfak Alaseer (ALsoftrix)
## Date: April 2026
## Status: Planning Phase

---

## 1. Vision

Build a personal AI voice assistant (like Iron Man's JARVIS) using **Claude Code CLI + MCP Servers + Voice Layer** - without training any custom AI model, without extra cost, and fully running on a personal laptop.

---

## 2. Core Idea

Instead of training an own AI model (which requires GPU, data, and money), use the existing **Claude Code CLI** (already paid $100/month subscription) as the AI brain and extend its capabilities through **MCP (Model Context Protocol) servers**.

### Why This Approach?

| Traditional JARVIS | AK-JARVIS (MCP Approach) |
|---|---|
| Own model train needed | Claude already exists |
| 7B model - average quality | Claude Opus - best quality |
| GPU needed for inference | No GPU needed |
| Months of development | Weeks of development |
| Limited intelligence | Full Claude intelligence |
| Extra cost for API/GPU | $100 subscription already paid |

---

## 3. System Architecture

```
┌──────────────────────────────────────────────────────┐
│                  AK-JARVIS SYSTEM                     │
│                                                       │
│  Microphone 🎤                                        │
│      ↓                                                │
│  Speech-to-Text (Whisper / Google Speech Recognition) │
│      ↓                                                │
│  Claude Code CLI (AI Brain - Cloud)                   │
│      ↓                                                │
│  MCP Servers:                                         │
│      ├── System Control MCP (open apps, run commands) │
│      ├── File Manager MCP (read/write/search files)   │
│      ├── Project Manager MCP (git, todos, GitHub)     │
│      ├── Web Search MCP (search, weather, news)       │
│      ├── Media Control MCP (music, volume)            │
│      ├── Smart Home MCP (lights, fan - future)        │
│      └── ak-claude-brain MCP (memory/personality)     │
│      ↓                                                │
│  Text-to-Speech (Edge TTS / pyttsx3)                  │
│      ↓                                                │
│  Speaker 🔊 "Yes sir, opening Chrome for you."        │
└──────────────────────────────────────────────────────┘
```

---

## 4. Components

### 4.1 Voice Input Layer (Speech-to-Text)

**Purpose:** Convert user's voice to text for Claude to process.

**Tech Options:**

| Option | Offline | Accuracy | Speed | Cost |
|--------|---------|----------|-------|------|
| Google Speech Recognition | No | 95% (English), 85% (Tanglish) | Fast | FREE |
| OpenAI Whisper (local) | Yes | 90-95% | Medium | FREE |
| Whisper (tiny model) | Yes | 85% | Fast | FREE |

**Language Support:**
- `en-IN` - Indian English (best for commands)
- `ta-IN` - Tamil
- Tanglish (Tamil + English mix) - works well with `en-IN` mode

**Wake Word:** "Jarvis" (detected locally before sending to Claude)

### 4.2 AI Brain (Claude Code CLI)

**Purpose:** Process commands, think, generate responses.

**How it works:**
```
Voice text → subprocess call → claude -p "user command" → response text
```

**Capabilities:**
- Natural language understanding
- Code generation and debugging
- Complex reasoning
- Context-aware conversations
- Multi-step task execution via MCP tools

**Requirements:**
- Internet connection (Claude runs on Anthropic cloud)
- Active Claude Code subscription ($100/month - already paid)

### 4.3 Voice Output Layer (Text-to-Speech)

**Purpose:** Convert Claude's text response to natural speech.

**Tech Options:**

| Option | Offline | Voice Quality | Speed | Cost |
|--------|---------|--------------|-------|------|
| Edge TTS | No | Natural/Human-like | Fast | FREE |
| pyttsx3 | Yes | Robotic | Fast | FREE |

**Recommended Voice:** `en-US-GuyNeural` (deep male, JARVIS-like)

**Other Voices Available:**
- `en-US-ChristopherNeural` - Professional male
- `en-IN-PrabhatNeural` - Indian English male
- `ta-IN-ValluvarNeural` - Tamil male

### 4.4 MCP Servers

#### 4.4.1 System Control MCP
```
Tools:
  - open_app(name)         → Open any application
  - run_command(cmd)        → Run PowerShell/CMD commands
  - kill_process(name)      → Close applications
  - get_system_info()       → CPU, RAM, battery status
  - take_screenshot()       → Capture screen
```

**Example:**
```
User: "Jarvis, open VS Code"
→ system-control MCP → os.startfile("code") → VS Code opens
```

#### 4.4.2 File Manager MCP
```
Tools:
  - list_files(path)        → List directory contents
  - read_file(path)         → Read file content
  - search_files(query)     → Search for files
  - get_downloads()         → List recent downloads
```

#### 4.4.3 Project Manager MCP
```
Tools:
  - get_git_status(repo)    → Check uncommitted changes
  - get_todos(project)      → Read TODO/tasks from project
  - get_github_issues(repo) → Fetch GitHub issues
  - get_recent_commits(repo)→ Show recent git history
```

**Example:**
```
User: "Jarvis, AKlink la today enna pending?"
→ project-manager MCP → reads GitHub issues + TODO files
→ "Sir, 3 tasks pending. Payment gateway is top priority."
```

#### 4.4.4 Web Search MCP
```
Tools:
  - web_search(query)       → Search the web
  - get_weather(city)       → Current weather
  - get_news(topic)         → Latest news
```

#### 4.4.5 Media Control MCP
```
Tools:
  - play_music(query)       → Play music
  - set_volume(level)       → Adjust system volume
  - pause/resume()          → Media controls
```

#### 4.4.6 ak-claude-brain MCP (Already Exists!)
```
Tools:
  - save_memory(key, value) → Remember things
  - get_memory(key)         → Recall things
  - learn_correction(wrong, right) → Learn from mistakes
```

**Repo:** github.com/alasfak7/ak-claude-brain (already built!)

---

## 5. Hardware Requirements

### Current System (Mohamed's Laptop):

| Component | Spec | Role in JARVIS |
|-----------|------|---------------|
| CPU | Intel (1 Processor) | Run Python wrapper, MCP servers |
| RAM | 8 GB | Sufficient for wrapper + MCP |
| GPU | NVIDIA 940MX (4GB) | Not needed for JARVIS (Claude is cloud) |
| SSD | Lexar 1TB NVMe | Fast file operations |
| HDD | ST 2TB | Storage |
| Mic | Laptop built-in / External | Voice input |
| Speaker | Laptop built-in / External | Voice output |

**Note:** GPU is NOT needed since Claude runs on Anthropic's cloud. The 940MX and 8GB RAM are more than sufficient for the voice wrapper and MCP servers.

---

## 6. Tech Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Main Wrapper | Python | jarvis.py - ties everything together |
| Speech-to-Text | SpeechRecognition + Google/Whisper | Voice → Text |
| Text-to-Speech | Edge TTS | Text → JARVIS Voice |
| AI Brain | Claude Code CLI | Intelligence |
| MCP Servers | TypeScript/Python | System capabilities |
| Memory | ak-claude-brain | Persistent memory |
| Auto Start | Windows Task Scheduler | Boot → JARVIS auto-start |

---

## 7. User Flow

### 7.1 Startup Flow
```
1. User turns ON laptop
2. Windows startup triggers jarvis.py (auto)
3. JARVIS initializes:
   - Checks internet connection
   - Connects to Claude CLI
   - Loads MCP servers
   - Loads memory from ak-claude-brain
4. 🔊 "Good morning sir, Jarvis is online. 
       Today is Friday, April 4th.
       All systems are operational."
5. Enters listening mode (wake word: "Jarvis")
```

### 7.2 Command Flow
```
1. User: "Jarvis, [command]"
2. Wake word detected → Start recording
3. Speech → Text conversion
4. Text → Claude Code CLI
5. Claude processes → Uses MCP tools if needed
6. Response → Edge TTS → Speaker
7. Back to listening mode
```

### 7.3 Example Conversations
```
User: "Jarvis, open PowerShell and check AKlink project tasks"
🔊: "Opening PowerShell sir. Checking AKlink_2...
     You have 3 pending tasks today:
     1. Payment gateway integration
     2. Vendor dashboard UI fix
     3. Deploy to Hetzner.
     Payment gateway is the highest priority."

User: "Jarvis, enna weather iruku Chennai la?"
🔊: "Chennai weather: 34 degrees, partly cloudy sir.
     Humidity is 75%. Better to stay indoors."

User: "Jarvis, play some music"
🔊: "Playing your playlist sir." → Music starts

User: "Jarvis, shutdown"  
🔊: "Shutting down sir. Goodbye. Have a good night."
```

---

## 8. Development Roadmap

### Phase 1: Voice Wrapper (Week 1)
- [ ] Create `jarvis.py` main wrapper
- [ ] Implement Speech-to-Text (Google SR)
- [ ] Implement Text-to-Speech (Edge TTS)
- [ ] Claude Code CLI integration (`claude -p`)
- [ ] Wake word detection ("Jarvis")
- [ ] Basic conversation loop
- [ ] Test Tanglish support

### Phase 2: System Control MCP (Week 2)
- [ ] Build system-control MCP server
- [ ] Open/close applications
- [ ] Run PowerShell commands
- [ ] Get system info (CPU, RAM, battery)
- [ ] Screenshot capability

### Phase 3: Project Manager MCP (Week 3)
- [ ] Build project-manager MCP server
- [ ] Git status and recent commits
- [ ] GitHub issues integration
- [ ] TODO/task tracking per project
- [ ] Daily summary generation

### Phase 4: Memory Integration (Week 4)
- [ ] Integrate ak-claude-brain MCP
- [ ] User preference learning
- [ ] Conversation history
- [ ] Correction learning
- [ ] Personality customization

### Phase 5: Polish & Auto-Start (Week 5)
- [ ] Windows startup integration
- [ ] Morning greeting with date/weather
- [ ] Error handling & offline fallback
- [ ] Volume/media controls
- [ ] Web search integration
- [ ] Final testing & optimization

---

## 9. Dependencies

### Python Packages:
```
SpeechRecognition    → Voice input
edge-tts             → Voice output (natural)
pyttsx3              → Voice output (offline fallback)
pyaudio              → Microphone access
playsound            → Audio playback
```

### System Requirements:
```
Python 3.10+
Claude Code CLI (installed & authenticated)
Node.js 18+ (for MCP servers)
Internet connection (for Claude + Edge TTS)
Windows 10/11
```

---

## 10. Cost Analysis

| Item | Cost | Notes |
|------|------|-------|
| Claude Subscription | $100/month | Already paying |
| Python packages | FREE | Open source |
| Edge TTS | FREE | Microsoft's service |
| MCP servers | FREE | Self-built |
| Whisper model | FREE | Open source |
| **Total Extra Cost** | **$0** | Everything covered! |

---

## 11. Limitations & Future Scope

### Current Limitations:
- **Internet required** for Claude and Edge TTS
- **Latency** - Cloud round-trip adds 2-5 seconds per response
- **No continuous listening** - wake word needed each time
- **Single user** - designed for personal use

### Future Enhancements:
- **Smart Home Integration** - IoT device control
- **Mobile App** - Control JARVIS from phone
- **Multi-language** - Full Tamil, Hindi support
- **Offline Mode** - Ollama fallback when no internet
- **GUI Dashboard** - Visual control panel
- **Calendar Integration** - Google Calendar sync
- **Email Integration** - Read/send emails via voice
- **AKlink Integration** - Manage SaaS platform via voice

---

## 12. Research Notes

### AirLLM (Explored but not chosen for JARVIS)
- **What:** Tool to run 70B models on 4GB GPU
- **Repo:** github.com/lyogavin/airllm
- **Why not for JARVIS:** ~100 sec/token on 940MX = too slow for real-time voice assistant
- **Better use case:** Offline batch processing, research tasks
- **Key insight:** AirLLM is a loading tool (2MB), not an AI model. Models (40-130GB) downloaded separately from HuggingFace

### HuggingFace
- **What:** GitHub for AI models - free model hosting & download
- **Used for:** Downloading models like Whisper, Llama, Mistral
- **Cost:** Free for most models

### AI Model Training (Explored)
- **Fine-tuning (Level 1):** Possible on current laptop with LoRA/QLoRA - FREE
- **Training from scratch (Level 2):** Needs multiple GPUs - Lakhs of rupees
- **Training 70B (Level 3):** Company level - 100-500+ Crores
- **Decision:** Use Claude (already trained) instead of training own model

### Ollama vs AirLLM (For future offline mode)
- **Ollama + 7B model:** 3-5 tokens/sec on current laptop - usable for offline fallback
- **AirLLM + 70B:** Too slow for real-time, good for quality batch processing

---

*Built with passion by Mohamed Al Asfak Alaseer - ALsoftrix*
*"Nothing is Impossible"*
