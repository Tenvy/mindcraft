# Ngrok Setup Guide for Mindcraft

This guide will help you expose your Mindcraft MindServer to the internet using ngrok.

## What is ngrok?

Ngrok creates a secure tunnel from a public URL to your local server, allowing you to:
- Access your Mindcraft UI from anywhere
- Share your bot with others remotely
- Test your bot from mobile devices
- Avoid port forwarding and firewall configuration

## Setup Instructions

### 1. Get a Free ngrok Account

1. Go to [ngrok.com](https://ngrok.com/)
2. Sign up for a free account
3. Once logged in, go to [your authtoken page](https://dashboard.ngrok.com/get-started/your-authtoken)
4. Copy your authtoken

### 2. Configure Mindcraft

Open `settings.js` and update the ngrok settings:

```javascript
const settings = {
    // ... other settings ...
    
    // ngrok settings (expose mindserver to the internet)
    "use_ngrok": true,  // ← Change this to true
    "ngrok_auth_token": "YOUR_AUTHTOKEN_HERE",  // ← Paste your token here
    
    // ... rest of settings ...
};
```

### 3. Run Mindcraft

Start Mindcraft normally:

```bash
node main.js
```

When ngrok is enabled, you'll see output like this:

```
MindServer running on port 8080
Starting ngrok tunnel...
============================================================
🌐 ngrok tunnel established!
📍 Public URL: https://abc123.ngrok.io
📍 Local URL: http://localhost:8080
============================================================
```

### 4. Access Your Bot

- **Locally**: http://localhost:8080 (as before)
- **From anywhere**: Use the ngrok URL (e.g., https://abc123.ngrok.io)

## Important Notes

### Free Tier Limitations

The free ngrok tier includes:
- ✅ 1 online ngrok process at a time
- ✅ HTTP/HTTPS tunnels
- ✅ Random URLs (e.g., https://abc123.ngrok.io)
- ⚠️ URLs change each time you restart
- ⚠️ 40 connections/minute limit

### Security Considerations

⚠️ **WARNING**: When using ngrok, your MindServer becomes accessible to anyone with the URL.

**Security recommendations:**
1. Don't share your ngrok URL publicly
2. Keep `allow_insecure_coding` set to `false` (default)
3. Monitor who connects to your server
4. Restart to get a new URL if compromised

### Custom Domains (Paid Plans)

If you upgrade to a paid ngrok plan, you can use custom domains:
- Pro plans: $10/month (3 reserved domains)
- Business plans: $30/month (5 reserved domains)

## Troubleshooting

### "Failed to start ngrok tunnel"

**If you see this error:**

```
Failed to start ngrok tunnel: <error message>
Continuing with local server only...
```

**Common causes:**
1. **No authtoken**: Make sure you added your authtoken to `settings.js`
2. **Invalid authtoken**: Check that you copied it correctly
3. **Network issues**: Check your internet connection
4. **ngrok already running**: Close any other ngrok processes

### ngrok not starting

Make sure the ngrok package is installed:

```bash
npm install @ngrok/ngrok --save
```

### Can't access the ngrok URL

1. Check that the ngrok tunnel is actually running (look for the public URL in logs)
2. Make sure you're using `https://` (not `http://`)
3. Try accessing from an incognito window
4. Check if your firewall is blocking ngrok

## Disabling ngrok

To go back to local-only access:

```javascript
"use_ngrok": false,  // Set back to false
```

## Advanced: Using ngrok CLI

You can also run ngrok manually alongside Mindcraft:

```bash
# In a separate terminal
ngrok http 8080
```

Then keep `use_ngrok: false` in settings.js. This gives you more control over ngrok configuration.

## Support

- ngrok Documentation: https://ngrok.com/docs
- Mindcraft Discord: https://discord.gg/mp73p35dzC
- Mindcraft FAQ: https://github.com/mindcraft-bots/mindcraft/blob/main/FAQ.md
