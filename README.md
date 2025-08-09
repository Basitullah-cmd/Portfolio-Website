# Portfolio-Website
A responsive personal portfolio built with HTML, CSS, and vanilla JavaScript to showcase my skills and projects.
Concept & goals
A single-page (or small multi-page) portfolio that:

Showcases your skills (tech stack, tools, soft skills).

Highlights projects with live links, screenshots, tech tags, short case studies.

Has a clear call-to-action (hire/contact, GitHub, resume download).

Is fast, mobile-first, accessible, and easy to update.

Uses only HTML, CSS, and vanilla JS — no frameworks.

High-level layout / sections
(Usually a single-page scroll layout with anchor nav)

Header / Nav

Logo / name (left), nav links (right). On mobile: hamburger menu.

Optional: theme toggle (light/dark) and CTA button ("Download Resume", "Contact").

Hero

Short intro: name, role (e.g., “Computer Engineering student — Frontend & IoT enthusiast”), one-line value proposition.

CTA buttons: “View Projects”, “Contact”.

Small photo or illustrated avatar / animated code-art.

About

Short bio, interests, education.

Stats/mini cards (Years coding, Projects, GitHub stars/ repos).

Skills

Categorized (Frontend, Backend, Tools, Languages).

Skill bars or chips; accessible progress meter or visually appealing icons.

Projects / Case Studies (main focus)

Grid of project cards: image, title, short description, tech tags, buttons (Demo / Code / Case study).

Click → modal or dedicated project page with more details: problem, solution, tech used, challenges, screenshots, role, repo link.

Experience / Education (optional)

Timeline list or cards.

Contact

Simple contact form + email/copy button. Social links (GitHub, LinkedIn, Twitter).

Option: Calendly link or email mailto.

Footer

Copyright, small nav, resume download, privacy link.

Responsive behavior & breakpoints
Mobile-first CSS. Start layout optimized for narrow screens.

Breakpoints (suggested):

up to 600px — mobile single-column

600–900px — tablet, two-column where helpful

900px+ — desktop grid, multi-column hero

Use CSS Grid for projects, Flexbox for nav/rows. Example: grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));

Accessibility & best practices
Semantic HTML: <header>, <main>, <section>, <nav>, <footer>, <form>.

Keyboard focus states, visible outlines for interactive elements.

Alt text for images/screenshots.

aria-* for modals and the hamburger menu (e.g., aria-expanded).

Form labels, role="alert" for submission messages.

Color contrast >= 4.5:1 for text.

Lazy-load images: loading="lazy".

Prefers-reduced-motion support: respect @media (prefers-reduced-motion: reduce).

Performance & SEO
Use <meta> tags: description, og:title, og:image, twitter card.

Minimize render-blocking CSS (inline critical CSS for hero if you want top performance).

Optimize images (webp if possible), compress screenshots.

Use link rel="preload" for hero image if large.

Add structured data (JSON-LD) for person/schema.org to help search engines.

Use readable URLs for project pages if multi-page.

Progressive features (vanilla JS)
Mobile nav: hamburger toggle with aria updates.

Theme toggle: store preference in localStorage and apply data-theme on <html>.

Project filter: filter projects by tag (HTML + JS to show/hide or re-render grid).

Modal project detail: accessible modal with focus trap.

Contact form: fetch to an email endpoint (Formspree, Netlify Forms, or a small server endpoint). Provide a fallback mailto link.

File structure (suggested)
vbnet
Copy
Edit
portfolio/
├─ index.html
├─ projects/
│  ├─ project-1.html   (optional)
│  └─ project-2.html
├─ css/
│  ├─ base.css
│  ├─ layout.css
│  └─ components.css
├─ js/
│  ├─ nav.js
│  ├─ theme.js
│  └─ projects.js
├─ assets/
│  ├─ images/
│  └─ icons/
├─ resume.pdf
└─ README.md
Sample snippets
Mobile nav + theme toggle (minimal):

html
Copy
Edit
<!-- index.html header -->
<header class="site-header">
  <a class="logo" href="#home">Basit Khan</a>
  <button id="navToggle" aria-label="Open menu" aria-expanded="false">☰</button>
  <nav id="mainNav" hidden>
    <a href="#projects">Projects</a>
    <a href="#skills">Skills</a>
    <a href="#contact">Contact</a>
    <button id="themeToggle" aria-pressed="false">🌙</button>
  </nav>
