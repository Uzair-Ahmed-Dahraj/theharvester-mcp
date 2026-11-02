# Contributing to theharvester-mcp

Thanks for your interest in the theHarvester MCP server.

## Ways to contribute

- Bug reports and feature ideas: open a GitHub issue. For a failed harvest, include your MCP
  client, Docker version, and the target domain shape (not the domain itself).
- Security reports: see [SECURITY.md](SECURITY.md). Do not open public issues for vulnerabilities.

## Development setup

```bash
git clone https://github.com/devkindhq/theharvester-mcp.git
cd theharvester-mcp
python3.12 -m venv .venv && .venv/bin/pip install -r requirements.txt pytest
.venv/bin/python -m pytest
```

Use Python 3.12, the same as the image. The tests start `server.py` and speak MCP over stdio.
The tests do not need theHarvester installed. CI runs them on Python 3.12 and builds the Docker
image for `linux/amd64`. CI runs both jobs on every pull request.

## Ground rules

- Keep changes small and focused, one concern per PR.
- Never commit client names, client domains, API keys or customer data. Use `example.com` in
  every example.
- Harvest only domains you own or have written permission to research.

## Pull requests

1. Fork, branch from `main`, and commit with a
   [Conventional Commits](https://www.conventionalcommits.org/) message, for example
   `fix: reject empty domains`.
2. Make sure `python -m pytest` passes.
3. Open the PR and describe what changed and why.

## Releasing

1. In one PR, set the new version `X.Y.Z` in all of these places:
   - `server.json`: `version` and the image tag in `packages[0].identifier`
     (`docker.io/devkind/theharvester-mcp:X.Y.Z`).
   - `.claude-plugin/plugin.json`: `version`.
   - `.mcp.json`: the image `devkind/theharvester-mcp:X.Y.Z`.
2. Merge the PR to `main`.
3. Push the tag `vX.Y.Z` from `main`.

The tag runs `.github/workflows/release.yml`. The workflow stops if the tag does not match the
`server.json` version and image tag. It then builds and pushes the multi-arch image to Docker Hub
(`X.Y.Z` and `latest`), and publishes `server.json` to the Official MCP Registry with GitHub OIDC.

The workflow needs the `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` repository secrets. Registry
auth uses GitHub OIDC and needs no secret.

## License

By contributing you agree your work is released under the MIT License.
