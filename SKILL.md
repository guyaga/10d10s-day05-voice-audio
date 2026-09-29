---
name: ai-voice-audio
description: Generate voice narration, dialogue, music, and sound effects using the ElevenLabs API. Text-to-speech with Eleven v4 (the newest and best model, recommended), v4 Turbo for real-time, audio tags for emotion and delivery ([whispers], [laughs], [excited]), multi-speaker dialogue, instant voice cloning, 85 languages including Hebrew, music generation and SFX. Use when the user wants voiceovers, narration, a dialogue or podcast, background music, sound effects, to clone a voice, or to add audio to any project. Works with ai-video-editor for adding narration to videos.
allowed-tools: Bash, Read, Write, Glob
---

# AI Voice & Audio (ElevenLabs, Eleven v4)

Generate professional voice narration, dialogue, music, and sound effects directly from Claude Code.

## Setup

1. **Get your API key** at [elevenlabs.io](https://elevenlabs.io) → Profile → API Keys
2. Set it:
   ```bash
   # Windows (PowerShell, persistent)
   setx ELEVEN_API_KEY "your-api-key-here"

   # macOS / Linux
   export ELEVEN_API_KEY=your-api-key-here
   ```

**Pricing:** Free tier gives ~10,000 characters/month. Starter plan ($5/mo) gives 30,000. Enough for learning.

## Models: use Eleven v4

| Model | ID | Best for | Max chars / request |
|-------|-----|----------|---------------------|
| **Eleven v4** ⭐ | `eleven_v4` | **Default. Use this.** Narration, audiobooks, characters, ads, anything where quality matters. 85 languages incl. Hebrew. | 10,000 |
| Eleven v4 Turbo | `eleven_v4_turbo` | Real-time: voice agents, interactive apps. Very high quality at low latency. | 10,000 |
| Eleven v3 | `eleven_v3` | Previous generation. Only if you prefer how a specific voice sounded on v3. | 5,000 |
| Flash v2.5 | `eleven_flash_v2_5` | Ultra-low latency, 32 languages, **no Hebrew**. | 40,000 |

**Rule: always default to `eleven_v4`. Use `eleven_v4_turbo` only when the user needs speed or real-time.**

### What changed from v3 (important)

- **Only two voice settings: `stability` and `similarity_boost`.** `style`, `speed` and `use_speaker_boost` are gone. The API accepts them but ignores them, so don't send them. SSML is not supported.
- **Delivery is controlled by audio tags and punctuation** (see below), not sliders.
- **Voice clones are much more accurate.** A clone on v4 can sound noticeably different from the same clone on v3, usually more like the real person. It also reproduces flaws in the recording (noise, room echo, harsh S sounds), so clean source audio matters more than ever.
- **Native accent across languages.** Same language as the clone's recording: accent preserved. Different language: v4 speaks it fluently, without carrying the original accent. (A Hebrew clone speaking English sounds like fluent English, not Hebrew-accented English.)
- **Voice Design voices** work on v4 but may perform less well than on earlier models.

## Text-to-Speech

### Step 1: Pick a voice

```bash
curl -s "https://api.elevenlabs.io/v1/voices" -H "xi-api-key: $ELEVEN_API_KEY"
```

Your own cloned voices appear here too (category `cloned`). Recommended premade voices:

| Voice | Style | ID |
|-------|-------|-----|
| Roger | Laid-back, casual | `CwhRBWXzGAHq8TQ4Fs17` |
| Sarah | Mature, confident | `EXAVITQu4vr4xnSDxMaL` |
| Charlie | Deep, energetic | `IKne3meq5aSn9XLyUdCD` |
| George | Warm storyteller | `JBFqnCBsd6RMkjVDRZzb` |
| Laura | Enthusiastic | `FGY2WhTYpPnrIDTdsKH5` |

### Step 2: Generate speech

```bash
curl -X POST "https://api.elevenlabs.io/v1/text-to-speech/JBFqnCBsd6RMkjVDRZzb?output_format=mp3_44100_128" \
  -H "xi-api-key: $ELEVEN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "[warm] Welcome back. [excited] Today we are building something REALLY cool... [laughs] let us go.",
    "model_id": "eleven_v4",
    "voice_settings": { "stability": 0.5, "similarity_boost": 0.8 }
  }' \
  --output ./output/narration.mp3
```

### Hebrew and other non-English text: use Node or Python, not curl

On Windows the shell corrupts Hebrew characters passed to curl. Always write a small `.mjs` (or Python) file, and pass `language_code`:

```javascript
import * as fs from "node:fs";
const VOICE_ID = "JBFqnCBsd6RMkjVDRZzb";
const r = await fetch(`https://api.elevenlabs.io/v1/text-to-speech/${VOICE_ID}?output_format=mp3_44100_128`, {
  method: "POST",
  headers: { "xi-api-key": process.env.ELEVEN_API_KEY, "Content-Type": "application/json; charset=utf-8" },
  body: JSON.stringify({
    text: "[warm] היי, ברוכים הבאים. [excited] היום אנחנו בונים משהו ממש מגניב! [laughs]",
    model_id: "eleven_v4",
    language_code: "he",
    voice_settings: { stability: 0.5, similarity_boost: 0.8 },
  }),
});
if (!r.ok) throw new Error(await r.text());
fs.writeFileSync("./output/narration-he.mp3", Buffer.from(await r.arrayBuffer()));
```

Language codes: `he` Hebrew, `en` English, `ar` Arabic, `es` Spanish, `fr` French, `de` German, `ru` Russian. Leave it out and the model detects the language itself; set it for Hebrew to be safe.

### Voice settings (v4)

| Setting | Range | Low | High |
|---------|-------|-----|------|
| **stability** | 0–1 | More expressive, varied delivery, follows tags more freely | Steadier, closer to a fixed baseline |
| **similarity_boost** | 0–1 | Looser, sometimes more natural | Sticks closer to the reference voice |

Starting points:
- **Narration / course / podcast:** stability 0.5, similarity 0.8
- **Ads, energetic reels, character acting:** stability 0.3, similarity 0.75
- **Calm, meditation, corporate:** stability 0.7, similarity 0.8
- **Your own cloned voice:** similarity 0.8–0.9

## Directing the performance: audio tags and punctuation

Put tags in square brackets **right before** the words they affect. They are performed, not read aloud (verified in Hebrew too).

| Kind | Examples |
|------|----------|
| Emotion | `[warm]` `[excited]` `[sad]` `[angry]` `[sarcastic]` `[curious]` `[thoughtful]` `[ecstatic]` `[annoyed]` |
| Delivery | `[whispers]` `[shouting]` `[quietly]` `[under his breath]` `[measured]` `[slowly]` `[quick, light, playful pace]` |
| Non-verbal | `[laughs]` `[soft chuckle]` `[giggle]` `[sighs]` `[gasps]` `[clears throat]` `[crying]` |
| Scene direction | `[Quiet, measured narration]` `[Pause, dry amusement]` `[Gradually building energy]` |
| Experimental | `[strong British accent]` `[sings]` and sound effects like `[applause]` |

- **Be specific.** `[low, gravelly voice]` beats a vague tag. Short scene directions work well on v4.
- **Tags work best when the voice can plausibly do it.** v4 can whisper with a voice that never whispered in its recording, but results vary. Test before a big job.
- **Punctuation shapes delivery:** `...` adds a pause and weight, CAPITALS add emphasis, short sentences speed things up, commas slow them down.
- **Pace:** there is no speed slider on v4. Use `[slowly]`, `[quick pace]`, ellipses and sentence length instead.

```text
[Quiet, measured narration] The training court lay beneath the keep, its stone walls silvered with frost.
[Pause, dry amusement] "You lost to a turnip cart yesterday," he said.
It was a VERY long day [sighs] ... nobody listens anymore.
```

## Dialogue (multiple speakers, one file)

One request, several voices, natural turn-taking. Works with `eleven_v4`:

```javascript
import * as fs from "node:fs";
const r = await fetch("https://api.elevenlabs.io/v1/text-to-dialogue?output_format=mp3_44100_128", {
  method: "POST",
  headers: { "xi-api-key": process.env.ELEVEN_API_KEY, "Content-Type": "application/json; charset=utf-8" },
  body: JSON.stringify({
    model_id: "eleven_v4",
    inputs: [
      { voice_id: "CwhRBWXzGAHq8TQ4Fs17", text: "[excited] Did you see the new voice model?" },
      { voice_id: "EXAVITQu4vr4xnSDxMaL", text: "[skeptical] Another one? [sighs] How good can it be." },
      { voice_id: "CwhRBWXzGAHq8TQ4Fs17", text: "[laughs] Just listen." },
    ],
  }),
});
fs.writeFileSync("./output/dialogue.mp3", Buffer.from(await r.arrayBuffer()));
```

Add `"language_code": "he"` for a Hebrew dialogue.

## Voice cloning

### Instant Voice Clone (seconds, from a short sample)

```bash
curl -X POST "https://api.elevenlabs.io/v1/voices/add" \
  -H "xi-api-key: $ELEVEN_API_KEY" \
  -F "name=My Voice" \
  -F "description=My cloned voice for narration" \
  -F "files=@voice-sample.mp3"
