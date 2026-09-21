this is my build log

09/09/26 I started this project.
## 09/12/26 — Homework 1: TouchDesigner + Claude MCP + webcam effect

**Result:** webcam → Edge TOP → Out TOP, built by Claude over the MCP connection.
Screenshot: `touchdesigner-screenshot.png` (the TD window).
Project file: `IXD415.toe`.

### What broke, what I asked Claude, what happened

1. **Claude couldn't click inside TouchDesigner.** macOS "Screen Recording" permission wasn't granted to the Claude app, so it couldn't see or drive the TD window. I asked it to just tell me what to click. Claude switched to sending Python commands to TD over the MCP web connection and gave me step-by-step click instructions for the parts only a human could do.

2. **The Claude app crashed mid-session — twice.** I lost the whole conversation. When I reopened, I asked "do you remember any of this?" It didn't, but it rebuilt context from the repo and a memory file it had written. We then set up a git-ignored `memory/` folder (an Obsidian vault with linked notes) so future sessions can pick up faster.

3. **The MCP connector has to be loaded by hand.** The connector is a `.tox` file that lives *outside* the project. Claude asked me to drag it onto the network canvas. Also, the port in Claude's notes (9981) was wrong — TD was actually listening on 9931 the first time. Claude found the real port with `lsof` and fixed its notes.

4. **Webcam stuck on "Waiting for authorization."** I had allowed the camera in System Settings, but TD had been running *before* I allowed it, so it never noticed. Claude had me quit and reopen TD. After the restart the camera worked (1280×720).

5. **After reopening, the connector answered but every request was "404 not found."** Claude found the cause: TD had saved the connector's file location as a relative path (`../../../../Documents/...`), which broke on reload, so its Python modules never imported. Fix: re-drag the `.tox` in, then Claude set the path to an absolute one and re-saved the project.

6. **Claude couldn't take the screenshot either** (same Screen Recording permission), so I took it with Cmd+Shift+3 and Claude copied it into the repo.

**Takeaway:** most of the friction was macOS permissions and things that only happen on a *restart*. Everything worked the first time and broke the second time.

## 09/16/26 — Hand-tracked ice magic (built with Claude over the MCP)

**What it does now** (`IXD415.toe`, screenshot: `are-you-freezing.png`):
- MediaPipe hand tracking (free TouchDesigner plugin) feeds a small Python "brain" (Script CHOP).
- Open palm → stream of snowflake particles from the palm, flying at the camera. Snowflake sound plays.
- Fist → a ring fills around the fist in 8 steps (green → red). Open at a higher step → more, faster, bigger snow.
- Both palms open → rainbow. A pill button "Are You Freezing?" appears; touching it plays a finale clip once.
- Rotate an open hand like a dial → synthesized kick / snare / hats / sub-bass fade in, locked to the clip's beat (137 BPM, the song's tempo).

**What broke, what I asked, what happened**
- TouchDesigner got paused (spacebar) and nothing moved for 10 minutes while Claude debugged the particle system. Claude eventually noticed the frame counter wasn't advancing and un-paused it.
- The particle engine's default camera had been orbited off into space; nothing rendered until Claude reset it.
- Particles died instantly because lifetime went negative (life 1.5 ± variance 3).
- Every "where is the particle emitter?" bug was a coordinate convention: the tracker's Y is bottom-up, the video is mirrored but the landmarks aren't, and the camera's field of view is horizontal, not vertical.
- Fire mode got cut: it kept turning into snow when the hand left, and the tracker's gesture labels were unreliable at odd angles. Replaced gesture labels with "how far are the fingertips from the wrist," which works at any rotation.
- The tracker hallucinated a hand on my face at 20% confidence → raised thresholds to 60–70%.
- The saved project ballooned to 181 MB (plugin embedded). Switched to referencing the plugin file → 1 MB.
- TouchDesigner's "increment filename on save" kept popping a dialog and freezing the connection. Turned off.
- In Composite TOP "over", the FIRST input is on top. Cost us a white pill with invisible text.

## 09/21/26

This week was way more thinking than actually building. I came up with ten concepts and found three reference works, so most of the week was basically figuring out what the project even was before making anything*.*

The first thing I did was research live data feeds: 
Ships, lightning, earthquakes, solar wind, Wikipedia edits, flights, tides. Literally everything. I ended up making this huge table with 15 different options using Chat GPT.

And then Claude basically pointed out that all 15 of them were the exact same idea.

Live data changes a visual and that was it. No real message or "nathan" behind it. 

And even worse, almost every single one already had some famous artist who had done a way better version of it. So I kind of had this moment of like, okay, I didn’t actually come up with 15 ideas. I researched 15 other people’s projects.

So I  flipped the approach.

Instead of the data being the main thing, the body became the actor and the data became more like the weather around you (literally in some concepts lol).

Claude threw 10 really rough ideas at me and I immediately killed probably half of them. One was literally “the piece refuses input from 8–9 p.m.” which I was like, absolutely not. That’s lazy.

The best ideas came from  arguing with Claude about what I felt would be cool, actually impactful, AND what I knew we had some sort of data for.

Things like turning your palms down and making it “rain” to cool the Amazon, holding your actual iPhone Screen Time page up to the camera so the piece can use your number, or doing a TikTok-style swipe where every new “post” is just another, more distorted version of you.

Those started feeling way more interesting because the person isn’t just standing there watching data move anymore. They’re actually doing something.

The thing that broke this week was my Govee light bars. They would not connect to WiFi, and it took me way too long to figure out why.

Turns out I had to disable 5 GHz, go into my iPhone WiFi settings, connect directly to the temporary network coming from the lights, and then finish setup from there. Once they were finally connected, LAN Control showed up, so that was a huge win! I also requested an API key just as a backup in case I need another way to control them later and received the key! 

Biggest thing I learned this week: research the artists before you fall in love with an idea because I almost spent an entire week making  worse versions of things that already existed.
