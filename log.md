this is my build log

09/09/26 I started this project.
## 09/12/26 — Homework 1: TouchDesigner + Claude MCP + webcam effect

**Result:** webcam → Edge TOP → Out TOP, built by Claude over the MCP connection.
Screenshots: `touchdesigner-screenshot.png` (the TD window), `webcam-edge-effect.png` (the raw output frame).
Project file: `IXD415.toe`.

### What broke, what I asked Claude, what happened

1. **Claude couldn't click inside TouchDesigner.** macOS "Screen Recording" permission wasn't granted to the Claude app, so it couldn't see or drive the TD window. I asked it to just tell me what to click. Claude switched to sending Python commands to TD over the MCP web connection and gave me step-by-step click instructions for the parts only a human could do.

2. **The Claude app crashed mid-session — twice.** I lost the whole conversation. When I reopened, I asked "do you remember any of this?" It didn't, but it rebuilt context from the repo and a memory file it had written. We then set up a git-ignored `memory/` folder (an Obsidian vault with linked notes) so future sessions can pick up faster.

3. **The MCP connector has to be loaded by hand.** The connector is a `.tox` file that lives *outside* the project. Claude asked me to drag it onto the network canvas. Also, the port in Claude's notes (9981) was wrong — TD was actually listening on 9931 the first time. Claude found the real port with `lsof` and fixed its notes.

4. **Webcam stuck on "Waiting for authorization."** I had allowed the camera in System Settings, but TD had been running *before* I allowed it, so it never noticed. Claude had me quit and reopen TD. After the restart the camera worked (1280×720).

5. **After reopening, the connector answered but every request was "404 not found."** Claude found the cause: TD had saved the connector's file location as a relative path (`../../../../Documents/...`), which broke on reload, so its Python modules never imported. Fix: re-drag the `.tox` in, then Claude set the path to an absolute one and re-saved the project.

6. **Claude couldn't take the screenshot either** (same Screen Recording permission), so I took it with Cmd+Shift+3 and Claude copied it into the repo.

**Takeaway:** most of the friction was macOS permissions and things that only happen on a *restart*. Everything worked the first time and broke the second time.
