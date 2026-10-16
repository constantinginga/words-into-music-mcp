# Words into Music MCP

Connect an AI assistant to [Words into Music](https://wordsintomusic.com/mcp/) to generate songs from prompts or lyrics, check credits, and access your private song library.

This repository contains public connection metadata and documentation only. The hosted application implementation is private and is not included.

## Connect

Use a client that supports remote Streamable HTTP MCP servers with OAuth.

```text
https://mcp.wordsintomusic.com/mcp
```

For clients that read `mcp.json`, use the configuration in this repository. Complete OAuth sign-in in your browser. Never paste your password, session cookie, or a provider API key into a chat. No local package installation is needed.

[Setup guide](https://wordsintomusic.com/blog/mcp-setup-guide/) · [ChatGPT](https://wordsintomusic.com/make-songs-in-chatgpt/) · [Claude](https://wordsintomusic.com/make-songs-in-claude/) · [Grok](https://wordsintomusic.com/make-songs-in-grok/)

ChatGPT and Claude have completed real production song-generation tests. Other clients need their own acceptance test. The Grok guide describes its documented route; a Grok end-to-end test is pending.

## Tools

| Tool | Purpose | Song credits |
| --- | --- | --- |
| `get_account_usage` | Check available and reserved credits | 0 |
| `prepare_song` | Validate and save a song brief | 0 |
| `start_song` | Start a prepared recording | 1 |
| `get_song_job` | Check progress and get the finished link | 0 |
| `list_songs` | List your private songs | 0 |
| `get_song` | Read song details and optionally lyrics | 0 |

Prepare a song first and confirm its cost before starting generation. A free account has one song in total across the website and connected clients. Subscriptions and song packs share the same wallet and limits. See [pricing](https://wordsintomusic.com/pricing/).

Supported song languages are English, Spanish, German, French, and Brazilian Portuguese. Request vocals or an instrumental, with a target duration of 60 to 180 seconds. Accepted jobs continue after you close the chat. Find finished recordings in your private library.

## OAuth

The service supports OAuth discovery, dynamic client registration, PKCE, and refresh tokens. Scopes are `account:read`, `songs:read`, and `songs:create`. Review permissions before granting access. Disconnect clients in Words into Music Settings to revoke future tool calls.

The service does not issue manual personal access tokens. Unauthenticated requests to the MCP endpoint receive HTTP 401. This is expected.

## Support

Contact and security reports: contact@wordsintomusic.com.

[Privacy](https://wordsintomusic.com/privacy/) · [Terms](https://wordsintomusic.com/terms/)

The MIT license covers only this repository's public metadata and documentation. It excludes the hosted service implementation, trademarks, and logo.
