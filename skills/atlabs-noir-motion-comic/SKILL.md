---
name: atlabs-noir-motion-comic
description: Create a script-to-video story with the Atlabs MCP and edit it into a noir motion comic with comic panels, speech balloons, typewriter narration, film grain, a three-part score, layered sound effects, sound words and a neon end card.
---

# Noir motion comic with Atlabs

This skill turns an idea into a finished 1:30 to 2:00 noir motion comic. Atlabs generates the story (script, illustrated scenes, character voices). The edit then gives it the comic treatment:

- cream panel borders, halftone and film grain;
- speech balloons with the spoken word lit red;
- typewriter narration boxes;
- a slow camera move on every panel;
- comic sound words (CLINK!, CLICK, BANG!);
- a score that cuts dead on the climax;
- layered sound design;
- a neon end card.

Work through the steps in order. Use the tools yourself and only stop to ask when a step says to.

## 0. Set up

**Atlabs MCP connector (required).** It writes and generates the story and makes the music and images.

- claude.ai: Settings > Connectors > Add custom connector. Paste the Atlabs MCP server URL from your Atlabs account, save, then connect and sign in to Atlabs.
- Claude Code: `claude mcp add --transport http atlabs <ATLABS_MCP_URL>`, then run `/mcp` and sign in.
- Check it with the `get_account` tool. It should show your plan and credit balance.

**Optional helpers:**

- A sound-effects generator (for example the ElevenLabs MCP connector) for the SFX in step 6. Otherwise use a licensed SFX library.
- ffmpeg, for trimming, normalizing and encoding.
- Any timeline editor that supports clip motion, overlays, transitions and several audio tracks.

If something is missing, say what in one line and continue with what you can do.

## 1. Write and generate the story

Write a tight noir script of about 30 to 35 scenes:

- A narrator in the past tense, wry and seductive. The first line sets a theme that the last line turns on its head.
- Two leads, plus a third character who pays off the twist.
- Dialogue lines of 1 to 2 seconds each. Short lines hit harder in balloons.
- 2 or 3 silent beats: a look, a kiss, the climax.
- Three acts:
  1. the meeting in a club or bar;
  2. the move to somewhere private;
  3. the reveal, the shot and the aftermath.
- Keep it suspenseful and sensual, never explicit.

Generate it with the Atlabs story tools: `plan_story` then `create_story`, or the `create_blank_story` flow. Follow each tool's own description.

- Style: a duotone noir illustration look (flat vector pop art, crimson skin tones, deep navy skies, hard black shadows, thin linework). Run `list_image_styles` to find it.
- Give the narrator and each character a distinct voice (`set_narrator_voice`, `set_character_voice`).
- Wait for the story to finish (`wait_for_story`), then collect every scene's image and voiceover with `get_story_assets`. Read the caption timings too, with `get_scene_captions`.
- Check each caption against the script text. Speech-to-text often mishears very short lines, so fix the wording and keep the timings.

## 2. Study every frame

Make contact sheets of the scene images (8 per sheet, numbered) and look at them. For each scene, note:

- who speaks, and where their mouth is (x and y from 0 to 1);
- empty areas, usually the sky or a wall, where a balloon or caption can sit without covering a face;
- location changes and props: glasses, doors, a lighter, a gun.

## 3. Edit: look and pacing

**Global look:**

- Comic panel border on, halftone about 50%.
- Film grain about 45%.
- Fade in from black over 0.4 s.

**Pacing.** Each scene lasts lead + voiceover + tail, and the voiceover starts after the lead.

| Scene type | Lead | Tail |
|---|---|---|
| Dialogue | 0.1 to 0.2 s | 0.3 to 0.5 s |
| Narration | 0.2 to 0.4 s | 0.5 to 0.9 s |
| First scene | about 1.6 s, so the room sound lands first | |
| Silent beat | 2.6 to 3.0 s in total | |
| Line interrupted by the shot | | 0 |
| Last line | | about 1.6 s |

**Camera move on every panel:**

- Slow push-in, 8 to 12% over the shot, as the default.
- Gentle pans (about 6%) on wide two-shots, and a pull-back (about 10%) on establishing shots.
- A stronger push (15 to 18%) on reveals, and a short shake on the gunshot frame.
- Light flicker (30 to 50%) on neon or club openers and on the reveal.

**Text:**

- Dialogue goes in white speech balloons placed in empty space near the top (y 0.10 to 0.15). The tail points at the speaker's mouth.
  - Keep balloons between x 0.18 and 0.78 so they never run off the frame.
  - Highlight the word being spoken in red as it is said.
