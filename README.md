# LiveKit Voice Agent with Python

A production-ready voice agent implementation using LiveKit and Python, featuring advanced conversational AI capabilities and optional telephony integration.

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
git clone <repository-url>
cd livekit-voice-agent-python
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
