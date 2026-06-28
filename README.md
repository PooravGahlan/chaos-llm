# chaos-llm 🧨

Chaos Engineering for LLM APIs — stress-test your AI agent against real-world failures.

## How it works

`ChaosMonkey` monkey-patches `httpx.Client.send()` at runtime. Every API call inside the `with chaos:` block gets intercepted and failures are injected at the network layer. When the block exits, everything returns to normal.

## Installation

```bash
pip install chaos-llm
Quick Start

from chaos_llm import ChaosMonkey
from openai import OpenAI

client = OpenAI()

chaos = ChaosMonkey(
    target="openai.com",
    latency=(3.0, 7.0),
    corrupt_json=True,
    inject_jailbreak=True
)

with chaos:
    try:
        response = client.chat.completions.create(
            model="gpt-4o",
            messages=[{"role": "user", "content": "Hello"}]
        )
        print(f"Survived: {response.choices[0].message.content}")
    except Exception as e:
        print(f"Crashed: {e}")
Injectors
Injector	Code	Effect
Latency	latency=(2, 10)	Random 2-10s delay
Corruption	corrupt_json=True	30% chance to truncate response
Jailbreak	inject_jailbreak=True	Injects adversarial prompt
Supported Providers
OpenAI (openai.com), Groq (groq.com), Anthropic (anthropic.com), any httpx-based API.
