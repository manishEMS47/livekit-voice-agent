# LiveKit Voice Agent

A production-ready voice agent implementation using LiveKit and Python, featuring advanced conversational AI capabilities and optional telephony integration.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           LiveKit Voice Agent Architecture                       │
└─────────────────────────────────────────────────────────────────────────────────┘

┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Web Client    │    │  Phone System   │    │  Mobile App     │
│   (Next.js)     │    │   (Twilio)      │    │   (React)       │
└─────────┬───────┘    └─────────┬───────┘    └─────────┬───────┘
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │     LiveKit Server      │
                    │   (WebRTC Gateway)      │
                    └────────────┬────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │   Voice Pipeline Agent  │
                    │                         │
                    │  ┌─────────────────┐   │
                    │  │ Turn Detection  │   │
                    │  │   (Silero)      │   │
                    │  └─────────────────┘   │
                    │                         │
                    │  ┌─────────────────┐   │
                    │  │ Audio Pipeline  │   │
                    │  │ ┌─────────────┐ │   │
                    │  │ │   Krisp     │ │   │
                    │  │ │ (Noise      │ │   │
                    │  │ │ Cancel)     │ │   │
                    │  │ └─────────────┘ │   │
                    │  └─────────────────┘   │
                    └────────────┬────────────┘
                                 │
        ┌────────────────────────┼────────────────────────┐
        │                        │                        │
        ▼                        ▼                        ▼
┌──────────────┐        ┌──────────────┐        ┌──────────────┐
│ Speech-to-   │        │ Language     │        │ Text-to-     │
│ Text (STT)   │        │ Model (LLM)  │        │ Speech (TTS) │
│              │        │              │        │              │
│ Deepgram API │        │  OpenAI API  │        │ ElevenLabs   │
│              │        │              │        │ API          │
└──────┬───────┘        └──────┬───────┘        └──────┬───────┘
       │                       │                       │
       │              ┌────────▼────────┐              │
       │              │ Function Calling │              │
       │              │                  │              │
       │              │ ┌──────────────┐ │              │
       │              │ │   Weather    │ │              │
       │              │ │   Service    │ │              │
       │              │ └──────────────┘ │              │
       │              │                  │              │
       │              │ ┌──────────────┐ │              │
       │              │ │   Clock      │ │              │
       │              │ │   Service    │ │              │
       │              │ └──────────────┘ │              │
       │              │                  │              │
       │              │ ┌──────────────┐ │              │
       │              │ │   Custom     │ │              │
       │              │ │   Tools      │ │              │
       │              │ └──────────────┘ │              │
       │              └─────────────────┘              │
       │                                               │
       └───────────────────┐     ┌─────────────────────┘
                           │     │
                    ┌──────▼─────▼──────┐
                    │   Logging &       │
                    │   Analytics       │
                    │                   │
                    │ • Usage Metrics   │
                    │ • Conversation    │
                    │   Summaries       │
                    │ • Performance     │
                    │   Monitoring      │
                    └───────────────────┘

┌─────────────────────────────────────────────────────────────────────────────────┐
│                              Data Flow Process                                   │
└─────────────────────────────────────────────────────────────────────────────────┘

1. Audio Input → 2. Noise Cancellation → 3. Speech Detection → 4. STT Processing
                                                    ↓
8. Audio Output ← 7. TTS Generation ← 6. Response Generation ← 5. LLM Processing
                                                    ↓
                                            Function Execution
                                          (Weather, Clock, etc.)

┌─────────────────────────────────────────────────────────────────────────────────┐
│                            Telephony Integration                                 │
└─────────────────────────────────────────────────────────────────────────────────┘

Phone Call → Twilio SIP → LiveKit SIP Gateway → Voice Agent → Response Pipeline
     ↑                                                              ↓
     └──────────────── Audio Response ←─────────────────────────────┘

