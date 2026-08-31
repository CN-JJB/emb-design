# Local AI task: restore non-UTF8 / binary-like source files

Use `docs/local-ai-nonutf8-manifest.yaml` exactly.

## Goal

Restore 14 source files from `CN-JJB/embbed-projects` into `CN-JJB/emb-design`.

These files were deliberately **not** copied by Web AI because GitHub's text connector cannot decode them safely as UTF-8. Some C/H files appear to use a legacy Chinese encoding; the EDA `.epro` file is binary/packed.

## Rules

1. In target repo, run `git pull --ff-only`.
2. For every manifest item, read source bytes from the source Git object, not through a text editor. Preferred form:
   `git -C <source-repo> cat-file blob HEAD:"<relative source path>" > "<target absolute path>"`
3. Do **not** transcode encodings.
4. Do **not** normalize CRLF/LF.
5. Do **not** open-and-save through an IDE before verification.
6. The target paths in the manifest are intentional:
   - ICM45686 old `driver/` and `examples/` content is normalized under `software/`;
   - the Skystar `.epro` remains under `references/eda/`.
7. After copying each file:
   - size must equal manifest `size`;
   - `git hash-object "<target>"` must equal manifest `git_blob_sha`.
8. Stage with line-ending conversion disabled (for example `git -c core.autocrlf=false add -- <paths>`).
9. Verify the **staged** blob with `git ls-files -s`; staged blob SHA must still equal manifest `git_blob_sha`.
10. If any staged SHA changes, stop and report; do not commit a transformed copy.
11. Do not change any other repository structure or facts.

## Commit

Suggested message:

`Restore non-UTF8 device source assets`

Push to `main`.

## Return

- 14/14 copied count;
- size verification count;
- staged Git blob SHA verification count;
- commit SHA;
- push result;
- any conflict or encoding observation.