- Narration goes in a cream typewriter caption box that types on, top-left or top-right. Use the bottom-left when the top is busy.

**Transitions** (centred on the cut, no overlap):

| Moment | Transition |
|---|---|
| Location change | Wipe left, 0.45 s |
| Arriving somewhere new, seduction, a narration about the lie | Dissolve, 0.6 to 0.8 s |
| After the kiss | Dip to black, 0.7 s |
| Gunshot | White flash, 0.25 s |
| Into the end card | Dip to black, 0.6 s |
| Everything else | Hard cut |

**Sound words.** Use bold comic lettering with a pop-in, on screen for 0.9 to 1.3 s, tilted 6 to 8 degrees, on an empty patch of the frame and never over a balloon:

- CLINK! on the glasses
- CLICK on the door lock
- CLICK on the gun hammer
- a large BANG! on the shot
- FLICK on the lighter

## 4. Score (Atlabs `generate_music`)

Generate three instrumental cues. Write "instrumental, no vocals" in each prompt.

1. **Seduction:** slow smoky noir jazz with brushed drums, upright bass, muted trumpet and sax, about 80 s.
2. **Tension:** dark pulsing strings and low synth that build, with a clear jump in intensity partway through.
3. **Coda:** a lonely, melancholy trumpet, about 60 s.

Lay them out like this:

- The seduction cue plays from the start to the beginning of act three, fading out over about 3 s.
- The tension cue fades in over about 2 s just before act three. Trim it so its big jump lands exactly on the gun reveal. Find the jump with a fine loudness scan.
- The tension cue cuts dead on the gunshot frame, with a 30 ms fade only.
- About 1.5 s of silence follows, so the gunshot rings out alone.
- The coda then plays through the aftermath and the end card, fading over the last 1.5 s.
- Music sits 15 to 18 dB under dialogue, with 6 dB of ducking under speech.

## 5. Sound design

About 20 effects. Normalize them before mixing, because raw SFX levels vary a lot: beds to about −24 dB RMS, one-shots to about −14 dB RMS, peaks limited at −1 dB.

**Beds** (30 s loops, ducked under speech, crossfaded where one copy ends):

- club room tone;
- hotel corridor hush;
- hotel room night hush (bring it back after the shot).

**One-shots, in story order:**

- ice in a glass on the first shot;
- glass clink;
- heels on marble as they leave;
- a sting on the "hunter becomes the hunted" line;
- silk glove;
- door lock click;
- zipper and sheets in the bedroom;
- a heartbeat under the lies, stopping on the reveal;
- door creak on the last words of the narration about the door;
- a sting and the hammer cock on the reveal;
- gunshot with a ringing tail, then the body slumping;
- zippo flick and a slow exhale of smoke in the aftermath;
- neon buzz and a low sax note on the end card.

**Starting gains relative to the normalized files** (adjust by ear):

| Effect | Gain |
|---|---|
| Gunshot | 0 dB, the loudest moment |
| Hammer, exhale, door lock | −6 dB |
| Glass, zippo, door creak | −7 to −8 dB |
| Heels, neon | −9 dB |
| Stings, heartbeat, zipper | −10 to −13 dB |
| Ice | −13 dB |
| Club bed | −2 dB |
| Room beds | −10 to −13 dB |
| Sax note on the card | −17 dB |

## 6. End card

A 5.5 s neon title card:

- The title in two neon lines strikes on at 0.35 s.
- A "made with" line appears at 1.5 s, then your logo at 2.3 s.
- Smoke drifts behind, and it fades out from 4.9 to 5.5 s.
- Put the neon buzz and the sax note under it.

## 7. Check and export

- Mix to about −13 to −14 LUFS integrated, with the gunshot as the peak moment.
- Watch the opening, the middle and the climax frame by frame. Look for balloons off the frame or over faces, sound words over balloons, and wrong caption text.
- Export 2560×1440 at 24 fps, H.264 at about 15 Mbps with AAC audio, for YouTube.

## 8. Thumbnails (optional)

Use Atlabs `generate_image` at 16:9 with 2 or 3 scene frames as references, so the style and characters match.

- Make 6 or 7 options, each with one huge 2 to 4 word hook. For example: an emotional reveal, a forbidden door, a question, three comic panels, the last line, "AI made this".
- Proofread every word of the text in each image, and fix typos with an edit pass.
- Save 1920×1080 copies under 2 MB.

## Wrap-up

Tell the user in a few lines what you did: the look, the pacing, the music, the number of sound effects, any caption fixes and the end card. Include the credits spent.