</header>
css
Copy
Edit
/* base.css (mobile-first) */
.site-header{display:flex;align-items:center;justify-content:space-between;padding:1rem;}
#mainNav{display:flex;gap:1rem;}
@media (max-width:700px){
  #mainNav{flex-direction:column;position:absolute;top:64px;right:1rem;background:#fff;padding:1rem;border-radius:8px;box-shadow:var(--shadow);}
}
js
Copy
Edit
// js/nav.js
const navToggle = document.getElementById('navToggle');
const mainNav = document.getElementById('mainNav');

navToggle.addEventListener('click', () => {
  const expanded = navToggle.getAttribute('aria-expanded') === 'true';
  navToggle.setAttribute('aria-expanded', String(!expanded));
  if(expanded) mainNav.hidden = true; else mainNav.hidden = false;
});
Theme toggle (light/dark):

js
Copy
Edit
// js/theme.js
const themeToggle = document.getElementById('themeToggle');
const saved = localStorage.getItem('theme') || (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
document.documentElement.setAttribute('data-theme', saved);
themeToggle.addEventListener('click', () => {
  const current = document.documentElement.getAttribute('data-theme');
  const next = current === 'dark' ? 'light' : 'dark';
  document.documentElement.setAttribute('data-theme', next);
  localStorage.setItem('theme', next);
});
Project card grid (HTML snippet):

html
Copy
Edit
<section id="projects" class="projects">
  <div class="filters">
    <button data-filter="all">All</button>
    <button data-filter="web">Web</button>
    <button data-filter="iot">IoT</button>
  </div>
  <div class="project-grid">
    <article class="project-card" data-tags="web,js" >
      <img src="assets/images/project1.png" alt="Screenshot of project 1" loading="lazy">
      <h3>Gesture Drone UI</h3>
      <p>Control a drone with hand gestures — UI, telemetry, and visualization.</p>
      <div class="tags"><span>HTML</span><span>JS</span><span>WebSocket</span></div>
      <div class="actions"><a href="/projects/project-1.html">Details</a><a href="https://github.com/you/project1">Source</a></div>
    </article>
    <!-- more cards -->
  </div>
</section>
Project filtering (vanilla JS idea):

js
Copy
Edit
// js/projects.js
document.querySelectorAll('.filters button').forEach(btn=>{
  btn.addEventListener('click', ()=>{
    const f = btn.dataset.filter;
    document.querySelectorAll('.project-card').forEach(card=>{
      const tags = card.dataset.tags.split(',');
      const show = f === 'all' || tags.includes(f);
      card.style.display = show ? '' : 'none';
    });
  });
});
Design & polish ideas
Use a consistent color palette (2 primary colors + neutral); use CSS variables for theming.

Micro-interactions: subtle card hover lift, button ripple/transition.

Terminal-style project demo for coding projects (small faux terminal animation).

Project case study: challenge → approach → outcome (metrics where possible).

Deployment & hosting
Simple options: GitHub Pages, Netlify, Vercel. All serve static HTML/CSS/JS.

Add CI: automatic deploy on push to main.

Connect domain (e.g., basit.dev or basitkh.tech). Use HTTPS (automatic on Netlify / GitHub Pages).

Analytics & contact handling
Lightweight analytics (Plausible) or simple Google Analytics consent.

Contact form via Formspree/Netlify forms/EmailJS. Also show direct email and GitHub link.

Testing checklist before launch
 Mobile nav works and is keyboard accessible.

 All images optimized + loading="lazy".

 Lighthouse score: aim >= 90 for Performance, Accessibility, Best Practices.

 SEO meta tags & JSON-LD added.

 Contact form submissions verified.

 Resume PDF link downloads correctly.

 Theme saved across reloads.

Example content plan for projects (what to write for each)
Title + short blurb (1–2 lines).

Tech stack (tags).

Demo link / GitHub link.

Role (solo / team).

Problem statement + approach (3–5 bullets).

Key screenshots / gifs — show functionality.

Optional: short video walkthrough.
