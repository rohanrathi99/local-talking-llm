# OpenAI Setup Guide

This guide explains how to use OpenAI instead of Ollama for the voice assistant.

## Changes Made

✅ **Replaced Ollama with OpenAI**
- Changed from `OllamaLLM` to `ChatOpenAI`
- Updated dependencies in `pyproject.toml`
- Added API key handling

## Installation

1. **Install the updated dependencies:**
   ```bash
   pip install langchain-openai
   # or if using uv:
   uv sync
   ```

2. **Get your OpenAI API Key:**
   - Go to https://platform.openai.com/api-keys
   - Create a new API key
   - Copy the key (you won't be able to see it again!)

3. **Set up the API Key (choose one method):**

   **Option 1: Hardcoded in app.py (Easiest)**
   - Open `app.py`
   - Find the line: `OPENAI_API_KEY_HARDCODED = "YOUR_OPENAI_API_KEY_HERE"`
   - Replace `YOUR_OPENAI_API_KEY_HERE` with your actual API key
   - Save the file
   - ⚠️ **Warning**: This is less secure but more convenient for local development

   **Option 2: Environment Variable (Recommended for production)**
   ```bash
   # Windows PowerShell:
   $env:OPENAI_API_KEY="your-api-key-here"
   
   # Windows CMD:
   set OPENAI_API_KEY=your-api-key-here
   
   # Linux/Mac:
   export OPENAI_API_KEY="your-api-key-here"
   ```

   **Option 3: Command Line Argument**
   ```bash
   python app.py --api-key your-api-key-here
   ```

   **Priority Order**: Command line argument > Environment variable > Hardcoded key

## Usage

### Basic Usage
```bash
python app.py
```

### With Different Models
```bash
# Use GPT-4o (fastest and cheapest)
python app.py --model gpt-4o-mini

# Use GPT-4 Turbo (more capable)
python app.py --model gpt-4-turbo

# Use GPT-3.5 Turbo (cheapest)
python app.py --model gpt-3.5-turbo
```

### Full Example
```bash
python app.py \
    --model gpt-4o-mini \
    --voice path/to/voice_sample.wav \
    --exaggeration 0.7 \
    --cfg-weight 0.3 \
    --save-voice
```

## Available OpenAI Models

- `gpt-4o` - Latest GPT-4, fastest and most capable
- `gpt-4o-mini` - Smaller, faster, cheaper version (recommended)
- `gpt-4-turbo` - Previous generation GPT-4
- `gpt-3.5-turbo` - Cheapest option, still very capable

Default: `gpt-4o-mini`

## Flow with OpenAI

The exact same flow as described in the README:

1. **User speaks** → Audio recorded
2. **Speech → Text** → Whisper transcribes
3. **Text sent to LLM** → `get_llm_response()` called
4. **History loaded** → Previous conversation retrieved
5. **Prompt prepared** → System + History + User input
6. **Prompt sent to OpenAI** → HTTP request to OpenAI API
7. **LLM generates answer** → OpenAI returns response
8. **History updated** → New messages saved
9. **Emotion analyzed** → Response checked for emotion
10. **Text → Speech** → ChatterBox TTS converts to audio
11. **Audio played** → User hears response

## Important Notes

⚠️ **API Costs**: OpenAI charges per token used. Monitor your usage at https://platform.openai.com/usage

⚠️ **Internet Required**: Unlike Ollama, OpenAI requires an internet connection

⚠️ **Privacy**: Your conversations are sent to OpenAI's servers. Review their privacy policy.

✅ **No Local Setup**: You don't need to install or run Ollama anymore

✅ **Better Models**: Access to GPT-4 and other advanced models

## Troubleshooting

**Error: "OpenAI API key not found"**
- Make sure you set the `OPENAI_API_KEY` environment variable
- Or use the `--api-key` argument
- Check that the key is correct (starts with `sk-`)

**Error: "Rate limit exceeded"**
- You've hit OpenAI's rate limits
- Wait a moment and try again
- Consider upgrading your OpenAI plan

**Error: "Insufficient quota"**
- Your OpenAI account has no credits
- Add credits at https://platform.openai.com/account/billing

## Comparison: Ollama vs OpenAI

| Feature | Ollama | OpenAI |
|---------|--------|--------|
| Setup | Requires installing Ollama | Just need API key |
| Cost | Free | Pay per use |
| Internet | Not required | Required |
| Privacy | 100% local | Data sent to servers |
| Speed | Depends on hardware | Fast, cloud-based |
| Models | Limited to downloaded models | Access to all GPT models |

