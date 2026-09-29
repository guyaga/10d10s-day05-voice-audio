<p align="center">
  <img src="cover.jpg" alt="AI Voice & Audio — ElevenLabs" width="100%">
</p>

<h1 align="center">Day 5 — AI Voice & Audio</h1>
<p align="center">
  <strong>10 Days 10 Skills</strong> · Claude Code Course by <a href="https://bestguy.ai">Guy Aga</a>
</p>
<p align="center">
  <img src="https://img.shields.io/badge/Service-ElevenLabs-E63B2E?style=flat-square" alt="ElevenLabs">
  <img src="https://img.shields.io/badge/Skill-ai--voice--audio-111111?style=flat-square" alt="Skill">
  <img src="https://img.shields.io/badge/Level-Beginner-E8E4DD?style=flat-square&labelColor=111111" alt="Beginner">
</p>

---

## What is This?

This skill lets you **generate professional voiceovers, music, and sound effects** directly from Claude Code using ElevenLabs. Type what you want to say, pick a voice (or clone your own), and get studio-quality audio in seconds.

### What Can You Do With It?

| You Say | What Happens |
|---------|-------------|
| "Narrate this script with a warm male voice" | Professional voiceover in MP3 |
| "Read this in Hebrew, excited at the start and whispering at the end" | One narration, emotion directed line by line |
| "Make a two-person podcast dialogue about X" | Several voices in one audio file |
| "Clone my voice from this recording" | Your voice, reading anything you type |
| "Create background music for a tech video" | Custom AI-generated track |
| "Generate a notification sound" | UI sound effect ready to use |
| "Add voiceover to my video" | Narration generated + mixed into video (with Day 3 skill) |

### Why ElevenLabs?

| Feature | What It Means |
|---------|---------------|
| **Eleven v4** | The newest model: most natural voice yet, much more accurate voice clones, 85 languages |
| **Voice Cloning** | Record 1 minute of your voice → AI speaks anything in YOUR voice |
| **85 Languages** | Same voice, different language, spoken fluently: Hebrew, English, Spanish, anything |
| **Music + SFX** | Not just voices — generate background music and sound effects too |
| **Emotion control** | Audio tags like `[whispers]`, `[laughs]`, `[excited]` direct the performance line by line |

---

## New: Eleven v4 (September 2026)

