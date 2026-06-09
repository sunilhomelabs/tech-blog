# Graph Report - /root/tech-blog  (2026-06-09)

## Corpus Check
- cluster-only mode — file stats not available

## Summary
- 11 nodes · 8 edges · 3 communities
- Extraction: 88% EXTRACTED · 12% INFERRED · 0% AMBIGUOUS · INFERRED: 1 edges (avg confidence: 0.9)
- Token cost: 0 input · 0 output

## Community Hubs (Navigation)
- [[_COMMUNITY_Community 0|Community 0]]
- [[_COMMUNITY_Community 1|Community 1]]
- [[_COMMUNITY_Community 2|Community 2]]

## God Nodes (most connected - your core abstractions)
1. `Naruto (AI SRE)` - 3 edges
2. `Naruto's Tech Blog Configuration` - 2 edges
3. `Build and Deploy Workflow` - 2 edges
4. `Why My Name Is Naruto` - 2 edges
5. `MkDocs` - 1 edges
6. `Material for MkDocs` - 1 edges
7. `Python Dependencies` - 1 edges
8. `Sunil (Lab Owner)` - 1 edges
9. `Sunil's Home Lab` - 1 edges
10. `Blog Landing Page` - 1 edges

## Surprising Connections (you probably didn't know these)
- `Build and Deploy Workflow` --calls--> `MkDocs`  [EXTRACTED]
  .github/workflows/deploy.yml → README.md
- `Naruto's Tech Blog Configuration` --implements--> `Material for MkDocs`  [EXTRACTED]
  mkdocs.yml → README.md
- `Naruto's Tech Blog Configuration` --references--> `Blog Landing Page`  [EXTRACTED]
  mkdocs.yml → docs/index.md
- `Build and Deploy Workflow` --references--> `Python Dependencies`  [EXTRACTED]
  .github/workflows/deploy.yml → requirements.txt
- `Why My Name Is Naruto` --references--> `Naruto (AI SRE)`  [EXTRACTED]
  docs/blog/2026-06-08-why-my-name-is-naruto.md → docs/about.md

## Import Cycles
- None detected.

## Communities (3 total, 0 thin omitted)

### Community 0 - "Community 0"
Cohesion: 0.40
Nodes (5): Sunil's Home Lab, Naruto (AI SRE), Sunil (Lab Owner), Why My Name Is Naruto, SRE Philosophy

### Community 1 - "Community 1"
Cohesion: 0.67
Nodes (3): Build and Deploy Workflow, MkDocs, Python Dependencies

### Community 2 - "Community 2"
Cohesion: 0.67
Nodes (3): Blog Landing Page, Naruto's Tech Blog Configuration, Material for MkDocs

## Knowledge Gaps
- **7 isolated node(s):** `MkDocs`, `Material for MkDocs`, `Python Dependencies`, `Sunil (Lab Owner)`, `Sunil's Home Lab` (+2 more)
  These have ≤1 connection - possible missing edges or undocumented components.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **What connects `MkDocs`, `Material for MkDocs`, `Python Dependencies` to the rest of the system?**
  _7 weakly-connected nodes found - possible documentation gaps or missing edges._