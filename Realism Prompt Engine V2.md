---
name: realism-prompt-framework
description: Build character sheets (always the fixed three-view headless layout), prop sheets, realistic Nano Banana Pro image prompts and realistic Seedance 2.5 multi-shot video prompts with a shared camera identity.
---

# Realism Prompt Framework — Sheets, Realistic Stills, Realistic Video

One skill, three outputs that share one look:

1. **Character & prop sheets**: neutral reference captures on grey. Character sheets ALWAYS use the fixed three-view headless framework in Step 5. No realism pass is applied to sheets.
2. **Realistic image prompts**: a Nano Banana Pro base prompt with an **image realism pass** appended.
3. **Realistic video prompts**: a Seedance 2.5 multi-shot prompt with a **video realism pass** built into the shots and the metadata.

Deliver only what the user asks for. If the request is ambiguous about which output they want, ask one short question. If they ask for "the full package", the order is: sheets → realistic stills → video prompt.

---

## The golden rule: content vs capture

Real footage looks real because of accumulated error from the camera, the lens, the sensor, the processing and the person holding it. This skill adds that error and nothing else.

**The base prompt owns CONTENT.** The realism pass never adds, removes or alters any of it:
- the character: identity, build, grooming, physical state
- performance, expression, gaze, pose, gesture
- wardrobe, including its condition (clean, worn or stained is as given)
- props and their condition
- the environment, set dressing and surface wear
- action, blocking, timing and dialogue
- the light sources present in the scene (a window, a lamp, the sun)

**The realism pass owns CAPTURE only:**
- the device, its era and how the image was taken or shared
- framing and operator error (tilt, crop, distance, loose composition, missed focus)
- lens behaviour, sensor behaviour and in-camera processing
- how the camera exposes the existing light (clipping, crushing, white balance)
- how existing surfaces resolve (pores visible, weave legible, detail smeared)
- in video: camera behaviour, motion artefacts, temporal flaws and microphone character

Write the pass **generically**, with phrases like "the subject", "bright edges" or "any fine repeating pattern", so it appends to any base prompt unchanged. If the base prompt is short on texture, fix that in the base prompt, using the user's own content. The pass does not invent new content to fix it.

---

## Step 1 — Pick a camera identity (shared across stills and video)

Choose one identity per scene and use it for both the stills and the video, so the start frame and the footage match. Each identity has **one anchor flaw**: a dominant signature strong enough to pull the whole image into that device and era. Without an anchor, the model falls back to a modern, clean sensor.

| Identity | Anchor flaw | Lens / sensor / processing | Video behaviour | Aspect |
|---|---|---|---|---|
| **Early smartphone (2010–2013)** | Clipped highlights + heavy phone sharpening | Soft corners, purple fringing, coarse shadow noise, waxy noise-reduction smear | Handheld shake, exposure pumping, autofocus hunting, rolling shutter, compression blocking in motion | 4:3 |
| **Modern phone (2020s)** | Computational processing: tone-mapped HDR look, crunchy sharpening | Ultrawide distortion only at 0.5x, smeared low-light detail | Stabilisation warp, digital-zoom softness | 4:3 / 9:16 |
| **2000s compact digicam** | Direct on-camera flash (at night, or as daylight fill) | ~35mm-equivalent wide end (never ultrawide), low dynamic range, chroma noise, JPEG blocking | n/a (stills) or low-res 4:3 clip mode | 4:3 |
| **Disposable / point-and-shoot film** | Flash + heavy grain + colour shift | Soft plastic lens, light leak at the edge, cast towards green/magenta | n/a | 3:2 |
| **Consumer camcorder (2000s)** | Video-look softness + auto-everything | Low resolution, smeared colour, blown skies | Zoom-rocker zooms, autofocus hunting, auto-exposure drift, motion smear | 4:3 |
| **Webcam / video call** | Low-res washed-out frame with a blown window | Heavy noise, soft focus | Frame-rate stutter, compression bursts | 16:9 low-res |
| **Prosumer mirrorless, handheld documentary** | Operator error: loose framing, focus landing slightly off | Mixed white balance, mild vignette | Handheld micro-jitter, late reframes, overshoot and settle | 16:9 |

