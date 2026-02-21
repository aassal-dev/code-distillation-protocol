# GetRepo — Code Distillation Protocol (Knowledge Extraction)

How to get and work with this repository: clone, branch model, and version workflow.

---

## Clone

```bash
git clone https://github.com/aassal-dev/code-distillation-protocol.git
cd code-distillation-protocol
```

---

## Branch and folder model

- **main**
  - Contains **Planning/** (plans, resources, version plans). Planning is always part of main.
  - After merges, main also contains released version folders (e.g. **V0.2.05/**, **V0.2.06/**).

- **One folder per version**
  - Each release lives in its own folder: **V0.2.05/**, **V0.2.06/**, etc.
  - Use everything inside that folder together; do not mix files from different version folders.

- **One branch per version**
  - Each version has a dedicated branch: **v0.2.05**, **v0.2.06**, etc.
  - That branch holds **Planning/** (from main) plus the contents of **V0.2.xx/** for that release.

---

## Workflow: new version (merge branch)

For each new version (e.g. v0.2.07):

1. **Start from main**
   - Ensure main is up to date and includes Planning (and any prior merged version folders).
   - `git checkout main && git pull origin main`

2. **Create the version merge branch**
   - `git checkout -b v0.2.xx`
   - Add or update the version folder **V0.2.xx/** (workflow, steps, checklist, templates, prompts, etc.).

3. **Commit and push the version branch**
   - `git add V0.2.xx/` (and only that folder if you keep versions isolated).
   - `git add Planning/` only if you have new Planning changes to include on this branch.
   - `git commit -m "Release v0.2.xx"`
   - `git push -u origin v0.2.xx`

4. **Merge to main when the release is ready**
   - Open a PR from **v0.2.xx** into **main**, or merge locally and push main.
   - main will then contain Planning plus the new **V0.2.xx/** folder.

**Summary:** Every version has its own folder and its own branch. Planning lives on main; each version branch is created from main, adds/updates that version’s folder, is pushed, and is then merged into main.

---

## Existing version branches

After cloning, list and use version branches as needed:

```bash
git branch -a
git checkout v0.2.06   # use v0.2.06
git checkout v0.2.05   # use v0.2.05
git checkout main      # Planning + all merged versions
```

---

## Reference

- **CHANGELOG.md** — Version history and themes.
- **V0.2.xx/README.md** — What’s in that version and how to run it.
