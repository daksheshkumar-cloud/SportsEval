# ⚡ Sports Eval — Complete Setup & Hosting Guide

## Project Structure
```
sports-eval/
├── index.html          ← Login / Registration Page
├── main.html           ← Dashboard (main hub)
├── training.html       ← Training Plans & Workout Mode
├── progress.html       ← Progress Charts & Analytics
├── profile.html        ← User Profile & Settings
├── about.html          ← About Page (public)
├── css/
│   ├── global.css      ← Design system, variables, navbar, cards
│   └── animations.css  ← All keyframes and animation utilities
└── js/
    └── data.js         ← All data, helpers, buildNavbar(), SE object
```

---

## 🚀 Prerequisites (Already Included via CDN)

| Library         | Version | Purpose                        |
|-----------------|---------|-------------------------------|
| Bootstrap       | 5.3.2   | Responsive grid & components   |
| Font Awesome    | 6.4.0   | Icons throughout the app       |
| Chart.js        | 4.4.0   | Line, Radar, Doughnut charts   |
| Google Fonts    | —       | Rajdhani + Nunito typography   |

No npm install needed. All CDN links are already in the HTML files.

---

## ▶️ Running Locally

### Option 1: VS Code Live Server (Recommended)
1. Open the `sports-eval/` folder in VS Code
2. Install "Live Server" extension
3. Right-click `index.html` → "Open with Live Server"
4. Opens at `http://127.0.0.1:5500`

### Option 2: Python HTTP Server
```bash
cd sports-eval/
python -m http.server 8080
# Visit http://localhost:8080
```

### Option 3: Node.js serve
```bash
npm install -g serve
serve sports-eval/
```

> ⚠️ Do NOT open HTML files directly with `file://` — fonts and CDNs work,
> but some browser security policies may restrict localStorage. Use a local server.

---

## 🔐 Demo Login Credentials

| Email                     | Password | Sport      |
|---------------------------|----------|------------|
| alex@sportseval.com       | demo123  | Basketball |
| priya@sportseval.com      | demo123  | Badminton  |

You can also register a new account on the login page.

---

## 🌐 Hosting Options

### Option A: GitHub Pages (Free)
1. Create a GitHub repository
2. Upload the entire `sports-eval/` folder contents
3. Go to Settings → Pages → Source: main branch / root
4. Your site will be live at: `https://yourusername.github.io/sports-eval/`

```bash
git init
git add .
git commit -m "Initial commit - Sports Eval"
git remote add origin https://github.com/yourusername/sports-eval.git
git push -u origin main
```

### Option B: Netlify (Free, Drag & Drop)
1. Go to https://netlify.com
2. Drag the `sports-eval/` folder onto the Netlify dashboard
3. Your site is live instantly with a `.netlify.app` URL
4. Optional: Connect to GitHub for auto-deploys

### Option C: Vercel (Free)
```bash
npm install -g vercel
cd sports-eval/
vercel
# Follow prompts — deployed in ~30 seconds
```

### Option D: Firebase Hosting (Free tier)
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
# Set public directory to: . (current folder)
firebase deploy
```

---

## 🔗 Page Navigation Map

```
index.html (Login)
    │
    └──► main.html (Dashboard)
              ├──► training.html  (Training Plans)
              ├──► progress.html  (Charts & Analytics)
              ├──► profile.html   (User Profile)
              └──► about.html     (About / Public)
```

All authenticated pages call `SE.requireAuth()` on load, which redirects
to `index.html` if no user session exists. Navigation is handled by the
shared `buildNavbar()` function in `data.js`.

---

## 🔧 Customization

### Add a New Sport
In `js/data.js`:
```js
// 1. Add to SPORTS object
SPORTS.swimming = {
  name: 'Swimming',
  icon: 'fa-swimmer',
  color: '#00bcd4',
  colorClass: 'sport-swimming',
  metrics: ['Lap Speed', 'Stamina', 'Technique', 'Turn Speed', 'Breathing'],
};

// 2. Add to TRAINING_PLANS
TRAINING_PLANS.swimming = [ /* your plans */ ];

// 3. Add to PROGRESS_DATA
PROGRESS_DATA.swimming = { /* mock data */ };
```

Then in `css/global.css`, add:
```css
.sport-swimming { background: rgba(0,188,212,0.15); color: #00bcd4; border: 1px solid rgba(0,188,212,0.3); }
```

### Change Color Theme
Edit the CSS variables at the top of `css/global.css`:
```css
:root {
  --primary: #00c896;   /* main green accent */
  --accent:  #ff6b35;   /* orange accent */
  --dark:    #0a0f1e;   /* background */
}
```

---

## ⚡ Tech Notes

- **No backend required** — all data is stored in `localStorage`
- **Session persistence** — user data survives page reloads
- **Auth guard** — `SE.requireAuth()` on every protected page
- **Responsive** — Bootstrap grid + custom media queries
- **Charts** — Chart.js with dark theme configuration
- **Animations** — Pure CSS keyframes, no JavaScript animation libraries

---

## 📋 Browser Compatibility

| Browser          | Support |
|------------------|---------|
| Chrome 90+       | ✅ Full  |
| Firefox 88+      | ✅ Full  |
| Safari 14+       | ✅ Full  |
| Edge 90+         | ✅ Full  |
| Mobile Chrome    | ✅ Full  |
| Mobile Safari    | ✅ Full  |

---

*Built with ❤️ for athletes everywhere.*
