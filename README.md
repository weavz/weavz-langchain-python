# LangChain + Weavz MCP

Uses `langchain-mcp-adapters` to expose the selected Weavz server's tools as LangChain tools. Tool Mode/Code Mode are configured server-side; the local `--mode` flag validates the expected names and does not switch a live server.

Requires Python 3.11 or 3.12, uv, a custom Weavz Streamable HTTP URL and MCP access token. Do not substitute a Weavz REST API key. The hosted personal `/mcp/weavz` connector uses OAuth.

```sh
uv sync --locked
uv run python main.py --fixture --mode tools
uv run python main.py --fixture --mode code
uv run pytest
```

The fixture is a separate loopback MCP SDK server with bearer auth and synthetic data. No model or live Weavz call is made. For live discovery, copy `.env.example` to `.env` and run `uv run --env-file .env python main.py --mode tools` or `--mode code`. For an optional LangChain agent run, connect an app with relevant data, set `WEAVZ_MODEL` and `OPENAI_API_KEY`, then add `--prompt "Find the latest record matching the task you connected"`. Each tool call uses the adapter-managed HTTP lifecycle.
