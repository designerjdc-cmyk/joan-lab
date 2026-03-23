# joan.lab

Personal prototype lab — all experiments organized by project with versioned prototypes.

## Quick start

```bash
git clone https://github.com/YOUR_USER/joan-lab.git
cd joan-lab
# Open index.html in browser, or deploy to GitHub Pages
```

## Deploy to GitHub Pages

1. Push this repo to GitHub
2. Go to **Settings → Pages**
3. Source: **Deploy from a branch**
4. Branch: `main` / `/ (root)`
5. Save — your lab will be live at `https://YOUR_USER.github.io/joan-lab/`

## How it works

```
joan-lab/
├── index.html          ← dashboard (reads manifest.json)
├── manifest.json       ← source of truth for all prototypes
├── projects/
│   ├── morphika/
│   │   ├── review-v01/
│   │   │   └── index.html
│   │   └── review-v02/
│   │       └── index.html
│   ├── growsmith/
│   │   ├── dashboard-v03/
│   │   │   └── index.html
│   │   └── sorteos-v01/
│   │       └── index.html
│   ├── torre-atangia/
│   │   └── iot-dashboard-v01/
│   │       └── index.html
│   └── darksmith/
│       └── scraper-v01/
│           └── index.html
└── README.md
```

The dashboard (`index.html`) fetches `manifest.json` at runtime and renders everything. Zero build step, pure vanilla HTML/CSS/JS.

## Adding a new prototype

### 1. Create the folder

```bash
mkdir -p projects/growsmith/audience-v01
```

### 2. Drop the HTML file

Save the prototype Claude gives you as `index.html` inside that folder.

If it's a React `.jsx` artifact from Claude, you have two options:
- **Quick**: Copy the rendered HTML from the artifact's "View source" and save as `index.html`
- **Standalone**: Wrap it in a CDN-powered HTML shell (template below)

### 3. Update manifest.json

Add an entry to the project's `prototypes` array:

```json
{
  "id": "audience-v01",
  "name": "Audience Segments",
  "version": "v0.1",
  "date": "2025-03-23",
  "status": "concept",
  "description": "Active/ghost/external follower segmentation",
  "tags": ["audience", "segmentation", "data"],
  "path": "projects/growsmith/audience-v01/index.html"
}
```

### 4. Push

```bash
git add .
git commit -m "add: growsmith/audience-v01"
git push
```

GitHub Pages updates automatically in ~60 seconds.

## Adding a new project

Add a new object to the `projects` array in `manifest.json`:

```json
{
  "id": "new-project",
  "name": "New Project",
  "color": "#FF5A8A",
  "description": "What this project is about",
  "prototypes": []
}
```

Then start adding prototypes as described above.

## Status values

| Status     | Meaning                                    |
|------------|--------------------------------------------|
| `active`   | Current working prototype                  |
| `wip`      | Work in progress, not fully functional     |
| `concept`  | Idea stage, placeholder or early sketch    |
| `archived` | Superseded or abandoned, kept for reference|

## React prototype shell template

If Claude gives you a `.jsx` component, wrap it in this HTML to make it standalone:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Prototype Name</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.9/babel.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
    // PASTE YOUR REACT COMPONENT HERE

    ReactDOM.createRoot(document.getElementById('root')).render(<YourComponent />);
  </script>
</body>
</html>
```

## Tips

- **Naming convention**: `kebab-case-vXX` for folder names (e.g. `review-v02`, `sorteos-v01`)
- **Assets**: Put images, CSS, JS alongside the `index.html` in the same folder
- **Multi-file prototypes**: Everything inside a prototype folder is self-contained
- **Local preview**: Just open `index.html` in your browser — no server needed (uses `fetch` for manifest, so use a local server if you hit CORS: `npx serve .`)
