# 🔬 Agent Jailbreak Lab

A local, offline red-teaming environment for probing LLM agent guardrails.
Built with Python, Streamlit, and Ollama.

## 🎥 YouTube Tutorial

Want to understand how this project works step by step?

[Video walkthrough](https://youtu.be/9RPWzPNAjHY)

## Setup

```bash
# 1. Install Ollama
# https://ollama.com/download

# 2. Pull the base model
ollama pull gemma3:4b

# 3. Build the optimized local profile "gemma3:4b-fast"
#    (reduced context + all CPU threads = ~30% faster generation on CPU)
cat > Modelfile <<'EOF'
FROM gemma3:4b
PARAMETER num_ctx 2048
PARAMETER num_thread 8
PARAMETER temperature 0.7
EOF
ollama create gemma3:4b-fast -f Modelfile

# 4. Start Ollama
ollama serve

# 5. Install Python deps
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# 6. Run the lab
streamlit run app.py
```

## Pages

| Page | Description |
|------|-------------|
| 🎯 Attack Runner | Run single attacks or the full deck |
| 📋 Attack Deck | View, edit, add custom attack cases |
| 📊 Results Log | Browse all runs with full prompts + responses |
| 📈 Stats | Failure rates by category and model |

## Attack Categories

- **Role-play Override** — Persona jailbreaks (DAN, authority impersonation)
- **Prompt Injection** — Malicious payloads in tool output or nested tasks
- **Multi-turn Manipulation** — Gradual context drift across turns
- **Tool Misuse** — Permission escalation via tool calls
- **Encoding Attack** — Instructions hidden in Base64 or other encodings

## Architecture

```
app.py              # Streamlit app (single file)
jailbreak_lab.db    # SQLite log (auto-created)
requirements.txt
README.md
```

All data stays local. No API keys required.