```

The response has a `voice_id`. Use it like any other voice, with `eleven_v4`.

### Recording tips for v4 (it copies everything)

1. **1–2 minutes** of clean speech is the sweet spot for an instant clone.
2. **One speaking style** throughout. Don't mix shouting, whispering and reading in the same sample; v4 captures a single style best.
3. **Quiet room, no echo, no music,** no fans or AC. v4 reproduces background noise and room sound.
4. **Same mic, same distance.** Volume jumps and harsh S sounds in the sample will show up in the clone.
5. **Speak the way you want the clone to sound.** Energetic sample, energetic clone.
6. **Already have a v3 clone?** Just switch `model_id` to `eleven_v4` and compare. It may sound different, usually closer to you. Pick the one you like.
7. **Professional Voice Clones** (trained on longer recordings) work on v4 too: in ElevenLabs → My Voices, hover the voice and click + next to Eleven v4.

## Music generation

```bash
curl -X POST "https://api.elevenlabs.io/v1/music" \
  -H "xi-api-key: $ELEVEN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Calm lo-fi hip hop beat with vinyl crackle, soft piano, and light drums",
    "music_length_ms": 30000
  }' \
  --output ./output/background-music.mp3
```

| Mood | Prompt |
|------|--------|
| **Corporate/Tech** | "Clean modern corporate music, light synths, inspiring, upbeat" |
| **Podcast intro** | "Short energetic podcast intro, electronic beats, 10 seconds" |
| **Calm/Focus** | "Ambient lo-fi music, soft piano, rain sounds, study vibes" |
| **Epic/Cinematic** | "Cinematic orchestral piece, dramatic build-up, strings and brass" |
| **Social media** | "Trendy upbeat electronic music, catchy, Instagram reel energy" |

## Sound effects

```bash
curl -X POST "https://api.elevenlabs.io/v1/sound-generation" \
  -H "xi-api-key: $ELEVEN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Notification chime, modern and clean, tech app style",
    "duration_seconds": 2.0,
    "prompt_influence": 0.3
  }' \
  --output ./output/notification.mp3
