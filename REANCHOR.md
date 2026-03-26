# SignalFox Tales — Project Re-Anchor Brief
**Last updated:** 2026-03-26  
**Repo:** github.com/SignalFoxSA/signalfox-tales  
**Build method:** Standalone HTML screens hosted on GitHub Pages  
**Role:** Claude writes ALL code. Ian reviews and uploads to GitHub.

---

## App Overview
- **Name:** SignalFox Tales — AI-powered mobile storytelling app
- **Tiers:** Guest · Free · Premium ($4.99/month or $39.99/year)
- **Genres:** 16
- **Voices:** 10 preset + Premium voice cloning
- **Features:** Family Mode, Community Feed, Streak tracking
- **Seed ask:** $500K–$800K

---

## Tech Stack
- Pure HTML/CSS/JS — no frameworks
- Standalone screens in `/screens/` folder
- Shared styles in `styles.css` at repo root
- Each screen links to next: `screen-XX.html`
- Unbuilt screens link back to `screen-01.html` temporarily
- Hosted on GitHub Pages

---

## Design System
| Property | Value |
|---|---|
| Background | Dark navy `#0a1020` → `#0d1530` → `#0f1e45` gradient |
| Gold | `#f5a623` — buttons, accents, active states |
| Text muted | `#a0b4d6` |
| Font | Nunito (Google Fonts) — 400, 600, 700, 800 |
| Button radius | 16px |
| Phone shell | 390×844px, border-radius 48px |
| Stars | 600 twinkling + 5 shooting stars on EVERY screen |

---

## Key Rules
- **NO** `mix-blend-mode` — damages images
- Always use original quality images, no pixel manipulation
- Star background JS snippet is identical on every screen
- `styles.css` is imported on every screen via `<link rel="stylesheet" href="../styles.css">`
- Screen HTML files live in `/screens/` folder

---

## Key Assets
| Asset | Location | Notes |
|---|---|---|
| Logo | `1000193362.png` | Transparent PNG, use as-is |
| Shelf nav image | `1000193701.png` | Transparent PNG, bottom nav all screens |
| Shared styles | `styles.css` (repo root) | Import on every screen |

---

## Shelf Nav (Standard Bottom Navigation)
Used on ALL screens from Screen 05 onward.

```html
<!-- SHELF NAV — paste inside .phone div, last child -->
<div class="shelf-nav">
  <img class="shelf-img" src="../assets/1000193701.png" alt="">
  <div class="st st-on"  onclick="location.href='screen-05.html'" style="left:22%;right:66.6%;top:3.5%;bottom:46.5%"></div>
  <div class="st" onclick="location.href='screen-11.html'" style="left:31.8%;right:58%;top:9.3%;bottom:48.8%"></div>
  <div class="st" onclick="location.href='screen-07.html'" style="left:39.2%;right:39.2%;top:0%;bottom:38.4%"></div>
  <div class="st" onclick="location.href='screen-11.html'" style="left:56.6%;right:34%;top:8.8%;bottom:47.7%"></div>
  <div class="st" onclick="location.href='screen-13.html'" style="left:64.4%;right:25.4%;top:8.1%;bottom:48.8%"></div>
</div>
```

**Tap zones (left to right):** Home → screen-05 · Library → screen-11 · Create → screen-07 · Community → screen-11 · Profile → screen-13

---

