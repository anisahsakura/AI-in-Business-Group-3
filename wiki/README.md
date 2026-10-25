# LLM wiki

The team's knowledge store, built with the agentic CLI on 8 October following [Karpathy's LLM-wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

- **Browse it:** [claude.ai/artifact/9fs9ktG5VN3psY9bziWpnE](https://claude.ai/artifact/9fs9ktG5VN3psY9bziWpnE)
- **Files:** the contents of `llm-wiki-group3.tar.gz`, extracted into this folder.

## How it works

- A source enters only after a person has verified it (Verify step, see the [agent README](../agent/README.md)).
- Its raw text is kept **verbatim**; Dutch sources stay Dutch.
- Each source gets a source page. `lint` compares every quotation on a page with the raw file, character by character, and flags orphan pages. The quote check was tested by altering one quotation on purpose; lint caught it (failure log F10).

## Status (25 October 2026)

One source is ingested, and it fails the Evidence gate, so the wiki cannot yet support a claim in a handbook chapter. The other sources are queued in `INGEST-QUEUE.md`, because the build environment could not reach the source sites (failure log F9). They are to be fetched on a team machine.
