# MCP tool reference

Endpoint: `https://mcp.wordsintomusic.com/mcp` (Streamable HTTP, OAuth). Read the live schemas from the server if an argument is rejected. Use the result's `structuredContent` or JSON text, and check `isError` first.

| Tool | Arguments | Result and credits |
| --- | --- | --- |
| `get_account_usage` | `{}` | Available and reserved credits, supported languages, account link. No credit. |
| `prepare_song` | `requestId`, `input`, `uiLocale` | `preparedRequestId`, normalized `input`, `songCreditCost: 1`, `expiresAt`. Saved for one hour. No credit. |
| `start_song` | `preparedRequestId` | `jobId`, `status`, `nextCheckAfterSeconds`. Uses one credit. The same preparation returns the same job. |
| `get_song_job` | `jobId` | Progress. `ready` includes `song` with a private `url`. No credit. |
| `list_songs` | optional `limit` 1–20 (default 10), optional `cursor` | The user's songs, newest first, with a `cursor` for the next page. No credit. |
| `get_song` | `id`, optional `includeLyrics` (default false) | One song the user owns and its private page URL. No credit. |
| `write_text` | `kind`, `idea` (5–2,000 characters), optional `direction`, `language`, `requestId` | Generated `text`. `kind` is `lyrics`, `rap`, `diss`, a genre lyric type, `titles`, `bands`, `stageNames`, `rapNames`, `albums`, `playlists` or `djNames`. Daily writing allowance, no song credit. |

Some clients also see a checkout tool. Use it only when the user explicitly asks to buy a plan or song pack and has chosen the offer; never buy anything on your own initiative.

## Song input

| Field | Value |
| --- | --- |
| `mode` | `idea` or `lyrics`, default `idea` |
| `idea` | Up to 2,000 characters, for idea mode |
| `lyrics` | Up to 4,000 characters, the user's own words, for lyrics mode |
| `direction` | Required musical direction, up to 400 characters |
| `language` | `en`, `es`, `de`, `fr`, `pt-BR`, `it`, `nl`, `pl`, `sv`, `ja` or `ko` |
| `instrumental` | Boolean, default false |
| `title` | Optional, up to 100 characters |
| `duration` | Whole seconds, 60–180, default 120 |

`uiLocale` sits outside `input`, takes the same codes and sets the language of the completion email.

## Example

```json
{
  "requestId": "3f2b9c1e-birthday-bicycle-0001",
  "input": {
    "mode": "idea",
    "idea": "A birthday song for a friend who fixes everyone's bicycles and never asks for thanks.",
    "direction": "Warm acoustic pop, gentle drums, clear storytelling vocals, 96 BPM",
    "language": "en",
    "title": "The Spare Bicycle",
    "duration": 120
  },
  "uiLocale": "en"
}
```

## States and errors

`queued`, `generating` and `saving` mean in progress. `ready` means done; use `song.url`. On `failed` or `needs-attention`, stop and report.

An OAuth error means the user must reconnect through the MCP client. Errors have the shape `{"error":{"code","message"}}`; show the message. An ownership or not-found error never justifies trying other IDs.
