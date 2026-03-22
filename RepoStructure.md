# SOCIO Repository Structure

This repository is the **parent integration repo** for three independent projects:

- `socioapp` → mobile/web app project (submodule)
- `sociogated` → gated/visitor management project (submodule)
- `socioweb` → web + server project (submodule)

The parent repo does **not** duplicate child code history. It tracks each child repo by commit pointer using Git submodules.

---

## Why this setup is different

Unlike a monorepo, SOCIO keeps each project as its own repository while still giving one top-level workspace.

- Child repos keep independent commit history, issues, and release cadence.
- Parent repo tracks which exact commit of each child should be used together.
- Cross-project coordination happens in one place without merging codebases.

---

## Current layout

```text
SOCIO/
  .gitmodules            # submodule mapping and URLs
  .gitignore             # parent-level ignores only
  RepoStructure.md       # this file
  socioapp/              # submodule -> sociomobilev2
  sociogated/            # submodule -> universitygatedv2
  socioweb/              # submodule -> socio2026v2
```

---

## Initial clone and setup

Clone with submodules:

```bash
git clone --recurse-submodules https://github.com/thesachinyyadav/SOCIO.git
```

If already cloned without submodules:

```bash
git submodule update --init --recursive
```

---

## How updates flow (important)

When you change code in a child repo (example: `sociogated`), do this:

1. Commit and push in child repo:
   ```bash
   git -C sociogated add .
   git -C sociogated commit -m "<message>"
   git -C sociogated push
   ```
2. Commit updated pointer in parent repo:
   ```bash
   git add sociogated
   git commit -m "chore: bump sociogated submodule pointer"
   git push
   ```

If step 2 is skipped, SOCIO will still point to the old child commit.

---

## Team workflow rules

- Open feature branches in the child repo for code changes.
- Use parent repo commits only for:
  - submodule pointer bumps
  - parent documentation
  - integration-level files/scripts
- Keep `.gitmodules` URLs current if repos are renamed or transferred.

---

## Useful commands

Check tracked submodule commits:

```bash
git submodule status
```

Pull latest pointers from parent branch:

```bash
git pull
git submodule update --init --recursive
```

Update child repos to latest remote (manual flow):

```bash
git -C socioapp pull
git -C sociogated pull
git -C socioweb pull
```

Then commit any pointer updates in SOCIO if required.