The skill now uses **Eleven v4**, ElevenLabs' newest model. **You don't need to pick a model or tune any settings:** the skill already chooses the best model with settings tuned for great results. Just ask for what you want, and if you feel like playing, everything stays open. If you installed the skill before, update it (see [Already installed? Update to v4](#already-installed-update-to-v4)).

| What changed | What it means for you |
|---|---|
| **More natural voices** | Better quality, emotion and delivery than any previous model |
| **Much more accurate clones** | Your cloned voice sounds more like you. It may sound different from how it sounded on v3 |
| **Audio tags replace sliders** | Direct the performance with `[warm]`, `[whispers]`, `[laughs]`, `[slowly]` instead of style/speed settings |
| **Fluent in every language** | A Hebrew clone speaking English sounds like fluent English, without carrying the Hebrew accent |
| **Longer text** | Up to 10,000 characters per request (v3 allowed 5,000) |
| **v4 Turbo** | A fast variant for real-time apps and voice agents |

---

## Prerequisites

- [ ] **Claude Code** (Pro or Max subscription)
- [ ] **ElevenLabs account** (free tier available)
- [ ] **ElevenLabs API Key**

> **This skill requires a separate API key** from ElevenLabs (not the same Gemini key from Days 1-2). Free tier gives ~10,000 characters/month — enough for this lesson.

---

## Step 1: Create an ElevenLabs Account

1. Go to [elevenlabs.io](https://elevenlabs.io)
2. Click **Sign Up** (Google or email)
3. Free tier is enough to start

---

## Step 2: Get Your API Key

1. After signing in, click your **profile icon** (bottom-left)
2. Click **Profile + API key**
3. Click **Create API Key**
4. Copy the key

---

## Step 3: Set Your API Key

```bash
# Windows (PowerShell) - saved permanently, open a new terminal afterwards
setx ELEVEN_API_KEY "your-api-key-here"

# macOS / Linux
export ELEVEN_API_KEY=your-api-key-here
```

---

## Step 4: Install the Skill

### The Easy Way (Recommended)

Open Claude Code and paste:

```
Install the ai-voice-audio skill globally: clone https://github.com/guyaga/10d10s-day05-voice-audio into ~/.claude/skills/ai-voice-audio (my home folder, not this project), then help me set up my ElevenLabs API key and test it with one short sentence.
```

### Manual Way

```bash
git clone https://github.com/guyaga/10d10s-day05-voice-audio ~/.claude/skills/ai-voice-audio
```

The skill lives in your home folder, so it works in every project.

### Already installed? Update to v4

Paste this into Claude Code:

```
Update my ai-voice-audio skill to the latest version from https://github.com/guyaga/10d10s-day05-voice-audio (git pull in ~/.claude/skills/ai-voice-audio, or re-clone it there if it is not a git folder). Then generate one short test sentence so I can hear the difference.
```

Or manually: `git -C ~/.claude/skills/ai-voice-audio pull`

---

## How to Use It

### Generate a Voiceover

```
Generate a voiceover saying "Welcome to our AI-powered marketing platform. 
In this video, we'll show you how to 10x your content production." 
Use a warm, professional male voice. Save to D:/Audio/narration.mp3
```

### Direct the Emotion with Audio Tags

```
Narrate this with the George voice. Start warm, get excited in the middle, and whisper the last line:
"[warm] Welcome back. [excited] Today we are building something REALLY cool! [whispers] and nobody knows about it yet."
```

Tags go in square brackets right before the words they affect. They are performed, not read aloud. Useful ones: `[warm]` `[excited]` `[sad]` `[sarcastic]` `[whispers]` `[shouting]` `[laughs]` `[sighs]` `[slowly]`. Ellipses (...) add pauses, CAPITALS add emphasis.

### Narrate in Hebrew

```
Read this in Hebrew with my cloned voice, calm and friendly, and save it to D:/Audio/intro-he.mp3:
"היי, ברוכים הבאים לשיעור. היום נלמד משהו חדש."
```

### Create a Dialogue

```
Make a short podcast dialogue between Roger and Sarah about why AI voices got so good.
Roger is excited, Sarah is skeptical at first. Save to D:/Audio/dialogue.mp3
```

### Clone Your Voice

```
Clone my voice from this recording: D:/Audio/my-voice-sample.mp3
Then use the cloned voice to narrate: "This is my brand voice, generated by AI."
```

> **Tip:** Record 1-2 minutes of clear speech in a quiet room, in one speaking style. v4 copies everything in the recording, including noise and echo.

### Generate Background Music

```
Generate 30 seconds of calm lo-fi hip hop music for a YouTube video intro.
Save to D:/Audio/background.mp3
```

### Create Sound Effects

```
Generate a modern app notification chime sound, 2 seconds long.
Save to D:/Audio/notification.mp3
```

### Add Narration to Video (Day 3 + Day 5)

```
Generate a voiceover for this script, then add it to D:/Videos/product-demo.mp4:
"Introducing the future of content creation. One tool. Infinite possibilities."
```

---

## Voice Cloning Tips

For the best clone quality:

Eleven v4 reproduces the source recording very faithfully, flaws included. So:

1. **1-2 minutes** of clear speech is the sweet spot for an instant clone
2. **One speaking style** — don't mix shouting, whispering and reading in the same sample
3. **Quiet room, no echo** — turn off AC, fans, TV. Background noise ends up in the clone
4. **Same mic, same distance** — volume jumps show up in the clone too
5. **Speak the way you want the clone to sound** — energetic sample, energetic clone
6. **Use similarity_boost 0.8-0.9** for cloned voices
7. **Cloned your voice before?** It works on v4 as is, no re-recording needed

---

## Pricing Quick Guide

| Plan | Price | Characters/Month | Voice Clones |
|------|-------|-------------------|-------------|
| Free | $0 | ~10,000 | 1 instant clone |
| Starter | $5/mo | 30,000 | 3 instant clones |
| Creator | $22/mo | 100,000 | 10 clones |

> **10,000 characters ≈ 5-7 minutes of speech.** Enough for several narrations.

---

## Works Great With Other Skills

| Skill | How They Work Together |
|-------|----------------------|
| **Day 3: Video Editor** | Generate narration → add to video with ffmpeg |
| **Day 2: Video Analyzer** | Transcribe existing video → regenerate narration in a different voice |
| **Day 1: Image Generation** | Generate audio + visuals for a complete content package |

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "API key not found" | Set `ELEVEN_API_KEY` in your terminal |
| "Quota exceeded" | Check credits at elevenlabs.io. Free tier resets monthly |
| Tag was read out loud | Put the tag in square brackets right before the words, e.g. `[laughs]`, and keep it short |
| My clone sounds different than before | That's v4's higher accuracy: it copies your recording more faithfully. Re-record a cleaner 1-2 minute sample if needed |
| Hebrew comes out garbled | Update the skill to the latest version (the prompt above). It already handles Hebrew correctly |
| Voice sounds robotic | Ask Claude for a more lively delivery, or add audio tags like `[warm]` or `[excited]` before the words |
| Clone doesn't sound like me | Record 1-2 minutes in one speaking style, quiet room, no echo. v4 copies the recording's flaws too |
| Audio too fast/slow | v4 has no speed slider: use tags like `[slowly]` or `[quick pace]`, ellipses and shorter sentences |

---

## Links

- [ElevenLabs — Sign Up](https://elevenlabs.io)
- [ElevenLabs API Docs](https://elevenlabs.io/docs/api-reference)
- [Eleven v4 model page](https://elevenlabs.io/docs/overview/models)
- [Prompting Eleven v4 (audio tags)](https://elevenlabs.io/docs/overview/capabilities/text-to-speech/best-practices#prompting-eleven-v4)
- [ElevenLabs Pricing](https://elevenlabs.io/pricing)
- [Course Page — bestguy.ai](https://bestguy.ai/course/10-days-10-skills)
- [HTML Skill Guide (Hebrew)](https://bestguy.ai/course/guides/day05-voice-audio.html)

---

<p align="center">
  <strong>10 Days 10 Skills</strong> — Claude Code Course<br>
  <a href="https://bestguy.ai">bestguy.ai</a> · Guy Aga © 2026
</p>