Rules:
- **Flaws must be consistent with the identity.** No ultrawide on a compact, no film grain on a phone, no flash flare on a camcorder in daylight.
- **Prefer phone identities on Nano Banana Pro.** Its phone-camera vocabulary is its strongest realism lever, and without "phone photo" or "snapshot" language it falls back to commercial-portrait polish.
- **Naming a real device and era** ("iPhone 4 in 2011", "Canon PowerShot compact around 2006") anchors harder than describing one. Adding how the image was shared ("posted straight to Facebook") also carries implied degradation.
- **Intensity dial.** *Subtle* = anchor plus 1 flaw per stage. *Medium* = anchor plus 1–2 per stage. *Heavy* = anchor plus 2 per stage. Default to medium.

---

## Step 2 — Base image prompt (Nano Banana Pro method)

Write the content first, as a single flowing natural-language paragraph that reads like a creative-director brief. Cover the subject, action and pose, expression, setting, and the light sources present, all exactly as the user gave them. Fill gaps only where the user left them open. Include the user's texture and material callouts here, because these are content.

If the user supplies a character reference, open with "Using the attached reference." and keep the in-prompt likeness to the few features the reference might drift on. Let the image carry the rest.

When the realism pass will be applied, **leave all polish and style words out of the base**: cinematic, epic, IMAX, anamorphic, flawless, perfect, hyper-detailed, 8k, masterpiece, ultra-realistic render, studio lighting, beauty lighting, shallow-depth-of-field portrait. These describe capture, not content, and they push the model towards glossy CGI.

---

## Step 3 — Image realism pass

Append the pass as a second paragraph, written in **capture-chain order** so the model reads every flaw as a consequence of one device:

1. **Capture line**: device, era, how it was taken or shared, aspect ratio. ("Captured as an unedited 4:3 photo on an iPhone 4 in 2011 and posted straight to Facebook…")
2. **Operator**: distance, tilt, off-centre placement, awkward crop, focus landing slightly off.
3. **Lens**: one flaw (soft corners, fringing on bright edges, distortion only if the identity has it).
4. **Sensor**: one or two flaws (clipped highlights, crushed shadows, noise in the shadows, moiré on fine repeating patterns).
5. **Processing**: one or two flaws (sharpening halos, noise-reduction smear, JPEG blocking, an off white balance).
6. **Closing negatives**: generic polish only, "No HDR, no retouching, no beauty lighting, no text, no watermarks."

Rules:
- **5–7 flaws in total, never 15.** Long flaw lists get averaged into "generally gritty" and none of them land. Two or three precise observations beat a long inventory.
- **State the flaw you want, positively.** Don't put a specific drifting attribute in the negatives, because naming it anchors it. Generic negatives like "no HDR" are fine.
- **Video start frames are different:** when a still will be used as a Seedance start frame, leave grain, noise and compression **out** of the still. Baked-in noise boils once it moves. Put those flaws in the video pass instead.
- **Settings override the prompt.** Aspect ratio and resolution come from the generation settings, so tell the user to set them to match the identity (for example 4:3, low resolution).
- **Post notes.** A sharp 2K render can't fully pass as an old JPEG. For the last mile, suggest downscaling and re-saving as low-quality JPEG, and adding grain or halation in post.

### Image example (early smartphone, medium intensity)

```
Using the attached reference. A man with a full auburn-tinged beard, thin gold wire-frame glasses, an olive cap worn backwards, an off-white tee and dark jeans stands outside the open door of a timber garden office in a back garden, holding a mug. Midday sun.

Captured as an unedited 4:3 photo on an iPhone 4 in 2011 and posted straight to Facebook, snapped quickly from about three metres away, tilted slightly, the subject off-centre to the left and cut off at the knees. The cheap lens leaves soft corners and purple fringing where bright edges meet the sky. The tiny sensor clips the sky to pure white, crushes the darkest areas to black, and puts coarse colour noise in the shadows, with faint moiré on any fine repeating pattern. Heavy phone processing adds bright sharpening halos around the subject's outline and smears fine detail into waxy blotches. No HDR, no retouching, no text, no watermarks.
```

