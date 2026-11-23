# Project Handoff: Deeksha & Prashant Interactive Wedding Invitation

## 1. Project Overview & Hosting Details
* **Couple:** Deeksha & Prashant
* **Wedding Date & Location:** 10 February 2027 • Kotdwar
* **GitHub Repository:** `pgaur1/deeksha-prashant-wedding` (Public)
* **Live GitHub Pages URL:** `https://pgaur1.github.io/deeksha-prashant-wedding/`
* **Architecture:** Static, mobile-first single-page site (`index.html`) hosted on GitHub Pages. No backend server or database is used; all assets (HTML, images, audio) are served statically over HTTPS (within GitHub Pages' 1 GB site limit and 100 GB/month soft bandwidth limit).

---

## 2. Current Status & Tasks Completed
* **Core Website:** Interactive mobile-first wedding invitation deployed and active at `https://pgaur1.github.io/deeksha-prashant-wedding/`.
* **WhatsApp Link Preview (Android & iOS):**
  * Open Graph tags (`og:title`, `og:description`, `og:image`, `og:url`) are configured in `index.html` pointing to `https://pgaur1.github.io/deeksha-prashant-wedding/invite-preview.jpg`.
  * **Key Constraint for iOS & Android WhatsApp:** `invite-preview.jpg` **must** be a true wide landscape JPEG (`1200 × 630` pixels, `1.91:1` aspect ratio) and under **500 KB**. Tall portrait images (`768 × 1376` or `1536 × 2752`) or vertical cards padded with heavy side blur fail or render poorly on iOS WhatsApp's preview crawler.
  * **Cache-Busting Testing:** Whenever `invite-preview.jpg` or `index.html` is updated on GitHub, append a new query parameter (e.g., `?v=8`, `?v=9`, `?v=10`) to the clean base URL (`https://pgaur1.github.io/deeksha-prashant-wedding/?v=9`) with no Markdown brackets or extra spaces when testing in WhatsApp.

---

## 3. Pending Enhancements to Integrate into `index.html`
The user wants to keep **all existing invitation sections and details** intact in `index.html`, while adding two interactive enhancements:
1. **3D Cinematic Movie-Style Opening Scene:** A full-viewport arched double-door entrance (`You're Invited • Tap to Enter`) framed with bougainvillea florals that swings inward in 3D perspective (`rotateY`), zooms into a sunlit terrace hero reveal (`We're Getting Married • Deeksha & Prashant • 10 February 2027 • Kotdwar`), and smoothly transitions into scrolling through the rest of the existing invitation.
2. **Optional Background Music Player:** A floating `Play / Pause Music` button paired with `<audio id="bg-music" src="./wedding-music.mp3" loop preload="metadata">` (keeping the `.mp3` under 3–5 MB) that starts automatically when the user taps to open the doors or taps the floating music toggle.

---

## 4. Ready-to-Use Code Modules for `index.html`

### A. Required Open Graph Meta Tags (Inside `<head>`)
Ensure `index.html` includes clean absolute URLs for the preview card:
```html
<meta property="og:type" content="website" />
<meta property="og:title" content="Deeksha &amp; Prashant — Wedding Invitation" />
<meta property="og:description" content="10 February 2027 • Kotdwar • Tap the invitation card to open your interactive wedding invite." />
<meta property="og:url" content="https://pgaur1.github.io/deeksha-prashant-wedding/" />
<meta property="og:image" content="https://pgaur1.github.io/deeksha-prashant-wedding/invite-preview.jpg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta name="twitter:card" content="summary_large_image" />
```

### B. Cinematic 3D Arched Door Opening & Zoom Scene (Top of `<body>`, Above Existing Content)
Place the `<style>` block in `<head>` (or above the section) and place `<section id="cinematic-opening">` immediately inside `<body>` before the existing invitation container so all existing sections remain untouched below it.

```html
<style>
  html {
    scroll-behavior: smooth;
  }

  /* --- Cinematic 3D Door Opening & Zoom Scene --- */
  #cinematic-opening {
    position: relative;
    width: 100%;
    height: 100vh;
    height: 100dvh;
    overflow: hidden;
    perspective: 1200px;
    background: #1b1210;
    user-select: none;
  }

  /* Inner Scenic Terrace Backdrop (zooms forward as doors open) */
  .scene-interior {
    position: absolute;
    inset: 0;
    background:
      linear-gradient(to bottom, rgba(20, 14, 18, 0.25), rgba(20, 14, 18, 0.55)),
      radial-gradient(circle at 50% 35%, #ffe6c7 0%, #f3b58c 38%, #6a7ba2 72%, #2b354f 100%);
    transform: scale(1);
    transition: transform 2.4s cubic-bezier(0.22, 1, 0.36, 1);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 24px;
    text-align: center;
    z-index: 1;
  }

  /* Optional: Uncomment to use a custom scenic image (e.g., terrace-bg.jpg) in the repo */
  /*
  .scene-interior {
    background-image: linear-gradient(rgba(18, 10, 14, 0.35), rgba(18, 10, 14, 0.6)), url('./terrace-bg.jpg');
    background-size: cover;
    background-position: center;
  }
  */

  #cinematic-opening.is-opening .scene-interior {
    transform: scale(1.16);
  }

  /* Couple Hero Text (fades in after doors swing open) */
  .hero-reveal {
    opacity: 0;
    transform: translateY(24px) scale(0.97);
    transition: opacity 1.1s ease 1.15s, transform 1.1s cubic-bezier(0.22, 1, 0.36, 1) 1.15s;
    color: #fffaf2;
    max-width: 420px;
    text-shadow: 0 2px 16px rgba(0, 0, 0, 0.45);
  }

  #cinematic-opening.is-opening .hero-reveal {
    opacity: 1;
    transform: translateY(0) scale(1);
  }

  .hero-kicker {
    font-size: 13px;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: #ffe2a8;
    margin-bottom: 10px;
  }

  .hero-names {
    font-family: Georgia, "Times New Roman", serif;
    font-size: clamp(34px, 8vw, 48px);
    font-weight: 700;
    line-height: 1.15;
    margin: 0;
    color: #ffd97d;
  }

  .hero-amp {
    display: block;
    font-size: 24px;
    font-style: italic;
    color: #fff6e5;
    margin: 4px 0;
  }

  .hero-meta {
    margin-top: 14px;
    font-size: 15px;
    line-height: 1.6;
    color: #fff8ee;
  }

  .scroll-cue {
    margin-top: 28px;
    font-size: 12px;
    letter-spacing: 0.22em;
    text-transform: uppercase;
    color: #ffe7ba;
    animation: pulseScroll 1.8s infinite ease-in-out;
  }

  @keyframes pulseScroll {
    0%, 100% { opacity: 0.45; transform: translateY(0); }
    50% { opacity: 1; transform: translateY(5px); }
  }

  /* Outer Archway Wall & Doors Wrapper */
  .arch-stage {
    position: absolute;
    inset: 0;
    z-index: 3;
    display: flex;
    align-items: center;
    justify-content: center;
    transform-style: preserve-3d;
    transition: transform 2.2s cubic-bezier(0.22, 1, 0.36, 1), opacity 0.9s ease 1.5s;
    cursor: pointer;
  }

  #cinematic-opening.is-opening .arch-stage {
    transform: scale(1.65);
    opacity: 0;
    pointer-events: none;
  }

  /* Stucco Wall Mask with Arched Cutout */
  .arch-wall {
    position: relative;
    width: min(84vw, 360px);
    height: min(74vh, 540px);
    border-radius: 180px 180px 12px 12px;
    box-shadow:
      0 0 0 100vmax #f7f2eb,
      inset 0 0 28px rgba(0, 0, 0, 0.35);
    display: flex;
    transform-style: preserve-3d;
    overflow: visible;
  }

  /* Bougainvillea Floral Accent Header */
  .floral-crown {
    position: absolute;
    top: -34px;
    left: -18px;
    right: -18px;
    height: 86px;
    border-radius: 50%;
    background:
      radial-gradient(circle at 20% 55%, #e84a3c 0 18px, transparent 20px),
      radial-gradient(circle at 38% 35%, #ff6b5b 0 22px, transparent 24px),
      radial-gradient(circle at 55% 28%, #d93829 0 24px, transparent 26px),
      radial-gradient(circle at 74% 40%, #ff7666 0 20px, transparent 22px),
      radial-gradient(circle at 86% 62%, #c92e20 0 16px, transparent 18px),
      radial-gradient(circle at 48% 50%, #3d6b35 0 36px, transparent 38px);
    pointer-events: none;
    z-index: 5;
  }

  /* Double Arched Wooden Doors */
  .arch-door {
    position: relative;
    width: 50%;
    height: 100%;
    background: linear-gradient(180deg, #4fa3c7 0%, #347fa1 55%, #245f7a 100%);
    border: 2px solid #23536b;
    box-shadow: inset 0 0 18px rgba(10, 32, 44, 0.35);
    transition: transform 1.6s cubic-bezier(0.25, 1, 0.35, 1);
    backface-visibility: hidden;
  }

  .arch-door-left {
    border-top-left-radius: 180px;
    border-bottom-left-radius: 10px;
    transform-origin: left center;
  }

  .arch-door-right {
    border-top-right-radius: 180px;
    border-bottom-right-radius: 10px;
    transform-origin: right center;
  }

  .arch-door::before {
    content: "";
    position: absolute;
    inset: 28px 14px;
    border: 1.5px solid rgba(255, 255, 255, 0.35);
    border-radius: inherit;
  }

  /* Inward 3D Swing */
  #cinematic-opening.is-opening .arch-door-left {
    transform: rotateY(105deg);
  }

  #cinematic-opening.is-opening .arch-door-right {
    transform: rotateY(-105deg);
  }

  /* Center Callout Badge Across Closed Doors */
  .door-badge {
    position: absolute;
    inset: 0;
    z-index: 4;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    color: #fffaf0;
    padding: 16px;
    pointer-events: none;
    transition: opacity 0.45s ease;
  }

  #cinematic-opening.is-opening .door-badge {
    opacity: 0;
  }

  .door-badge-title {
    font-family: Georgia, "Times New Roman", serif;
    font-style: italic;
    font-size: 32px;
    line-height: 1.1;
    text-shadow: 0 2px 10px rgba(0, 0, 0, 0.4);
  }

  .door-badge-sub {
    margin-top: 14px;
    padding: 7px 16px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.2);
    border: 1px solid rgba(255, 255, 255, 0.55);
    font-size: 12px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
  }
</style>

<section id="cinematic-opening" aria-label="Wedding Invitation Opening Scene">
  <div class="scene-interior">
    <div class="hero-reveal">
      <p class="hero-kicker">We're Getting Married</p>
      <h1 class="hero-names">
        Deeksha
        <span class="hero-amp">&amp;</span>
        Prashant
      </h1>
      <p class="hero-meta">
        10 February 2027 • Kotdwar<br />
        Together with their families, invite you to celebrate their wedding
      </p>
      <div class="scroll-cue">Scroll ↓</div>
    </div>
  </div>

  <div class="arch-stage" id="arch-trigger" role="button" tabindex="0" aria-label="Tap to open wedding doors">
    <div class="arch-wall">
      <div class="floral-crown"></div>
      <div class="arch-door arch-door-left"></div>
      <div class="arch-door arch-door-right"></div>
      <div class="door-badge">
        <div class="door-badge-title">You're<br />Invited</div>
        <div class="door-badge-sub">Tap to Enter</div>
      </div>
    </div>
  </div>
</section>
```

### C. Floating Background Music Toggle & Unified Interaction Script (Bottom of `<body>`)
```html
<button id="music-toggle" type="button" aria-label="Toggle wedding music"
  style="position:fixed;bottom:18px;right:18px;z-index:50;padding:10px 14px;border-radius:999px;border:1px solid #d4af37;background:#3b0d16;color:#fff8f0;font-size:14px;cursor:pointer;">
  🎵 Play Music
</button>

<audio id="bg-music" src="./wedding-music.mp3" loop preload="metadata"></audio>

<script>
  (function () {
    const openingScene = document.getElementById("cinematic-opening");
    const archTrigger = document.getElementById("arch-trigger");
    const musicBtn = document.getElementById("music-toggle");
    const bgMusic = document.getElementById("bg-music");
    let opened = false;
    let isPlaying = false;

    async function startMusic() {
      if (!bgMusic || isPlaying) return;
      try {
        await bgMusic.play();
        isPlaying = true;
        if (musicBtn) musicBtn.textContent = "🔊 Pause Music";
      } catch (err) {
        // Mobile browsers require direct user tap before audio playback
      }
    }

    function triggerDoorOpen(fromUserTap = false) {
      if (!opened) {
        opened = true;
        openingScene.classList.add("is-opening");
      }
      if (fromUserTap) {
        startMusic();
      }
    }

    if (archTrigger) {
      archTrigger.addEventListener("click", () => triggerDoorOpen(true));
      archTrigger.addEventListener("keydown", (e) => {
        if (e.key === "Enter" || e.key === " ") {
          e.preventDefault();
          triggerDoorOpen(true);
        }
      });
    }

    // Gently auto-open doors after 1.6s if guest hasn't tapped yet
    setTimeout(() => triggerDoorOpen(false), 1600);

    if (musicBtn && bgMusic) {
      musicBtn.addEventListener("click", async () => {
        try {
          if (!isPlaying) {
            await bgMusic.play();
            isPlaying = true;
            musicBtn.textContent = "🔊 Pause Music";
          } else {
            bgMusic.pause();
            isPlaying = false;
            musicBtn.textContent = "🎵 Play Music";
          }
        } catch (err) {
          console.error("Audio playback failed:", err);
        }
      });
    }
  })();
</script>
```

---

## 5. Next-Step Checklist for the Receiving Agent
1. **Merge into Existing `index.html`:** Insert the cinematic opening scene above the existing invitation sections without removing any existing venue, schedule, RSVP, or family details.
2. **Optimize `invite-preview.jpg`:** Ensure `invite-preview.jpg` in the root repository is cropped/composed as a true `1200 × 630` landscape JPEG under `500 KB` (without vertical letterbox blur) so iOS WhatsApp renders the rich preview card reliably.
3. **Add Audio Asset (Optional):** Upload `wedding-music.mp3` (compressed under 3–5 MB) to the root of `pgaur1/deeksha-prashant-wedding` if background music is enabled.
4. **Verify on GitHub Pages:** Wait 1–2 minutes after committing changes for `pages-build-deployment` to succeed, then test on Android and iOS WhatsApp using a clean cache-busted URL (`https://pgaur1.github.io/deeksha-prashant-wedding/?v=10`).