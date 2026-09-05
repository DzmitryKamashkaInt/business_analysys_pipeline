---
description: Run the full call-transcript pipeline (clean → analyze → verify → write minutes & summary → verify again) for one transcript.
---

Run the call-transcript pipeline end to end for the transcript given in `$ARGUMENTS`
(a file path to a raw transcript, plus optional metadata like a call date/title if the user
supplied one).

Storage root for this pipeline is local, OneDrive-synced storage, separate from this repo's
`projects/` (which is reserved for the `/ba-*` business-analysis pipeline):

```
C:\Users\d.komashko\OneDrive - Intetics Inc\AgenticStorage\Transcript analysis\<call-slug>\
```

`<call-slug>` = `<YYYY-MM-DD>_<short-kebab-title>`, derived from the call date (ask the user
if not inferable from the transcript/filename) and a short title (from the transcript
subject, or ask).

## Steps

1. **Set up the project folder.** Create:
   ```
   <call-slug>/
     00-source/transcript-raw.<ext>   ← exact copy of the input transcript, untouched
     00-source/metadata.md            ← date, participants (if known), duration, source filename
     working/
     verification/
     final/
   ```

2. **Clean.** Invoke `call-transcript-cleaner` on `00-source/transcript-raw.<ext>` →
   `working/01-cleaned-transcript.md`.

3. **Verify cleaning.** Invoke `call-fidelity-verifier` with checkpoint `cleaning`, comparing
   `working/01-cleaned-transcript.md` against `00-source/transcript-raw.<ext>` → write the
   report to `verification/checkpoint-1-cleaning.md`.
   - If FAIL: re-invoke `call-transcript-cleaner` with the specific issues from the report,
     then re-verify. Max 2 rework rounds.
   - If still FAIL after 2 rounds: stop and show the user the unresolved issues — do not
     proceed to analysis on an unverified cleaning pass.

4. **Analyze.** Invoke `call-analyzer` on `working/01-cleaned-transcript.md` →
   `working/02-structured-analysis.md`.

5. **Verify analysis.** Invoke `call-fidelity-verifier` with checkpoint `analysis`, comparing
   `working/02-structured-analysis.md` against `00-source/transcript-raw.<ext>` (the raw
   source, not the cleaned one) → `verification/checkpoint-2-analysis.md`.
   - Same rework loop as step 3 (max 2 rounds, route feedback to `call-analyzer`).

6. **Generate documents.** Once analysis passes, invoke `call-minutes-writer` and
   `call-summary-writer` (these can run concurrently — they don't depend on each other) on
   `working/02-structured-analysis.md` → `final/meeting-minutes.md` and
   `final/call-summary.md`.

7. **Verify final documents.** Invoke `call-fidelity-verifier` with checkpoint `final-docs`
   twice — once per document — each time comparing against `00-source/transcript-raw.<ext>`
   (always the raw source) → `verification/checkpoint-3-minutes.md` and
   `verification/checkpoint-3-summary.md`.
   - Same rework loop (max 2 rounds each, route feedback to the relevant writer agent).

8. **Report back to the user**: paths to the two final documents, and a one-line status of
   each verification checkpoint (pass on first try / passed after rework / unresolved —
   flagged for manual review).

## Notes

- Every fidelity check in this pipeline compares against the ORIGINAL raw transcript, never
  against an intermediate artifact — this is what catches drift accumulating across stages.
- Don't skip a verification step even if an artifact "looks obviously fine" — the whole
  point of this pipeline is that nothing ships without being checked against source.
- If the user has not authorized a git remote/push for this OneDrive folder, don't push
  anywhere — this pipeline's output lives in the local OneDrive-synced folder only, which
  syncs to SharePoint on its own.
