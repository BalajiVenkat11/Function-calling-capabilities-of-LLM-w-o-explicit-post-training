🔮 LLM Function Calling Experiment
Comparing TinyLlama-1B, Qwen2.5-7B, and Mistral-7B-Instruct-v0.2 on a Fake Gem API

This project benchmarks how different open-source LLMs behave when forced into a function-calling workflow using a very simple Flask-like API pattern:

{"function": "get_gem", "args": {"moon_sign": "<value>"}}

The goal:

➡️ Determine which models reliably produce strict JSON outputs
➡️ Identify whether non-function-calling-trained models can still behave like tool-calling models
➡️ Evaluate their usefulness for structured API workflows

🔧 Tools Used

Python + Transformers (Hugging Face)

Flask-style fake API for simulating tool-calling workflows

GPU inference (Colab / local)

N-shot prompting (no retries, no guardrails)

Identical prompt across all models

Temperature = 0 for strict obedience testing

| Model                        | Params | Function Calling Training?  | JSON Obedience        | API Workflow Success      | Notes                                                                                                                      |
| ---------------------------- | ------ | --------------------------- | --------------------- | ------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **TinyLlama-1B**             | 1B     | ❌ No                        | ⚠️ Low                | ❌ Frequent failures       | Good for minimal setups, but struggles with strict schemas and often adds extra text.                                      |
| **Qwen2.5-7B Instruct**      | 7B     | ✅ Yes (native tool calling) | ✅ Very High           | ✅ Excellent               | Designed for function calling; consistently produces perfect JSON.                                                         |
| **Mistral-7B-Instruct-v0.2** | 7B     | ❌ No                        | ⭐ *Surprisingly High* | ⭐ Best first-try response | Not trained for tool calling but behaves almost like it is — highly obedient, minimal hallucination, extremely clean JSON. |

🧪 Key Finding

Mistral-7B-Instruct-v0.2 — which has no function-calling training — produced the cleanest, strictest JSON on the very first attempt.

This is unexpected because Qwen2.5-7B is explicitly trained for tool calling.

Why did Mistral outperform?

Strong instruction obedience

Good prior training on JSON patterns

Low hallucination behavior

Very predictable decoder behavior

Responds cleanly to "return ONLY JSON" constraints

In single-shot scenarios, Mistral acted almost exactly like a function-calling model.

📌 Conclusion

Even models without function-calling fine-tuning can reliably act as function callers if:

The schema is simple

The system instruction is strict

Temperature is low

Prompt structure is consistent

Mistral-7B-Instruct-v0.2 is an excellent choice for lightweight JSON or API workflows without requiring gated models or special tool-calling datasets.
