# Facehunch MCP

**Photo-search context with clear limits.**

Facehunch helps readers understand photo-search methods, source pages and the limits of a visual result. Its guides distinguish finding copies of a photograph from comparing faces across different photographs. The workspace presents a fictional sample and consented API tests; the current FaceCheck test index does not produce reliable identity matches.

[Website](https://facehunch.com) · [MCP repository](https://github.com/facehunch/mcp-server) · [Agent skill](https://github.com/facehunch/agent-skill) · [npm package](https://www.npmjs.com/package/facehunch-mcp)

## What this connector does

This package connects a local stdio MCP client to the hosted [Facehunch MCP server](https://mcp.facehunch.com/mcp). Hosted tools run on Cloudflare; the local package bridges the connection and opens browser-based OAuth. You do not need to deploy a Worker or paste a product password into your assistant.

| Tool | What it does |
| --- | --- |
| `get_profile` | Read the signed-in account context. |
| `list_reports` | List reports already owned by the account, with status and access metadata. |
| `get_report` | Retrieve an owned report’s metadata and available stored source links while respecting locked results. |

The MCP does not initiate new face searches, identify people, infer sensitive traits or research personal information. It only retrieves existing owned reports. Locked results remain hidden. An empty report list means the account has no saved reports, not that a photograph has no online matches. Test results are not identity verification.

## Example workflow

1. Locate a report that already exists in the user’s account.
2. Read its status and access state; preserve dates and source URLs exactly as returned.
3. Summarize what the stored report contains without inferring identity or treating a similarity score as verification.

### Things to ask your assistant

> List my saved reports with their dates, statuses and links.

> Open this existing report and summarize its available source links without identifying anyone.

> Check whether this report is accessible. If it is locked, return its product link instead of guessing its contents.

## Connect a remote MCP client

1. Open the client’s custom MCP or connector settings.
2. Enter `https://mcp.facehunch.com/mcp` as the remote server URL.
3. Complete Facehunch sign-in in your browser and review the permissions on the consent screen.
4. Return to the client and load the available tools.

Use a client that supports Streamable HTTP MCP and OAuth. Custom-connector availability depends on the client and your account. A public repository or npm release does not mean the integration has been approved for a client’s marketplace.

## Claude Desktop and other stdio clients

Requires **Node.js 22 or newer** and an existing Facehunch account. Add this entry to your client’s MCP configuration:

```json
{
  "mcpServers": {
    "facehunch": {
      "command": "npx",
      "args": [
        "-y",
        "facehunch-mcp"
      ]
    }
  }
}
```

You can also run `npx -y facehunch-mcp` from a terminal to start the bridge. It speaks MCP over stdio; it is not an interactive chat interface. For desktop clients that support MCPB extensions, download the `.mcpb` file from [Facehunch releases](https://github.com/facehunch/mcp-server/releases).

## Permissions and account access

Requested scopes: `profile:read reports:read`. Older profile-only connections need to reconnect and explicitly approve the additional permissions before content tools are available.

Only approve a connection you intended to start. If sign-in opens a new tab, finish it, return to the consent screen and refresh. [Manage or revoke connected apps](https://facehunch.com/oauth/mcp/connections).

The package uses pinned [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) for OAuth, PKCE and local token storage. Tokens on your computer are credentials. The connector uses the fixed endpoint above and rejects command-line endpoint overrides. Product passwords and underlying provider credentials are not requested by this package.

## Troubleshooting

- **No tools or insufficient permissions:** reconnect through browser consent and check the selected account.
- **An empty list:** confirm that the account owns the expected items. Empty results are different from a failed request.
- **A link asks you to sign in:** open it with the owning product account; a private product link is not a public share link.
- **The browser blocks authorization:** inspect the browser’s displayed error and restart an expired request from the client. Never send cookies or tokens in an issue.

## Add the companion skill

The [Facehunch agent skill](https://github.com/facehunch/agent-skill) explains how to select the right records, interpret results and respect the workflow’s limits:

```sh
npx skills add facehunch/agent-skill
```

## Learn more about Facehunch

- [Photo search and its limits](https://facehunch.com/)
- [Reverse image search explained](https://facehunch.com/reverse-image-search)
- [Face search versus reverse image search](https://facehunch.com/blog/face-search-vs-reverse-image-search)
- [Photo-search privacy](https://facehunch.com/blog/photo-search-privacy)
- [Test-mode help centre](https://facehunch.com/help-center)
- [Photo removal](https://facehunch.com/data-removal)

## Development and support

```sh
npm ci
npm test
npm run bundle
```

[Report a connector issue](https://github.com/facehunch/mcp-server/issues) with your client, Node.js version and a redacted error. Keep `package.json`, `manifest.json`, `server/config.json`, `server.json` and the lockfile version aligned for releases. GitHub Actions publishes versioned npm packages and MCPB assets. See [LICENSE](https://github.com/facehunch/mcp-server/blob/main/LICENSE) for the MIT license.
