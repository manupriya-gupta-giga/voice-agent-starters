# Voice agent starters

Two small starting points for Manupriya Gupta's Aaron Swartz Day workshops:

1. **Low-touch travel assistant:** choose a location, ask where to go next, and open walking directions.
2. **Build a voice agent for your community:** answer simple questions using a tiny, editable set of community facts.

**Using this repo is optional.** Bring your own tools, reuse one part, or just follow the conversation and design exercises. No particular model or vendor is required for the workshop.

## Run it

Requires Python 3.10 or newer. No packages to install, accounts to create, build step or database.

```sh
git clone https://github.com/manupriya-gupta-giga/voice-agent-starters.git
cd voice-agent-starters
python3 server.py
```

Open **http://localhost:8000**. On Windows, use `py server.py` if `python3` is unavailable. If port 8000 is busy, use `python3 server.py --port 8001`.

Choose an example, type a question and press **Send**. Try **Speak** to fill the input from your microphone, or enable **Read replies aloud**. Review recognized words before sending them. Stop audio stops listening and playback. Typed input works even when browser speech is unavailable.

Demo mode uses simple scripted replies, not a language model. It is useful for getting the interface running and discussing the interaction before connecting AI.

## Workshop 1: a low-touch travel assistant

Start with a visitor at one of three Golden Gate Park landmarks. Select the current stop and ask, “Where should I go next?” The starter suggests the next stop in a fixed sample sequence. A separate link opens Google Maps walking directions when clicked.

Make one change:

- Replace the three stops in `examples.json` with your own small itinerary.
- Ask the visitor whether they want a quiet stop, food or a museum before suggesting the next place.
- Add a transport preference, then decide what trustworthy route information you would need.

The starter does not geocode arbitrary addresses, locate you, optimize routes, check opening hours or provide live turn-by-turn navigation. Those are optional extensions. A text message saying a different location does not override the selected stop.

## Workshop 2: a voice agent for your community

Switch to the community example. Ask, “When is the workshop?”, “Where is it?” or “Is it accessible?” The sample schedule and venue are **fictional**, not information about Aaron Swartz Day.

Make one change:

- Replace `community.answers` and its matching keywords in `examples.json` with a few verified facts about your group.
- Add an unanswered question and decide how the agent should handle missing information.
- Have someone interrupt a spoken reply, change their mind or ask two questions at once. Decide what you would improve.

The scripted keyword matcher is deliberately basic. It returns the first matching topic and does not understand follow-up questions. Optional AI mode supports a short conversation history. Either way, the demo cannot register people, book anything, send messages or transfer a call.

## Optional: connect one AI service

The included adapter makes one HTTPS request to the Anthropic Messages API using Python's standard library. This is optional, replaceable and not a sponsorship or recommendation to purchase credits.

In your terminal, set your own API key and an available model ID from your account, then restart the server:

```sh
export ANTHROPIC_API_KEY='your-own-key'
export ANTHROPIC_MODEL='an-available-model-id'
python3 server.py
```

On Windows PowerShell, set `$env:ANTHROPIC_API_KEY` and `$env:ANTHROPIC_MODEL` instead. The server does not automatically load `.env` files.

Enable **Use the optional AI connection** in the page. Requests can incur charges. The key stays on the Python server and is never served to the browser. Edit `prompts.md` to change instructions; edit `ai_reply()` in `server.py` to use another provider. No API calls happen in demo mode, even if your terminal has a key configured.

Text and the last four conversation turns are sent to the AI provider only when AI mode is enabled. Conversations are kept in the page's memory, not written to disk. Browser speech recognition may separately send audio to the browser vendor's service, and may require internet access. Use typed input if you prefer to avoid that. No analytics, external scripts or font services are included.

This is a localhost teaching starter, not a production service. Phone calling is a possible next step, not a dependency: first make the conversation useful, then attach a phone or realtime voice transport if your project needs one.

## Small checks before showing your agent

- Unknown starting location: does it ask rather than guess?
- Missing community information: does it acknowledge the gap?
- No microphone, API key or network: is there a clear path back to typed demo mode?
- AI mode: does it stick to supplied facts? Model instructions are a starting point, not a guarantee.
- Stop, reset and switch examples: does old context stay out of the new conversation?

Run the included checks:

```sh
python3 -m unittest -v
```

## Files

| File | Purpose |
| --- | --- |
| `server.py` | Local server, scripted replies and optional AI adapter |
| `index.html` | Browser interface, speech input and speech output |
| `examples.json` | Sample itinerary and fictional community facts |
| `prompts.md` | Short instructions for optional AI mode |
| `test_server.py` | Grounding, validation and local HTTP checks |

## References

- [Anthropic Messages API](https://platform.claude.com/docs/en/api/messages/create)
- [Browser speech recognition and its limitations](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition)
- [Google Maps directions URLs](https://developers.google.com/maps/documentation/urls/guide)

MIT licensed. Contributions and alternative approaches are welcome.