## Star Background JS (copy to every screen)
```javascript
const canvas = document.getElementById('starCanvas');
const ctx = canvas.getContext('2d');
const phone = document.querySelector('.phone');
canvas.width = phone.offsetWidth;
canvas.height = phone.offsetHeight;

const stars = Array.from({length: 600}, () => ({
  x: Math.random() * canvas.width,
  y: Math.random() * canvas.height,
  r: Math.random() * 1.4 + 0.3,
  alpha: Math.random(),
  speed: Math.random() * 0.008 + 0.002,
  dir: Math.random() > 0.5 ? 1 : -1
}));

const shootingStars = [];
function spawnShooting() {
  shootingStars.push({
    x: Math.random() * canvas.width * 0.7,
    y: Math.random() * canvas.height * 0.4,
    len: Math.random() * 80 + 60,
    speed: Math.random() * 6 + 5,
    alpha: 1, angle: Math.PI / 5
  });
}
for (let i = 0; i < 5; i++) setTimeout(spawnShooting, i * 1800);
setInterval(spawnShooting, 3000);

let animFrame;
function drawStars() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  stars.forEach(s => {
    s.alpha += s.speed * s.dir;
    if (s.alpha >= 1 || s.alpha <= 0.1) s.dir *= -1;
    ctx.beginPath();
    ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
    ctx.fillStyle = `rgba(255,255,255,${s.alpha})`;
    ctx.fill();
  });
  for (let i = shootingStars.length - 1; i >= 0; i--) {
    const ss = shootingStars[i];
    const grad = ctx.createLinearGradient(ss.x, ss.y,
      ss.x - Math.cos(ss.angle) * ss.len, ss.y - Math.sin(ss.angle) * ss.len);
    grad.addColorStop(0, `rgba(255,248,200,${ss.alpha})`);
    grad.addColorStop(1, 'rgba(255,248,200,0)');
    ctx.beginPath();
    ctx.moveTo(ss.x, ss.y);
    ctx.lineTo(ss.x - Math.cos(ss.angle) * ss.len, ss.y - Math.sin(ss.angle) * ss.len);
    ctx.strokeStyle = grad; ctx.lineWidth = 1.5; ctx.stroke();
    ss.x += Math.cos(ss.angle) * ss.speed;
    ss.y += Math.sin(ss.angle) * ss.speed;
    ss.alpha -= 0.018;
    if (ss.alpha <= 0) shootingStars.splice(i, 1);
  }
  animFrame = requestAnimationFrame(drawStars);
}
drawStars();

document.addEventListener('visibilitychange', () => {
  if (document.hidden) cancelAnimationFrame(animFrame);
  else animFrame = requestAnimationFrame(drawStars);
});
```

---

## Screen Status

| Screen | File | Status | Notes |
|---|---|---|---|
| 01 | screen-01.html | ✅ DONE | Splash — logo, taglines, Get Started/Log In/Guest, stars, language selector |
| 02 | screen-02.html | ✅ DONE | Onboarding carousel — 3 slides, auto-advance, swipe, dots |
| 03 | screen-03.html | ✅ DONE | Sign Up — email/password/name, social, free tier badge, T&C |
| 04 | screen-04.html | ✅ DONE | Log In — email/password, Face ID, Remember Me, Forgot Password |
| 05 | screen-05.html | ✅ DONE | Home Dashboard — profile hero, badges, usage bar, stories, streak, AI picks, shelf nav |
| 06 | screen-06.html | ⬜ TODO | Genre Selection |
| 07 | screen-07.html | ⬜ TODO | Story Generator |
| 08 | screen-08.html | ⬜ TODO | Story Player |
| 09 | screen-09.html | ⬜ TODO | Voice Selection |
| 10 | screen-10.html | ⬜ TODO | Family Mode |
| 11 | screen-11.html | ⬜ TODO | Library |
| 12 | screen-12.html | ⬜ TODO | Story Complete |
| 13 | screen-13.html | ⬜ TODO | Profile |
| 14 | screen-14.html | ⬜ TODO | Settings / Upgrade |
| 15–21 | — | ⬜ TBD | To be defined |

---

## Screen Links Map
| From | To | Trigger |
|---|---|---|
| screen-01 | screen-02 | Get Started |
| screen-01 | screen-04 | Log In |
| screen-01 | screen-05 | Continue as Guest |
| screen-02 | screen-03 | Finish onboarding |
| screen-03 | screen-05 | Account created |
| screen-03 | screen-04 | Log In link |
| screen-03 | screen-05 | Continue as Guest |
| screen-04 | screen-05 | Logged in |
| screen-04 | screen-03 | Sign Up link |
| screen-04 | screen-05 | Continue as Guest |
| screen-05 | screen-07 | Create a Story / Wand |
| screen-05 | screen-08 | Continue / Play |
| screen-05 | screen-11 | Library / Community |
| screen-05 | screen-13 | Profile / Fox |
| screen-05 | screen-14 | Upgrade |

---

## Working Conventions
- **"Brake"** = stop and update everything before continuing
- **"Back"** = carry on where we left off
- Claude builds screen by screen, Ian confirms before moving on
- Every new chat: paste this file to re-anchor Claude
- Update this file before ending every chat session

---

## Free Tier Spec
- 5 stories/month
- 16 genres available
- 2 voices
- Community feed access
- No voice cloning

## Premium Tier Spec
- Unlimited stories
- All 16 genres
- 10 preset voices + voice cloning
- Family Mode
- $4.99/month or $39.99/year
