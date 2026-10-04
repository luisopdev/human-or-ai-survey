# Human or AI? – Music Survey

A single-page survey site with no build step and no dependencies. Open `index.html` in any browser to try it.

## Demo mode (now)

`CONFIG.submitUrl` is empty, so:

- the clips are simulated (no audio needed), and
- answers aren't saved. They're only printed to the browser console.

This is enough to show the teacher how the survey works.

## Make it real

1. **Wording.** Edit the `TEXT` object in `index.html`. Its keys match the numbers in the "Survey Screen Text" doc (1.1, 2.3 …).
2. **Clips.** Put the MP3s in a `clips/` folder next to `index.html`. Set each clip's `src`, for example `{ id: 'C1', src: 'clips/c1.mp3' }`. The real duration is read from the file.
   Keep the correct answer (human/AI) out of this file. Store it in the database so participants can't find it in the page source.
3. **Saving answers.** Set `CONFIG.submitUrl` (for example `/api/responses`). The page sends one POST per participant:

```json
{
  "responseId": "uuid",
  "startedAt": "2026-10-04T15:02:11.000Z",
  "submittedAt": "2026-10-04T15:06:40.000Z",
  "musicLevel": "basic | intermediate | advanced",
  "listeningSetup": "headphones | speaker_quiet | speaker_noisy",
  "clips": [
    { "clipId": "C3", "position": 1, "answer": "human | ai", "confidence": "guessing | fairly_sure | very_sure | null", "plays": 2 }
  ]
}
```

   Any 2xx response shows the thank-you screen. Anything else shows a "try again" message.

## Suggested hosting

Azure Static Web Apps: `index.html` and `clips/` as the site, plus a Node.js managed Function at `api/responses` that inserts into the database.

Suggested tables (PostgreSQL or Azure SQL):

- `clips(clip_id PK, file_name, made_by ['human'|'ai'], source_credit)`
- `responses(response_id PK, started_at, submitted_at, music_level, listening_setup)`
- `answers(response_id FK, clip_id FK, position, answer, confidence, plays)`

Don't store names, emails or IP addresses, so the survey stays anonymous.
# human-or-ai-survey
AP Research - Human or AI survey
