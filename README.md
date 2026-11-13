# Words into Music MCP

Connect an AI assistant to [Words into Music](https://wordsintomusic.com/mcp/) to generate songs from prompts or lyrics, check credits, and access your private song library.

This repository contains public connection metadata and documentation only. The hosted application implementation is private and is not included.

## Connect

Use a client that supports remote Streamable HTTP MCP servers with OAuth.

```text
https://mcp.wordsintomusic.com/mcp
```

For clients that read `mcp.json`, use the configuration in this repository (Agent Plugins format; most clients only need the `url`). Complete OAuth sign-in in your browser. Never paste your password, session cookie, or a provider API key into a chat. No local package installation is needed.

[Setup guide](https://wordsintomusic.com/blog/mcp-setup-guide/) · [ChatGPT](https://wordsintomusic.com/make-songs-in-chatgpt/) · [Claude](https://wordsintomusic.com/make-songs-in-claude/) · [Grok](https://wordsintomusic.com/make-songs-in-grok/)

ChatGPT and Claude have completed real production song-generation tests. Other clients need their own acceptance test. The Grok guide describes its documented route; a Grok end-to-end test is pending.

## Gemini CLI

```sh
gemini extensions install https://github.com/constantinginga/words-into-music-mcp
```

In Gemini CLI, run `/mcp auth words-into-music` and complete OAuth sign-in. Then check `/mcp` for the available tools. The extension manifest and installation have been validated with Gemini CLI 0.40.1. Authenticated Gemini song generation has not completed an acceptance test yet.

## Agent skill

[`skills/words-into-music`](skills/words-into-music/SKILL.md) teaches an agent the safe flow: check credits, prepare the brief, confirm the one-credit cost, start, poll every 15 seconds and return the private song link. Install it with:

```sh
npx skills add constantinginga/words-into-music-mcp
```

The repository is also a portable agent plugin (`plugin.json`, `mcp.json`, `skills/`). In Hermes Agent, install it with `hermes plugins install words-into-music` and complete the OAuth sign-in it starts.

## What you can do

- Write and create a song from an idea, or have your own lyrics sung, in any of 11 languages
- Choose the style, mood, instruments and vocals, or make an instrumental from 60 to 180 seconds long
- Check your song credits before you start
- Follow a song while it generates and get the link to the finished song
- Find songs in your private library and read their lyrics
- Write lyrics, song titles or band and stage names with the free writing tools, which don't use song credits

Example requests:

- "Write an upbeat birthday song for my sister Ana in Spanish, acoustic pop with warm vocals."
- "Turn these lyrics into a slow piano ballad: ..."
- "Make a 60-second lo-fi instrumental for studying."
- "How many song credits do I have left?"
- "Show my last five songs."

## Tools

| Tool | Purpose | Song credits |
| --- | --- | --- |
| `get_account_usage` | Check available and reserved credits | 0 |
| `prepare_song` | Validate and save a song brief | 0 |
| `start_song` | Start a prepared song | 1 |
| `get_song_job` | Check progress and get the finished link | 0 |
| `list_songs` | List your private songs | 0 |
| `get_song` | Read song details and optionally lyrics | 0 |
| `write_text` | Write lyrics, song titles or names with the free generators | 0 (daily writing allowance) |
| `create_checkout_link` | Open a Stripe checkout for a plan or song pack the user chose (not offered in ChatGPT) | 0 |

Prepare a song first and confirm its cost before starting it. Starting the same preparation again returns the same job, so a retry never spends a second credit. Song credits come from monthly or yearly plans and one-time song packs, which share one wallet. See [pricing](https://wordsintomusic.com/pricing/).

Supported song languages are English, Spanish, German, French, Brazilian Portuguese, Italian, Dutch, Polish, Swedish, Japanese and Korean. Songs usually take a few minutes and keep generating after you close the chat. Finished songs are in your private library.

## OAuth

The service supports OAuth discovery, dynamic client registration, PKCE, and refresh tokens. Scopes are `account:read`, `songs:read`, and `songs:create`. Review permissions before granting access. Disconnect clients in Words into Music Settings to revoke future tool calls.

The MCP endpoint uses OAuth only. Unauthenticated requests to it receive HTTP 401, which is expected. For scripts, a REST API with API keys created in Settings is documented at [wordsintomusic.com/developers](https://wordsintomusic.com/developers/).

## Support

Contact and security reports: contact@wordsintomusic.com.

[Privacy](https://wordsintomusic.com/privacy/) · [Terms](https://wordsintomusic.com/terms/)

The MIT license covers only this repository's public metadata and documentation. It excludes the hosted service implementation, trademarks, and logo.