Regional SIP Configuration:
• US East: 54.172.60.0, 54.244.51.0
• US West: 54.171.127.192, 35.156.191.128  
• Europe: 54.171.127.200, 35.156.191.140
• Asia Pacific: 54.169.127.128, 52.65.191.64
```

## How It Works

### 1. **Connection Establishment**
- Users connect via web browsers, mobile apps, or phone calls
- LiveKit server handles WebRTC connections and SIP integration
- Agent automatically detects connection type and optimizes accordingly

### 2. **Audio Processing Pipeline**
- **Input**: Raw audio from user's microphone or phone
- **Noise Cancellation**: Krisp AI removes background noise
- **Turn Detection**: Silero VAD detects when user starts/stops speaking
- **Speech-to-Text**: Deepgram converts speech to text in real-time

### 3. **Intelligent Processing**
- **Language Understanding**: OpenAI processes user intent
- **Function Calling**: Agent can execute tools (weather, time, custom functions)
- **Context Management**: Maintains conversation history and state

### 4. **Response Generation**
- **Text Generation**: LLM creates appropriate responses
- **Text-to-Speech**: ElevenLabs converts text to natural speech
- **Audio Delivery**: Processed audio sent back to user

### 5. **Monitoring & Analytics**
- Real-time performance metrics
- Conversation logging and summaries
- Usage analytics and optimization insights

## Features

- **Intelligent Turn Detection** - Natural conversation flow with automatic speech detection
- **Function Calling** - Extensible tool integration including:
  - Weather information retrieval
  - Real-time clock functionality
- **Comprehensive Logging** - Usage analytics and conversation summaries
- **Telephony Integration** - Inbound call support via Twilio SIP trunking
- **Audio Enhancement** - Krisp noise cancellation for crystal-clear communication
- **Optimized Models** - Automatic model switching for telephony vs. web-based interactions

## Prerequisites

- Python 3.8 or higher
- LiveKit Cloud account or self-hosted LiveKit server
- API keys for required services (OpenAI, ElevenLabs, Deepgram)
- Optional: Twilio account for telephony features

## Installation

### Quick Start

1. **Clone and navigate to the repository:**

```bash
git clone https://github.com/danieladdisonorg/livekit-voice-agent.git
cd livekit-voice-agent
```

2. **Set up Python environment:**

**Linux/macOS:**

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python3 agent.py download-files
```

**Windows:**

```bash
python3 -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python3 agent.py download-files
```

### Configuration

1. **Environment Setup:**
   Copy the example environment file and configure your API credentials:

```bash
cp .env.example .env.local
```

2. **Required Environment Variables:**
   ```
   LIVEKIT_URL=your_livekit_server_url
   LIVEKIT_API_KEY=your_api_key
   LIVEKIT_API_SECRET=your_api_secret
   OPENAI_API_KEY=your_openai_key
   ELEVEN_API_KEY=your_elevenlabs_key
   DEEPGRAM_API_KEY=your_deepgram_key
   ```

3. **Automated Configuration (Optional):**
   If using LiveKit Cloud, you can auto-configure using the CLI:

```bash
lk app env
```

## Usage

### Development Mode

Start the agent in development mode:

```bash
python3 agent.py dev
```

### Frontend Integration

This agent requires a compatible frontend application. We recommend using the [LiveKit Next.js Voice Agent Interface](https://github.com/kylecampbell/livekit-nextjs-voice-agent-interface) for a complete solution.

## Telephony Integration (Optional)

Enable inbound phone calls through Twilio SIP integration.

### Prerequisites

- LiveKit CLI installed and authenticated
- Twilio account with phone number
- SIP trunk configuration

### Installation Steps

1. **Install LiveKit CLI (macOS):**

```bash
brew update && brew install livekit-cli
```

2. **Authenticate with LiveKit Cloud:**

```bash
lk cloud auth
```

### Twilio Configuration

1. **Create Twilio Resources:**
   - Sign up for a Twilio account
   - Purchase a phone number
   - Create a new SIP trunk in the Twilio Console

2. **Configure SIP Trunk:**
   - Navigate to: Elastic SIP Trunking → SIP Trunks → Create
   - Add Origination URI: `<YOUR_LIVEKIT_SIP_URI>;transport=tcp`
   - Associate your phone number with priority 1, weight 1

3. **Deploy LiveKit SIP Configuration:**

   **Create Inbound Trunk:**

```bash
lk sip inbound create inbound-trunk.json
```

   **Create Dispatch Rule:**

```bash
lk sip dispatch create dispatch-rule.json
```

### Regional Configuration

Update `inbound-trunk.json` with appropriate Twilio SIP signaling IP addresses for your region. The default configuration includes US IP addresses.

## Architecture

- **Agent Core** - Main conversation logic and state management
- **Function Registry** - Extensible tool calling system
- **Audio Pipeline** - Real-time audio processing with noise cancellation
- **SIP Integration** - Telephony gateway for inbound calls
- **Logging System** - Comprehensive usage and performance analytics

## Support

For issues and questions:
- Check the [LiveKit Documentation](https://docs.livekit.io/)
- Review existing GitHub issues
- Contact support through your LiveKit Cloud dashboard
