## Adapt Authoring 0.11.5 – Patch Collection

This repository contains **local patches for the Adapt Authoring Tool 0.11.5**.  
They are intended for people maintaining existing 0.11.x installations now that the project is largely unmaintained.

### Contents

- **`0.11.5/attribution.patch`**  
  Adds richer asset attribution metadata (attribution text, source URL, licence) throughout the asset-management workflow, stores it in Mongo, exposes it in the output, and wires a small editor‑side helper so components can easily pull attribution text from a selected asset.

- **`0.11.5/projectsView.patch`**  
  Reworks the projects dashboard UI and filtering:
  - Replaces the “view tags / hide tags” toggle with a simpler always‑visible tag list in the project details.
  - Adds a dedicated tag filter area in the sidebar that shows tag names and usage counts.
  - Improves empty/loading states and moves tag selection into the sidebar instead of using sidebar “chips”.
  - Removes infinite scrolling on the projects dashboard and switches to single‑shot fetch + client‑side filtering/sorting.

- **`0.11.5/speed.patch`**  
  Optimises the course editor for large courses:
  - Adds a `/content/pageStructure/:id` REST endpoint that returns page, article, block and component data in a single request.
  - Caches page structure on the client and updates `ContentModel` / `ContentCollection` to use the cache before hitting the API.
  - Means that ContentObject pages now load a LOT faster

> **Important**  
> These patches are **only known to apply cleanly to Adapt Authoring Tool 0.11.5**.  
> If you are on a fork or a different version, expect to resolve conflicts manually.

---

## How to Apply the Patches

- **Step 1 – Put the patches in the Adapt Authoring root**  
  Copy the `0.11.5` folder (and this `README.md` if you like) into the **root of your Adapt Authoring 0.11.5 checkout** (the folder that contains `package.json`, `lib/`, `frontend/`, etc.).  
  You should then have paths like `./0.11.5/attribution.patch` inside your Adapt Authoring project.

- **Step 2 – Dry‑run each patch**  
  From the Adapt Authoring root, run a dry‑run for each patch to check it will apply cleanly:

  ```bash
  cd /path/to/adapt_authoring
  patch --dry-run -p1 < 0.11.5/attribution.patch
  patch --dry-run -p0 < 0.11.5/projectsView.patch
  patch --dry-run -p0 < 0.11.5/speed.patch
  ```

  If any dry‑run fails, your source tree likely differs from stock 0.11.5; you may need to fix conflicts manually.

- **Step 3 – Apply the patches for real**  
  Once the dry‑run succeeds, apply the patches:

  ```bash
  cd /path/to/adapt_authoring
  patch -p1 < 0.11.5/attribution.patch
  patch -p0 < 0.11.5/projectsView.patch
  patch -p0 < 0.11.5/speed.patch
  ```

- **Step 4 – Rebuild the authoring tool**  
  Still from the Adapt Authoring root, run a production build so the frontend picks up the changes:

  ```bash
  grunt build:prod
  ```

  Then restart the Adapt Authoring server and check that:

  - Asset attribution fields behave as expected.
  - The projects dashboard UI and tag filters work.
  - Editor pages (especially large ones) load noticeably faster.

