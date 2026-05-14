# Installation

## System requirements

- **Bun** v1.2+ (recommended runtime)
- **Node.js** 20+ (alternative runtime)
- **macOS** or **Linux** (Windows not yet supported)

## npm / bun (recommended)

```bash
# with bun
bun add -g @var-ia/cli

# with npm
npm install -g @var-ia/cli
```

## Docker

```bash
# build from source
docker build -t varia https://github.com/var-ia/varia.git
docker run --rm varia analyze "Earth" --depth quick

# or build locally from a cloned repo
git clone https://github.com/var-ia/varia.git
cd varia
docker build -t varia .
docker run --rm varia analyze "Earth" --depth quick
```

## From source

```bash
git clone https://github.com/var-ia/varia.git
cd varia
bun install
bun run build
```

## Verify installation

```bash
wikihistory --version
```
