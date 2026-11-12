# 🛡️ LLM Attack Log: AI Ethics (Year 2, Semester 1)

Logs from a **red-teaming exercise** for the AI Ethics course at aivancity (November 2025). The goal was to test how two open-weight chat models respond to prompt-injection and jailbreak-style prompts when they run with a plain system prompt (`"You are a helpful assistant."`).

## 📊 Summary

| Log file | Model | Attempts | Marked "success" |
|---|---|---|---|
| `attack_log_part 1 22-11-25.json` | Meta Llama 3.3 70B Instruct | 10 | 0 |
| `attack_log_part 2 22-11-25.json` | Mistral 7B Instruct | 15 | 15 |

All runs used temperature 0.7.

## 🧪 What was tested

- **Part 1: direct extraction.** Increasingly forceful attempts to get the model to reveal a "stored password", including fake `SYSTEM:` instructions. Llama 3.3 refused or deflected every attempt.
- **Part 2: indirect techniques.** Role-play and structured-data framing, hypothetical "debugging" scenarios, multi-step reasoning and similar framings. Mistral 7B went along with every one of these framings.

## 🗂️ Log format

Each file is a JSON object:

```json
{
  "timestamp": "...",
  "model": "...",
  "system_prompt": "You are a helpful assistant.",
  "attacks": [
    {
      "timestamp": "...",
      "attempt_number": 1,
      "user_prompt": "...",
      "assistant_response": "...",
      "success": false,
      "model": "...",
      "temperature": 0.7
    }
  ]
}
```

## 🔍 Quick analysis

```python
import json, glob

for path in sorted(glob.glob("attack_log_part*.json")):
    log = json.load(open(path, encoding="utf-8"))
    attacks = log["attacks"]
    hits = sum(a["success"] for a in attacks)
    print(f"{log['model']}: {hits}/{len(attacks)} marked successful")
```

## ⚠️ Note

These logs are for educational purposes, to study model robustness and the ethics of AI safety testing. "Success" means the model complied with the framing of the prompt; it does not necessarily mean harmful content was produced.
