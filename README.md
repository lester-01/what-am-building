# What I am building

**Live site: [projects.benlester.me](https://projects.benlester.me)**

A public board of the projects I build, from early ideas to live products. I am [Ben Lester](https://benlester.me), a software and systems engineer, and this is the short list of work I am willing to show.

## What it shows

- Each project has a status, such as idea, building or live.
- Public projects link to their repository. Private projects show a name and a short summary only. Their code is not shown.
- Where a project has its own website, that link is shown too.
- You can filter by visibility, status and tag, or search by text.

## How it is made

I keep a small `project.json` file in each of my repositories. It holds the status, tags, languages and whether the project may appear here.

A private repository (my command center) runs a GitHub Actions job every day. It reads those files, builds a list of the projects I chose to share, and publishes it to this repository as `data/projects.json` and `data/status.json`. `index.html` reads those files. There is no build step and no framework.

## Do not edit by hand

`data/` and `index.html` are written by the job. Changes made here are overwritten on the next run.

## Run it locally

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000.
