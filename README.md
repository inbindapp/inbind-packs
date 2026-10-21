# Inbind packs

Workspace templates for Inbind. The app's "Start from template" lists the top-level folders of this repository that contain a `pack.json`, and copies the chosen folder into a new workspace.

A pack is one top-level folder with this layout:

```
<pack>/
  pack.json        {"name", "description", "version"}
  AGENTS.md        workspace instructions
  Binders/         starter Binder pages (index.md first)
  Notes/           starter notes structure
  .claude/skills/  optional pack-specific skills
  ...              any other folders the pack needs (for example Content/)
```

The app reads the `main` branch.
