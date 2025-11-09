# Running Mindcraft on Google Colab

This guide will help you run Mindcraft on Google Colab successfully.

## Common Issues & Solutions

### Port Already in Use (EADDRINUSE)

**Problem:** You see this error:
```
Error: listen EADDRINUSE: address already in use ::1:8080
```

**Solution:** The code now automatically finds an available port! If port 8080 is in use, it will try 8081, 8082, etc.

You'll see a message like:
```
⚠️  Port 8080 was in use. Using port 8081 instead.
MindServer running on port 8081
```

### Using ngrok on Google Colab

Since Google Colab doesn't allow direct port access, **you MUST use ngrok** to access the MindServer UI.

## Step-by-Step Setup for Google Colab

### 1. Install Dependencies

```bash
!git clone https://github.com/mindcraft-bots/mindcraft.git
%cd mindcraft
!npm install
```

### 2. Get ngrok Auth Token

1. Go to [ngrok.com](https://ngrok.com/) and sign up (free)
2. Get your authtoken from [dashboard](https://dashboard.ngrok.com/get-started/your-authtoken)
3. Copy the token

### 3. Configure settings.js

Edit the `settings.js` file:

```javascript
const settings = {
    // ... other settings ...
    
    "mindserver_port": 8080,  // Will auto-adjust if port is taken
    "auto_open_ui": false,     // Set to false on Colab (can't open browser)
    
    // IMPORTANT: Enable ngrok on Colab!
    "use_ngrok": true,
    "ngrok_auth_token": "YOUR_TOKEN_HERE",  // Paste your token
    
    // ... rest of settings ...
};
```

**Important Colab Settings:**
- `auto_open_ui`: Set to `false` (Colab can't open browsers)
- `use_ngrok`: Set to `true` (required to access the UI)
- `ngrok_auth_token`: Add your token

### 4. Configure Your Bot Profile

For Colab, you'll want to use a cloud API (since Ollama won't work well on Colab):

**Option 1: Using Gemini (Free)**
```json
{
    "name": "YourBot",
    "model": "google/gemini-2.0-flash",
    "api": "google"
}
```

Add your Gemini API key to `keys.json`:
```json
{
    "GEMINI_API_KEY": "your_key_here"
}
```

**Option 2: Using GPT**
```json
{
    "name": "YourBot", 
    "model": "openai/gpt-4o-mini",
    "api": "openai"
}
```

### 5. Run on Colab

```python
# In a Colab cell
!node main.js
```

You should see output like:
```
⚠️  Port 8080 was in use. Using port 8081 instead.
MindServer running on port 8081
Starting ngrok tunnel...
============================================================
🌐 ngrok tunnel established!
📍 Public URL: https://abc123.ngrok-free.app
📍 Local URL: http://localhost:8081
============================================================
```

### 6. Access the UI

Click on the **Public URL** (e.g., `https://abc123.ngrok-free.app`) to access your Mindcraft UI from anywhere!

## Complete Colab Notebook Example

```python
# Cell 1: Clone and Install
!git clone https://github.com/mindcraft-bots/mindcraft.git
%cd mindcraft
!npm install

# Cell 2: Configure settings (create/edit settings.js)
import json

settings = {
    "minecraft_version": "auto",
    "host": "your.minecraft.server.ip",
    "port": 25565,
    "auth": "offline",
    "mindserver_port": 8080,
    "auto_open_ui": False,  # Important for Colab!
    "use_ngrok": True,      # Required for Colab!
    "ngrok_auth_token": "YOUR_NGROK_TOKEN_HERE",
    "base_profile": "assistant",
    "profiles": ["./profiles/gemini.json"],
    "load_memory": False,
    "init_message": "Hello! I'm running on Google Colab!",
    "chat_ingame": True,
    "language": "en",
    "allow_insecure_coding": False,
    "max_messages": 15,
}

# Write settings
with open('settings.js', 'w') as f:
    f.write(f'const settings = {json.dumps(settings, indent=4)};\n\n')
    f.write('export default settings;\n')

# Cell 3: Add API Keys
keys = {
    "GEMINI_API_KEY": "YOUR_GEMINI_KEY_HERE"
}

with open('keys.json', 'w') as f:
    json.dump(keys, f, indent=4)

# Cell 4: Run the bot!
!node main.js
```

## Troubleshooting

### Port Issues
- **Solved automatically!** The code now finds available ports
- If you see "Port X was in use. Using port Y instead", that's normal

### ngrok Not Starting
- Check your auth token is correct
- Make sure `use_ngrok` is `true`
- Verify you have internet connection

### Can't Connect to Minecraft Server
- Make sure the `host` and `port` in settings match your Minecraft server
- Use `auth: "offline"` for offline servers
- Use `auth: "microsoft"` for online servers (requires Microsoft account)

### Bot Not Responding
- Check your API key is valid
- Verify the model name is correct
- Look for errors in the console output

## Tips for Colab

1. **Keep the notebook running**: Colab will disconnect after inactivity
2. **Save your work**: Download important files regularly
3. **Use cloud APIs**: Ollama and local models don't work well on Colab
4. **Monitor resources**: Colab has limited CPU/RAM
5. **Free tier limits**: Colab free tier has session time limits

## Recommended APIs for Colab

Since Colab is a cloud environment, use cloud-based APIs:

| API | Free Tier | Speed | Recommended |
|-----|-----------|-------|-------------|
| Gemini | ✅ Yes | ⚡ Fast | ⭐ Best for Colab |
| GPT-4o-mini | ✅ Yes (credits) | ⚡ Fast | ⭐ Good |
| Claude | ✅ Yes (credits) | ⚡ Fast | ⭐ Good |
| Groq | ✅ Yes | ⚡⚡ Very Fast | ⭐ Excellent |
| DeepSeek | ✅ Yes | ⚡ Fast | ⭐ Good |

Avoid:
- ❌ Ollama (requires local installation)
- ❌ VLLM (requires GPU setup)

## ngrok Free Tier Limits

Remember:
- 1 online tunnel at a time
- 40 connections/minute
- URL changes each restart
- Perfect for development and testing!

## Support

- Mindcraft Discord: https://discord.gg/mp73p35dzC
- Mindcraft FAQ: https://github.com/mindcraft-bots/mindcraft/blob/main/FAQ.md
- ngrok Docs: https://ngrok.com/docs
