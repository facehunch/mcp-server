# Facehunch MCP

Explore where your photographs appear online through a consent-based face-search workflow.

get_profile reads only your own account ID and email. It does not identify other people, perform face searches, access photographs or search reports, or spend credits.

## Remote MCP

Use **https://facehunch.com/mcp** in a client that supports remote MCP with OAuth. Sign in to Facehunch and explicitly approve the connection.

## Claude Desktop and other stdio clients

Requires Node.js 22 or newer. Add this configuration:

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

Or install the `.mcpb` file from [Releases](https://github.com/facehunch/mcp-server/releases) in your desktop client's Extensions settings. The connector opens your browser for sign-in. If the consent page asks you to sign in, use its new-tab link, then return and refresh the consent page.

## Privacy and permissions

Your product password and provider credentials are not requested by this package. The pinned [mcp-remote](https://www.npmjs.com/package/mcp-remote) bridge handles OAuth, PKCE and local token storage. It connects only to the fixed endpoint above; command-line endpoint overrides are not supported. OAuth tokens are stored locally by mcp-remote and should be treated as credentials.

Revoke a connection at [Facehunch MCP connections](https://facehunch.com/oauth/mcp/connections).

## Development and publishing

Run `npm ci` and `npm test`. GitHub Actions publishes a new package version using the organization’s `NPM_TOKEN` secret, then builds and releases the desktop bundle. Keep package.json, manifest.json, server/config.json and server.json versions aligned.

Marketplace approval is separate from npm publication. See the product’s submission notes for endpoint tests and review prerequisites.

[Website](https://facehunch.com) · [Issues](https://github.com/facehunch/mcp-server/issues)
