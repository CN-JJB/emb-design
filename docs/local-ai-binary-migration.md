# Local AI task: migrate remaining binary assets

Use the exact file manifest at:

`docs/local-ai-binary-manifest.yaml`

Repositories:

- source: `CN-JJB/embbed-projects`
- target: `CN-JJB/emb-design`

## Required work

1. Make sure both repositories are available locally.
2. In `emb-design`, run `git pull --ff-only` before modifying anything.
3. Copy every source -> target mapping listed in the manifest exactly.
4. Preserve binary bytes; do not re-encode, optimize, rename, convert, or recompress files.
5. Do **not** migrate the KiCad official public mirror, project workspaces, tools, or test libraries.
6. For every copied file, verify source and target SHA-256 are identical.
7. Update `index/kicad-assets.yaml` for the six private 3D models:
   - change `status: source-only` to `status: ready`;
   - add the actual target path if the entry does not already have one;
   - preserve old_path, provenance, and existing source information.
8. Update `docs/migration-2026-08-30.md` to record that the binary migration was completed locally, with file count and commit SHA.
9. Run `git status --short` and make sure only expected files are included.
10. Commit and push to `main`.

Suggested commit message:

`Migrate remaining device, material, and KiCad binary assets`

## Return exactly

- copied file count by group;
- SHA-256 verification result;
- files skipped or failed;
- commit SHA;
- push result;
- unresolved issues.

If any target file already exists with different content, do not overwrite it silently. Report it as a conflict.
