# Hi, I'm Alejandro 👋

Software developer from Gran Canaria, British and Spanish. I build apps and AI projects, and I'm looking for my first software engineering role or degree apprenticeship in the UK.

- 🎓 Level 5 diploma in Multiplatform Application Development (2026), now studying an **AI and Big Data** specialisation (current average 9.9/10)
- 🇫🇮 Erasmus+ placement at CasaMedia, Finland: Flutter apps, a business management system and an AI chatbot using Google and OpenAI APIs
- 📚 Learning C with Harvard's **CS50x**
- 🏀 Former youth basketball head coach · 🏎️ sim racing and motorsport fan

[LinkedIn](https://www.linkedin.com/in/alejandro-wade-montesdeoca-ba51a1299/) · [Email](mailto:alejandrowade2017@gmail.com)

---

## ☕ SpotHop: find cafés, pubs, bars and restaurants on a live map

**[Try it live](https://aleexwadee.github.io/Cafe-Finder/)** · **[Read the code](https://github.com/AleexWadee/Cafe-Finder)**

A web app in plain JavaScript (ES modules, no framework) that shows places near you on a live map, using free OpenStreetMap data.

- Started as one 1,400-line script; I split it into **16 modules** by responsibility ([`js/core`](https://github.com/AleexWadee/Cafe-Finder/tree/main/js/core), [`js/data`](https://github.com/AleexWadee/Cafe-Finder/tree/main/js/data), [`js/ui`](https://github.com/AleexWadee/Cafe-Finder/tree/main/js/ui))
- **Keeps working when a service is down**: asks several servers at once, uses the first good answer, and falls back to a second data source
- **Caching and offline use** with a service worker, smart search with typo tolerance, directions, and 6 languages

The part I'm proudest of, slightly shortened from [`js/data/api.js`](https://github.com/AleexWadee/Cafe-Finder/blob/main/js/data/api.js):

```js
// Asks every Overpass server at the same time and uses the first good answer.
async function fetchOverpass(query) {
  const servers = CONFIG.OVERPASS_SERVERS;
  const controllers = servers.map(() => new AbortController());
  const attempts = servers.map(async (server, i) => {
    const timer = setTimeout(() => controllers[i].abort(), OVERPASS_TIMEOUT);
    try {
      const res = await fetch(server, { method: "POST", body: new URLSearchParams({ data: query }), signal: controllers[i].signal });
      if (!res.ok) throw new Error(`${server} responded ${res.status}`);
      return (await res.json()).elements || [];
    } finally {
      clearTimeout(timer);
    }
  });
  try {
    return await Promise.any(attempts);   // first server to answer wins
  } finally {
    controllers.forEach((c) => c.abort()); // stop the slower ones
  }
}
```

## 🎙️ Wadia: an AI voice assistant that runs on my laptop

**[Read the code](https://github.com/AleexWadee/Wadia)**

Python voice assistant with speech recognition, a local language model (Qwen via Ollama) and speech synthesis, all running on my own laptop with no cloud services. It replies about two seconds after you stop talking, remembers past conversations, and has a guest mode that hides private data.

I first wanted to train my own model, but my laptop didn't have enough memory, so I switched to running local models and giving them web and file access instead.

Highlights: it speaks the answer sentence by sentence while the model is still writing it ([`brain.py`](https://github.com/AleexWadee/Wadia/blob/main/local/brain.py)), the model decides by itself when to use tools like web search, weather or a safe calculator ([`tools.py`](https://github.com/AleexWadee/Wadia/blob/main/local/tools.py)), and a phone app and guest mode run on the same `aiohttp` backend ([`server.py`](https://github.com/AleexWadee/Wadia/blob/main/local/server.py)).

---

## 🛠️ Tools I use

- **Languages:** Python · Java · JavaScript · SQL · Dart
- **Frameworks and tools:** Flutter · Git and GitHub · Ollama (local AI models) · VS Code · Android Studio
- **Learning:** C (CS50x)
