# Dream Reel

Turn a short story into a video on an Android phone, fully offline, using [Local Dream](https://github.com/xororz/local-dream) to generate the frames.

You write scenes, Dream Reel asks Local Dream for each frame (each frame grows out of the one before it), and it joins the frames into an MP4.

## Files

| File | What it is |
| --- | --- |
| `index.html` | **Dream Reel Lite**, the simple version. Scene cards, a characters box, an optional start image with a "what can change" mask, preview, and MP4 export. Start here. |
| `dream-reel-full.html` | The full version with every setting visible: action timeline, photo and video sources, video restyling, and separate strength sliders. |

Each page is one self-contained HTML file with no outside libraries.

## Using it on the phone

1. In Local Dream, open an NPU model and wait until it has loaded, then press **Home** (not Back) so it keeps running.
2. Download `index.html` to the phone and open it in Chrome from the Files app.
3. Check that the status button says **Connected**. If Chrome asks to allow access to devices on your network, allow it.
4. Describe the characters, write one line per scene, and set the frames per scene. Then tap **Generate**.
5. Tap **Preview** to watch it, then **Make video** and **Save video**.

Generate only makes what's missing: new scenes, edited scenes, and extra frames. To try again with different pictures:

- **Redo all frames** (under the scenes) remakes everything with new seeds. **Clear all frames** deletes them but keeps your text.
- **Redo scene** and **Clear** on a scene card do the same for one scene.
- Tap a frame for **Redo from here to the end** (this frame and everything after it) or **Redo only this frame**.
- Every redo asks whether to **use the current frames as samples**. On: each new frame starts from the frame before it plus its old version, so it keeps the look and changes details. Off: it starts only from the frame before it. The choice is remembered.

Each scene has its own **Choose what can change**: paint over the scene's picture which areas may change, and everything else stays as it is in every frame of that scene. It's drawn over the scene's first frame, or the picture the scene grows out of. **Reset** removes it. It's ignored while that scene has a camera move. The start image's choice applies to scenes without their own.

Each new frame always starts from the one before it; only the very first frame (with no start image) starts from text alone. While frames are being made, the small picture in the bottom bar loops through every frame so far and jumps to each new one. Tap it for a large view.

Each scene makes 1 frame by default. Change it per scene with − / +, or for new scenes in **Settings → Frames per new scene** (**Use for all scenes** applies it everywhere). Open **Prompt sent to Local Dream** on a scene to see the exact prompt, with any part the model ignores struck through. Work autosaves in the phone's browser storage.

## Privacy

The pages only talk to Local Dream's engine on the same phone at `http://127.0.0.1:8081`. A Content-Security-Policy in each file blocks every other network request, so nothing is uploaded.

While Local Dream is open, its engine also accepts requests from other web pages in Chrome. Close Local Dream when you're not using it, and leave its "Allow LAN access" and "Host mode" settings off.

## Limits

- **Prompt length:** Stable Diffusion 1.5 reads about 75 tokens per prompt and ignores the rest. Each scene card shows the count from Local Dream.
- **Not a video model:** smooth motion comes from small changes between frames, camera moves, and blending. Two characters in fast action are hard for these models.
- **Resolution:** on NPU, pick the same size Local Dream loaded the model at (its Advanced Settings, then reload the model). A different size gives coloured noise; the page detects that, drops the frame and stops. If unsure, use 512 × 512. CPU models support squares up to 512 × 512.

## Website

Once GitHub Pages is on (**Settings → Pages**, source **Deploy from a branch**, branch `main`, folder `/ (root)`), the app is at:

**https://beamancorp.github.io/dream-reel/** (full version: `/dream-reel-full.html`)

Open it in Chrome on the phone and use **⋮ → Add to Home screen**. After the first visit it also opens without internet; only Local Dream is needed. When online, it picks up the latest version automatically.
