# Blog Dashboard — Setup Guide

Aapke blog ke liye ek **dashboard** ban gaya hai (Decap CMS). Isme aap login karke
blog likh sakte hain, cover image laga sakte hain, aur **Publish** dabate hi post live
ho jati hai. Posts `posts.json` mein save hoti hain aur `blog.html` unhe khud dikhati hai.

> Ek baar ka setup hai (GitHub + Netlify). Iske baad sirf `yoursite.com/admin` kholna
> hai aur likhna shuru. Ye steps sirf aap apne accounts se kar sakte hain.

---

## A. Files jo ban gayi hain
- `admin/index.html` + `admin/config.yml` — dashboard (CMS)
- `posts.json` — saari blog posts yahan store hoti hain
- `blog.html` — ab posts dynamically dikhata hai (hardcoded nahi)
- `index.html` — Netlify Identity login widget add ho gaya
- `netlify.toml` — Netlify settings (no build needed)
- `uploads/` — yahan aapki upload ki hui images jaayengi (auto ban jayega)

---

## B. One-time setup (Netlify — sabse aasan)

### 1. Site ko GitHub par daalein
- github.com par ek **new repository** banayein (private chalega).
- Is poore folder ko us repo mein push karein.
  (Agar git nahi pata to GitHub Desktop app use karein — drag & drop, "Commit", "Push".)

### 2. Netlify par deploy karein
- app.netlify.com → **Add new site → Import an existing project** → apna GitHub repo chunein.
- Build command: **khaali chhor dein**. Publish directory: **`.`** → Deploy.
- Site live ho jayegi (e.g. `your-site.netlify.app`).

### 3. Login enable karein (Identity + Git Gateway)
- Netlify site → **Site configuration → Identity → Enable Identity**.
- Identity → **Registration** ko **Invite only** kar dein (taake koi aur signup na kare).
- Identity → **Services → Git Gateway → Enable** karein.

### 4. Apne aap ko invite karein
- Identity → **Invite users** → apna email daalein → invite.
- Email mein aaye link par click karein → password set karein.

### 5. Dashboard kholein
- `your-site.netlify.app/admin/` par jayein → email/password se login.
- **Blog → Blog Posts** kholein → "Add Post" → likhein → **Publish**.
- 1–2 minute mein site khud update ho jayegi (Netlify rebuild + redeploy).

---

## C. Local par test karna (deploy se pehle, optional)
Terminal mein, is folder ke andar:
```
npx decap-server
```
phir dusri terminal mein site serve karein:
```
python -m http.server 8731
```
aur browser mein `http://localhost:8731/admin/` kholein. (Local mode mein login nahi
maangega — `config.yml` mein `local_backend: true` set hai.) Yahan likhi posts seedha
aapki local `posts.json` mein save hongi.

---

## A-2. Portfolio (same dashboard)

Dashboard mein ab **2 collections** hain:
- **Blog** → blog posts (`posts.json`)
- **Portfolio** → aapka client kaam (`portfolio.json`) — `portfolio-design.html` par dikhta hai

Portfolio item add karte waqt:
- **Type** chunein: Image / Video / Website / UI/UX Design (isi se frontend par category tab decide hota hai)
- **Cover image** upload karein (grid mein dikhega; GIF bhi yahan upload kar sakte hain)
- **Video or GIF URL** — sirf Video type ke liye (YouTube/Vimeo/.mp4 link)
- **External link** — Behance project ya live website ka URL (click karne par khulta hai)

`portfolio-design.html` link ho gaya hai: Index page ke "View Portfolio" button se + ArtxPremium ke "View Full Portfolio" button se.

## D-2. Dashboard password (local gate)

`/admin/` ab ek **password screen** se khulta hai (taake koi aur dashboard na khole).

- **Password** `admin/index.html` ki upar wali line mein set hai.
- **Change karne ke liye:** `admin/index.html` kholein aur ye line edit karein:
  ```js
  const ADMIN_PASSWORD = "your-password-here";   // ← apna password yahan likhein
  ```
  Save karein, phir `/admin/` par `Ctrl+Shift+R`.
  (Ya mujhe naya password bata dein, main set kar dunga.)

⚠️ **Note:** Ye **light protection** hai (password browser ke code mein hota hai — bohot tech-savvy banda bypass kar sakta hai). **Asli/strong security** deploy ke baad **Netlify Identity** degi (neeche section B). Dono saath kaam karte hain: pehle ye password, phir Netlify login.

> Tip: agar dubara password screen test karna ho to browser tab band karke dobara kholein (ya `Ctrl+Shift+R` se hard-refresh karein).

## D. Zaroori notes
- **Branch:** `admin/config.yml` mein `branch: main` set hai. Agar aapke repo ka default
  branch `master` hai to use `master` kar dein.
- **Vercel / GitHub Pages** use karna ho (Netlify ke bajaye) to mujhe batayein — backend
  ko GitHub OAuth par switch karna paRega (thoda alag setup).
- Har post ka **Slug** unique aur lowercase-with-dashes rakhein (e.g. `my-new-post`).
- **Featured** toggle ON karne se post upar bade card mein dikhti hai (sirf ek ko featured rakhein).
