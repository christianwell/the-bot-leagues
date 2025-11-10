# Teams — How to add teams & players

This document explains how to add teams and players to the static `teams.html` page and gives a recommended data-driven alternative.

## 1) Quick manual edit (small sites)

Open `teams.html`. Each team is represented by a `.team-section` element.

Example — add a new team with two members:

```html
<article class="team-section">
  <h2>New Team Name</h2>
  <div class="member-grid">
    <article class="profile-card">
      <img class="avatar" loading="lazy" src="/path/to/avatar.png" alt="Player One avatar">
      <h3>Player One</h3>
      <p class="meta">Role: Pilot</p>
      <p class="bio">Short bio about Player One.</p>
      <div class="card-actions">
        <a class="button" href="#">View</a>
      </div>
    </article>

    <article class="profile-card">
      <img class="avatar" loading="lazy" src="/path/to/avatar2.png" alt="Player Two avatar">
      <h3>Player Two</h3>
      <p class="meta">Role: Engineer</p>
      <p class="bio">Short bio about Player Two.</p>
      <div class="card-actions">
        <a class="button" href="#">View</a>
      </div>
    </article>
  </div>
</article>
```

Tips:
- Keep images reasonably sized (e.g. 88×88 or square thumbnails). Use `loading="lazy"` to avoid blocking page load.
- Put new team sections inside the `<main class="wrapper">` area.
- Use relative paths for images (`images/avatars/alice.png`) to keep repo portable.

## 2) Recommended — data-driven approach (scales better)

Instead of editing `teams.html` every time, store teams in a JSON file (e.g. `data/teams.json`) and render them with a tiny client-side script `scripts/teams.js`.

Example `data/teams.json`:

```json
[
  {
    "name": "Team Alpha",
    "members": [
      { "name": "Alice", "role": "Captain", "bio": "Loves PvP bots.", "avatar":"/images/alice.png" },
      { "name": "Dana", "role": "Support", "bio": "Works on testing.", "avatar":"/images/dana.png" }
    ]
  },
  {
    "name": "Redstone Raiders",
    "members": [ { "name": "Bob", "role":"Dev", "bio":"Path optimization.", "avatar":"/images/bob.png" } ]
  }
]
```

Client-side renderer (very small example to put in `scripts/teams.js`):

```js
fetch('/data/teams.json')
  .then(r => r.json())
  .then(teams => {
    const container = document.querySelector('.teams-list') || document.createElement('div');
    teams.forEach(team => {
      const section = document.createElement('article');
      section.className = 'team-section';
      section.innerHTML = `
        <h2>${team.name}</h2>
        <div class="member-grid">
          ${team.members.map(m => `
            <article class="profile-card">
              <img class="avatar" loading="lazy" src="${m.avatar}" alt="${m.name} avatar">
              <h3>${m.name}</h3>
              <p class="meta">Role: ${m.role}</p>
              <p class="bio">${m.bio}</p>
              <div class="card-actions"><a class="button" href="#">View</a></div>
            </article>`).join('')}
        </div>`;
      container.appendChild(section);
    });
  })
  .catch(e => console.error('Failed to load teams.json', e));
```

Notes:
- Put `data/teams.json` and `scripts/teams.js` in the repo, and add `<script src="/scripts/teams.js" defer></script>` to `teams.html`.
- This makes adding/removing teams a simple JSON edit and avoids merge conflicts.

## 3) Preview locally

Run a tiny local server from the project root:

```bash
python3 -m http.server 8000
# then open http://localhost:8000/teams.html
```

## 4) Accessibility & best practices

- Add descriptive `alt` text for avatars.
- Keep contrast high for text on dark backgrounds (adjust CSS variables in `styles.css`).
- Use semantic markup (`<article>`, `<section>`, `<nav>`, `<main>`).

If you'd like, I can implement the JSON-driven renderer and wire everything up now.
