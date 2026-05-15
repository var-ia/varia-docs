# Installation

## System requirements

- **Node.js** 20+ or **Bun** v1.2+
- **macOS** or **Linux** (Windows not yet supported)

## Zero install (recommended)

No download needed — runs directly from npm:

```bash
npx @refract-org/cli analyze "Earth" --depth brief
```

With bun (if installed):

```bash
bunx @refract-org/cli analyze "Earth" --depth brief
```

## Local install

```bash
# with bun
bun add -g @refract-org/cli

# with npm
npm install -g @refract-org/cli
```

Then use the `wikihistory` command directly:

```bash
wikihistory analyze "Earth" --depth brief
```

## Docker

Prebuilt image (no build step):

```bash
docker run ghcr.io/var-ia/cli analyze "Earth" --depth brief
```

Or build from source:

```bash
git clone https://github.com/refract-org/refract.git
cd refract
docker build -t refract .
docker run --rm refract analyze "Earth" --depth brief
```

## From source

```bash
git clone https://github.com/refract-org/refract.git
cd refract
bun install
bun run build
```

## Verify installation

```bash
wikihistory --version
```