---

## Step 4 — Realistic video prompt (Seedance 2.5)

Run the standard Seedance 2.5 multi-shot framework, then apply the video realism pass. The core rules carry over unchanged:

- The total is **exactly 30 seconds** unless the user asks for another length. Shots run **2–5 seconds** each. Each opens with a `[MM:SS - MM:SS]` stamp and the last ends at the total length.
- Each shot specifies **shot size, lens, angle and movement**, varied between consecutive shots.
- Write one paragraph per shot inside a **single fenced code block**, with no lists or headers.
- The prompt is a hard maximum of **7,500 characters**.
- The consistency locks still apply where the scene needs them: character lock, blocking lock, persistent-prop lock, continuous-action lock and state-change props. State the trait you want, never the trait you don't.
- The tagged-reference rules still apply: spaces around `@tag`, inline tags win, the manifest for bulk refs, and environment-reference-only wording for plates.
- Dialogue still gets a line per shot, in quotes, each line tagged separately, with "ensure the voice matches @[tag]".

### What the video realism pass changes

**1. Choose the edit logic.**
- **Single camera** (any amateur identity, the default): one operator and one device. Every cut reads as the recording stopping and starting from the same person, like jump cuts. No reverse angles or cutaways that would need a second camera, and no positions the operator couldn't physically reach. Favour fewer, longer shots (6–8).
- **Coverage** (the prosumer documentary identity, or when the user's scene needs POVs and reaction angles): multiple angles are allowed, as if one shooter is grabbing coverage across takes, but every move stays handheld and imperfect.
- **One continuous take** (when the user asks for it): the timestamps mark beats within one unbroken take, not cuts. Open each beat with "the same take continues", make every framing change by zooming or by the operator moving, and add a continuity lock.

**2. Express the four shot parameters within what the device can do.** The variation rule still applies, but only inside those limits:
- Lens becomes device focal: "phone at widest", "phone 1x", "zoomed in 2x so detail softens", "camcorder zoomed fully in".
- Movement becomes operator movement: handheld static, handheld pan, walk-in, walk-with, quick whip-pan to, set down on a surface. **No orbits, cranes, dolly moves or drone moves** unless the device could genuinely do them.

**3. Add operator behaviour to some shots, not all of them.** Use one per shot at most: a late reframe that overshoots and settles, autofocus hunting, exposure pumping as bright areas enter the frame, or a stumble in the walk. Flaws in every shot read as an effect. Flaws in some shots read as a person filming.

**4. Replace the fixed Style line with a Capture line.** The standard IMAX/anamorphic Style line contradicts realism, so in this mode it is replaced by:
`Capture: [identity, era, aspect] — [anchor flaw], [2–4 motion/temporal flaws from the identity table].`
The identity must match the stills exactly, or the model re-polishes the footage.

**5. Microphone character in the Audio line.** Diegetic-only still applies, but recorded through the device: "recorded through the phone's built-in mic, thin and slightly tinny, wind buffeting the mic, handling noise when the phone moves, automatic gain pumping between loud and quiet moments." This affects how the sound was captured, not what sounds exist.

**6. Negatives.** Keep the standard negatives. Drop "no distortion" if the identity has distortion, and add "No gimbal or stabilised movement, no cinematic colour grade."

### Metadata block (realism mode)

```
Location: [as given, with time of day]
Capture: [identity line]
Audio: [diegetic line with mic character]
Negatives: No subtitles, no on-screen text, no captions, no watermarks, no music, no score, no BGM. No face-swapping or identity drift between shots, no extra fingers or limbs. No gimbal or stabilised movement, no cinematic colour grade.
[Any lock lines the scene needs]
```

### Video example (early smartphone, single camera)

*User brief: phone video of the man chatting to camera outside his garden office, then heading inside. Character reference supplied as @jack.*

```
[00:00 - 00:05] Wide, phone at widest, chest height, handheld walk-in — the operator walks across the back garden towards @jack , who stands outside the open door of the timber garden office holding a mug; the frame bobs with every step.

[00:05 - 00:10] Medium, phone 1x, slightly low angle, handheld static — @jack turns to camera and talks, mug in both hands; the framing drifts left and corrects late.

[00:10 - 00:14] Medium close-up, zoomed in 2x so detail softens, eye-level, handheld — his face and glasses fill more of the frame; autofocus briefly hunts and settles.

[00:14 - 00:20] Wide, phone at widest, high angle, slow handheld pan — the operator pans with @jack as he steps towards the open office door; exposure pumps darker as the bright sky swings into frame.

[00:20 - 00:25] Medium, phone 1x, eye-level, handheld walk-with — following a step behind as he goes through the doorway; the interior drops to near black before the exposure slowly lifts.

[00:25 - 00:30] Medium wide, phone 1x, low angle, set down on a surface, static — the phone rests on a desk, slightly crooked, @jack settling into frame as the recording runs on.

Location: Back garden and timber garden office, midday sun.
Capture: Handheld 2011 iPhone 4 video, 4:3 — highlights clipping to white, constant small hand shake, rolling-shutter skew on faster movement, smeared motion blur, compression blocking that worsens during motion, noise crawling in the shadows.
Audio: Diegetic sound only, recorded through the phone's built-in mic — in-world speech, garden ambience and foley, thin and slightly tinny, wind buffeting the mic outside, handling noise when the phone moves, automatic gain pumping between loud and quiet moments.
Negatives: No subtitles, no on-screen text, no captions, no watermarks, no music, no score, no BGM. No face-swapping or identity drift between shots, no extra fingers or limbs. No gimbal or stabilised movement, no cinematic colour grade.
Character lock — @jack : full auburn-tinged beard, thin gold wire-frame glasses, olive cap worn backwards, dark brown eyes, in every shot.
```

---

## Step 5 — Character & prop sheets (neutral references)

Sheets are clean reference captures. **Never apply the realism pass to a sheet**, because flaws baked into a reference get inherited everywhere downstream. The look belongs to the scene frames and the video. Sheets use clean studio light on grey with no grade.

### Character sheet — ALWAYS the three-view headless framework

**This is the only character sheet layout.** Whenever the user asks for a character sheet, character reference sheet, turnaround or reference sheet for a character, use this exact template. Never fall back to any other layout: no four-view or five-view sheets, no reordering the views, no swapping a view for a different angle, no extra panels, and no freeform arrangement, even when the request is a single short line ("make a character sheet for X"). Copy the template word for word and fill in only the bracketed parts.

```
Character reference sheet — three views on a neutral grey studio background, arranged left to right:
[VIEW 1 — FULL BODY, FRONT, HEADLESS] Full-body front-facing view of this character's costume and body, standing straight-on, arms slightly away from the sides. Full body visible from neck to feet — no head, the costume shown as if worn on an invisible/absent head. Focus on how the garments hang, layer, and fit.
[VIEW 2 — FULL BODY, REAR] Full-body rear view of the same character, directly from behind, standing in the same neutral pose. Full body visible head to feet, including the back of the head, and the rear of every garment. [HEADWEAR LINE]
[VIEW 3 — FRONT CLOSE-UP] Head-and-shoulders extreme close-up, straight-on front view. Sharp, detailed focus on realistic skin texture, facial features, and surface detail.
Character: [CHARACTER DESCRIPTION]
Lighting & presentation: Clean studio lighting — soft key light from the upper left, gentle fill from the right. Consistent character identity, proportions, and costume details across all three views. Even, neutral grey seamless backdrop.
Constraints: No text, no watermarks, no extra figures, no background environment.
```

**Filling the template:**
- **[CHARACTER DESCRIPTION]**: the photoreal character description exactly as the user gave it: identity, age, hair, eyes, build, wardrobe and its condition, injuries, expression. Fill gaps only where the user left them open. For visible skin, call for true micro-pores and fine natural texture, because this is how the skin resolves rather than a change to the character.
- **[HEADWEAR LINE]**: if the character wears a cap or hat, add a line confirming its rear detail is visible (e.g. "Brim of the cap visible."). If not, leave the line out entirely.
- **View 1 garment wording** may be made specific to the costume (e.g. "Focus on how the armour plates sit, overlap, articulate and fit") without changing anything else in the view.
- **Reference supplied**: open the Character line with "Using the attached reference — the same man/woman, same face, same [the few features most likely to drift]." Keep everything else in the template unchanged.
- **Variation sheets** (ageing, injuries, costume changes): use the same template, anchor the Character line hard to the supplied reference, and state only what the user wants changed.
- **Layered versions** (a helmet, mask or headwear added later): write an edit prompt on the existing sheet output rather than a new sheet. Keep the headless front view exactly as it is, add the item only in the rear view and the close-up, and keep everything else unchanged.
- **Creatures and non-humanoids**: keep the same three-panel structure, lighting, backdrop and constraints. The headless view exists to show costume, so for a creature with no costume, View 1 becomes a full-body front view with the head included. For a quadruped, View 2 becomes a full-body 90-degree side profile from snout to tail, because a rear view hides the silhouette, limbs and tail. Say in one line which adaptation you made.

### Filled example

```
Character reference sheet — three views on a neutral grey studio background, arranged left to right:
[VIEW 1 — FULL BODY, FRONT, HEADLESS] Full-body front-facing view of this character's costume and body, standing straight-on, arms slightly away from the sides. Full body visible from neck to feet — no head, the costume shown as if worn on an invisible/absent head. Focus on how the garments hang, layer, and fit.
[VIEW 2 — FULL BODY, REAR] Full-body rear view of the same character, directly from behind, standing in the same neutral pose. Full body visible head to feet, including the back of the head, and the rear of every garment. Brim of the cap visible.
[VIEW 3 — FRONT CLOSE-UP] Head-and-shoulders extreme close-up, straight-on front view. Sharp, detailed focus on realistic skin texture, facial features, and surface detail.
Character: A caucasian man in his mid-thirties. Full, well-grown mid-brown beard, short brown hair, brown eyes. He wears thin gold wire-frame glasses with squared-round lenses. A grey washed cotton cap worn backwards, adjuster strap across the forehead. Plain white oversized t-shirt, blue jeans and dark unbranded trainers.
Lighting & presentation: Clean studio lighting — soft key light from the upper left, gentle fill from the right. Consistent character identity, proportions, and costume details across all three views. Even, neutral grey seamless backdrop.
Constraints: No text, no watermarks, no extra figures, no background environment.
```

### Prop sheet

Props are not characters and use their own layout: front, rear, 3/4 detail and 90° profile views on the same neutral grey seamless backdrop with the same clean studio lighting and constraints, then the prop description exactly as given.

### Location plates

Environment as subject, no characters. In realism mode, a plate gets the **same camera identity** as the scene, applied at subtle intensity, so the space inherits the look. When attaching a plate to Seedance, use the environment-reference-only wording so it isn't cloned as a start frame.

---

## Delivery

- Each prompt goes in its **own fenced code block** with a short label line above it (e.g. `IMAGE PROMPT — Realistic still: garden office, early smartphone`).
- After the prompts, write a **companion note of 3–4 sentences**: which identity and intensity were used and why, the generation settings to match (aspect ratio, resolution), any post steps, and one tweak to try if the anchor flaw doesn't land. For video, suggest re-rolling a single failing shot rather than the whole sequence. For character sheets, the note covers any adaptation made (creature, layered edit) and the aspect ratio to generate at.
- Never describe changes to the character, wardrobe, performance or environment in the companion note. There shouldn't be any.

## Diagnosing a miss

- **Looks modern and clean:** the anchor flaw didn't land. Name a specific device model, add the sharing context, check the aspect ratio in settings, and remove any polish words left in the base.
- **Looks like the wrong device** (e.g. ultrawide on a compact): a flaw contradicts the identity. Remove it.
- **Generally gritty, nothing specific:** too many flaws. Cut back to the anchor plus one per stage.
- **Likeness drifting at distance:** strengthen the likeness locks in the base prompt or move the camera closer. Don't remove flaws to fix it.
- **Video re-polishes the still:** the Capture line doesn't match the image identity, or the movement vocabulary is too cinematic.
- **Character sheet came out in a different layout:** the template wasn't copied word for word. Regenerate from the exact three-view headless template in Step 5.
