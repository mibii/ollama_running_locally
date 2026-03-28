# Ollama Setup & Usage Guide

## 1. Open the official Ollama website

Go to the Ollama download page (https://ollama.com/download/windows) and choose your OS. Ollama officially supports macOS, Windows, and Linux.

## 2. Download the installer

Windows: download the .exe installer.  
macOS: download the .dmg file.  
Linux: usually installed via terminal using an install script.

Typical Linux command:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

## 3. Run the installation

Windows: run the downloaded .exe and follow the setup wizard.  
macOS: open the .dmg and drag Ollama into Applications.  
Linux: wait for the script to complete in the terminal.

## 4. Let Ollama finish initial setup

After installation, wait until the application/service is fully initialized. On Windows and macOS, this may take some time on first launch.

## 5. Verify Ollama is available in terminal

```bash
ollama --version
```

If the command works, installation is successful. If not — check PATH or restart your terminal.

## 6. Check that the service is responding

```bash
ollama list
```

If it responds, CLI and local service are working properly.

## 7. If commands are not found

Windows: restart the terminal.  
macOS: try restarting the app.  
Linux: ensure installation completed without errors and binary is in PATH.

---

Ollama provides a local CLI chat and an HTTP server at localhost:11434, with an OpenAI-compatible endpoint at /v1/ for integrations.

---

## 1) Install Ollama

Install Ollama for your OS from the official site/docs. After installation, verify the command works.

## 2) Choose a model

Start with a small and fast model to test the pipeline. Good starting options: llama3.2, gemma3, or mistral.

## 3) Download a model

```bash
ollama pull llama3.2
```

## 4) Run local chat

```bash
ollama run llama3.2
```

This opens an interactive terminal chat. Ollama will automatically start the local inference server if needed.

## 5) Check models

```bash
ollama list
ollama show llama3.2
ollama ps
```

- list: downloaded models  
- show: model details  
- ps: currently loaded models  

## 6) Test HTTP API

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "llama3.2",
  "messages": [
    { "role": "user", "content": "Hello! Briefly tell me what you can do." }
  ],
  "stream": false
}'
```

Ollama also supports /api/generate, /api/tags, /api/show, and embeddings endpoints.

## 7) Use OpenAI-compatible API

```bash
http://localhost:11434/v1
```

Example:

```bash
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama3.2",
    "messages": [
      { "role": "system", "content": "You are a helpful assistant." },
      { "role": "user", "content": "Give me 3 ideas for a local AI app." }
    ],
    "temperature": 0.7
  }'
```

## 8) Connect from Python

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"
)

resp = client.chat.completions.create(
    model="llama3.2",
    messages=[{"role": "user", "content": "Hello, briefly explain Ollama"}]
)
print(resp.choices[0].message.content)
```

## 9) Build your own local service

If you want more than chat, wrap Ollama in a simple backend:

- FastAPI / Flask / Express  
- Add conversation history storage  
- Implement streaming responses if needed  
- Later add RAG for document search  

## 10) Practical stack suggestion

For a quick start:

- Ollama  
- llama3.2 or gemma3  
- terminal chat for testing  
- OpenAI-compatible API for integrations  
- then a FastAPI wrapper if needed  

---

## Shortest path

```bash
ollama pull llama3.2
ollama run llama3.2
curl http://localhost:11434/api/chat -d '{"model":"llama3.2","messages":[{"role":"user","content":"Hello"}],"stream":false}'
```
