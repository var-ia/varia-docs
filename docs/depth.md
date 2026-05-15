# Analysis depth levels

The `--depth` flag controls how many analyzers run and how aggressively they parse. Applies to `analyze`, `diff`, and `explore` commands.

| Level | Analyzers | Use case |
|-------|-----------|----------|
| `brief` | Section differ only | Quick check — did anything change on this page? |
| `detailed` | Section differ, citation tracker, revert detector, template tracker | Standard analysis — most common |
| `forensic` | All analyzers including protection tracker, page move detector, category diff, wikilink diff, talk page correlation, edit cluster detection | Full audit — export evidence or build timelines |

`brief` skips citation parsing, revert pattern matching, and template extraction. It only compares section headings between revisions.

`detailed` (default) runs the full set of deterministic analyzers. Sufficient for most workflows.

`forensic` enables cross-revision correlation (talk page matching, edit cluster detection) and metadata trackers (protection changes, page moves, category and wikilink diffs). Use when you need the complete event stream for export or visualization.

Example:

```bash
wikihistory analyze "Earth" --depth forensic --cache
```
