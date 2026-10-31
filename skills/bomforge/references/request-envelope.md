# Summarized BOM sourcing request

Use `search_sourcing_brief` after interpreting the available engineering context locally. The tool accepts 1–50 manufacturing work packages, not a raw BOM or transcript. Read the actual tool schema in the current session before calling it.

- `required` contains only user-established constraints; these affect filtering.
- `search_signals` contains model-inferred clues; these affect recall, not requirements.
- Preserve explicit geography. The default is U.S.-only; set `us_only: false` for an explicitly international search.
- Keep exact source-line references locally so the user can trace a work package back to their BOM.
- `privacy.raw_bom_transmitted` describes what you actually sent. Reduce the request until it can truthfully be false.
- If the task has more than 50 distinct packages that cannot be combined without losing hard constraints, explain the size limit and agree on a bounded first batch. Do not silently drop lines.

Example: the user explicitly requires aluminum 6061 components in the United States and says the project is a prototype. Five-axis machining is inferred from the description, so it remains a search signal.

```json
{
  "objective": "Find candidate suppliers for prototype aluminum components",
  "work_packages": [
    {
      "id": "machined-aluminum",
      "name": "Machined aluminum components",
      "description": "Small machined structural components with multiple faces",
      "required": {
        "materials": ["aluminum 6061"],
        "geography": { "country": "United States" }
      },
      "search_signals": { "capabilities": ["5-axis CNC machining"] },
      "production_stage": "prototype"
    }
  ],
  "us_only": true,
  "results_per_package": 5,
  "privacy": {
    "source": "bom_markdown",
    "raw_bom_transmitted": false,
    "redacted_fields": ["part numbers", "customer name", "exact geometry"]
  }
}
```

The response provides candidates by work package and distinguishes observed required terms from absent terms. Do not promote a search match into qualification or supplier approval. Keep additional diligence in working notes and report supported findings with profile links.