```

| Type | Prompt |
|------|--------|
| **UI/App** | "Soft click sound, modern mobile app interface" |
| **Transition** | "Swoosh transition sound, smooth and fast" |
| **Success** | "Achievement unlock sound, positive chime, game style" |
| **Ambient** | "Coffee shop background noise, gentle chatter and cups" |
| **Nature** | "Ocean waves gently crashing on a beach, relaxing" |

## Integration with the video editor (Day 3)

```bash
# 1. narration (English example; for Hebrew use the Node template above)
curl -X POST "https://api.elevenlabs.io/v1/text-to-speech/VOICE_ID" \
  -H "xi-api-key: $ELEVEN_API_KEY" -H "Content-Type: application/json" \
  -d '{"text": "[warm] Welcome to our product showcase...", "model_id": "eleven_v4"}' \
  --output narration.mp3

# 2. mix it into the video
ffmpeg -i video.mp4 -i narration.mp3 \
  -filter_complex "[1:a]volume=1.0[narr];[0:a][narr]amix=inputs=2:duration=first" \
  -c:v copy output.mp4
```

Or just tell Claude Code: *"Generate a warm voiceover saying 'Welcome to our product...' with the George voice, then add it to D:/Videos/product-demo.mp4 as narration."*

## Best practices

1. **Use `eleven_v4`** unless you need real-time (`eleven_v4_turbo`).
2. **Send only `stability` and `similarity_boost`.** Direct everything else with tags and punctuation.
3. **Hebrew:** Node/Python script, `language_code: "he"`, never curl on Windows.
4. **Test one short sentence first,** especially with new tags or a new clone.
5. **Long scripts:** up to 10,000 characters per request; split longer text at paragraph breaks.
6. **Verify tags weren't read aloud** on important jobs: run the file through speech-to-text (`POST /v1/speech-to-text`, `model_id: scribe_v1`) and read the transcript.
7. **Save your clone's `voice_id`** and reuse it across projects.
8. **Output to `./output/`** and check your credits before big generations.
9. **The model keeps improving.** Re-test your voices every few weeks; results can shift.

## Common use cases

1. **Video narration** for tutorials, product demos, courses
2. **Podcasts and dialogue** with two or more voices in one file
3. **Audiobooks and character voices** directed with scene tags
4. **Social media** reels, stories, TikToks
5. **Background music and sound design**
6. **Your own voice** reading anything, in 85 languages, fluently
7. **Accessibility:** audio versions of written content
