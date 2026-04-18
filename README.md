CipherStack 🔐
> Node-based cascade encryption builder — visually chain cipher algorithms and watch your data transform at every step.
🔗 Live Demo → darkgreen-fox-338032.hostingersite.com
---
![CipherStack Preview](preview.png)
---
What Is CipherStack?
CipherStack lets you build a visual encryption pipeline by chaining multiple cipher algorithms together as nodes. Plaintext enters the first node, each node's output feeds into the next, and the final node produces the encrypted result. Switch to Decrypt mode and the pipeline runs in reverse — recovering the exact original plaintext.
Built for the VYRO Hackathon — Frontend Track, Hard difficulty.
---
Features
Visual node pipeline — add, reorder, and remove cipher nodes on a canvas
9 cipher algorithms — 6 configurable + 3 bonus
Intermediate outputs — every node shows exactly what it received and produced
Encrypt & Decrypt modes — one toggle reverses the entire pipeline automatically
Round-trip verification — one-click PASS/FAIL correctness test
Live preview — pipeline re-runs as you type (debounced 300ms)
Drag to reorder — grab any node and drag it to a new position
Undo history — Ctrl+Z steps back through node changes
Export / Import — save your pipeline as JSON and reload it anytime
Shareable URL — pipeline config encoded in the URL hash, share with one link
AI Assistant — powered by Gemini 2.5 Flash, asks for demo inputs and explains ciphers
Particle background — animated canvas network on the workspace
Keyboard shortcuts — Ctrl+Enter to run, Ctrl+Z to undo, Escape to close AI
---
Cipher Reference
Configurable (counts toward the 3-node minimum)
Cipher	Config	How It Works
Caesar Shift	Shift: -25 to 25	Shifts each letter by N positions in the alphabet. Non-alpha chars unchanged.
XOR Cipher	Key: any string	XORs each UTF-8 byte with repeating key bytes. Outputs hex. Its own inverse.
Vigenère	Keyword: alpha string	Polyalphabetic substitution — each letter shifted by the corresponding keyword char.
Rail Fence	Rails: 2–10	Zigzag transposition across N rails. Pure rearrangement, no substitution.
Columnar	Key: alpha string	Writes text into rows, reads columns sorted alphabetically by key.
AES	Key + Mode (CBC/ECB)	Symmetric encryption. Output as Base64.
Bonus (no config required)
Cipher	Description
Base64	Encode to Base64 / decode back. Unicode-safe.
ROT13	Rotate letters by 13. Its own inverse.
Reverse	Reverses the string. Emoji and Unicode safe.
---
Correctness Contract
Every cipher satisfies:
```
cipher.decrypt(cipher.encrypt(input, config), config) === input
```
for any string input and any valid configuration.
---
Quick Demo Tests
3-Node Test
#	Cipher	Config
1	Vigenère	keyword = `STORM`
2	Rail Fence	rails = `5`
3	XOR	key = `x9!mQ`
Input: `Attack at dawn, the enemy won't expect it!`
Encrypt → copy output → switch to Decrypt → paste → Run → should recover input exactly.
---
6-Node Nuclear Test
#	Cipher	Config
1	Rail Fence	rails = `6`
2	Vigenère	keyword = `CIPHERSTACK`
3	Columnar	key = `QUANTUM`
4	Caesar	shift = `19`
5	XOR	key = `n3cl34r!@#`
6	AES	key = `vyro2026hackathon!`, mode = `CBC`
Input: `CipherStack was built by a genius and no one can break this encryption pipeline ever!`
Hit Round-Trip Test → should show `✓ PASS`
---
How to Run
This is a single HTML file — no build step, no dependencies, no install.
```bash
# Option 1 — open directly in browser
open cipherstack.html

# Option 2 — local server (required for AI assistant CORS)
python3 -m http.server 8080
# then visit: http://localhost:8080/cipherstack.html
```
---
How to Deploy
Hostinger / cPanel:
Upload `cipherstack.html` to `public\_html` via File Manager.
Netlify (drag & drop):
Go to netlify.com/drop and drag the HTML file.
Vercel:
```bash
npm i -g vercel \&\& vercel
```
GitHub Pages:
```bash
git add .
git commit -m "deploy"
git push
# then enable Pages in repo Settings → Pages → main branch → / (root)
```
---
Tech Stack
Layer	Choice
UI Framework	Vanilla JS + HTML5 + CSS3
Fonts	Inter (UI) + JetBrains Mono (cipher IO) via Google Fonts
Animations	CSS keyframes + Canvas API (particle network)
Drag & Drop	HTML5 native drag events
AI Assistant	Gemini 2.5 Flash API
Cipher Library	Pure JavaScript — zero external crypto dependencies
State	Plain JS object + history stack (undo)
Persistence	URL hash (shareable) + JSON export/import
No React. No build tools. No node_modules. One file.
---
Project Structure
```
cipherstack.html     ← entire application (single file)
preview.png          ← screenshot for README
pipeline.json        ← example exported pipeline (import to demo instantly)
README.md
```
---
Architecture
All cipher logic follows a single contract:
```js
{
  id: 'caesar',
  name: 'Caesar Shift',
  type: 'configurable',
  configSchema: \[{ key: 'shift', label: 'Shift amount', type: 'number', min: -25, max: 25 }],
  defaultConfig: { shift: 3 },
  encrypt(input, config) { ... },
  decrypt(input, config) { ... }
}
```
The pipeline engine is a pure function — it walks the node array forward (encrypt) or backward (decrypt) and knows nothing about individual ciphers. Adding a new cipher requires only adding one object to the registry.
---
License
MIT — free to use, modify, and distribute.
---
Built with 🔐 for VYRO Hackathon 2026
