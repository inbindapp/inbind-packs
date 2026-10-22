# Inbind packs

Packs for Inbind: ready-made workspaces. The app's "Start from a pack" lists the top-level folders of this repository that contain a `pack.json`, and copies the chosen pack into a new workspace.

A pack is one top-level folder with this layout:

```
<pack>/
  pack.json        {"name", "description", "version"}
  AGENTS.md        workspace instructions
  Binders/         starter Binder pages (index.md first)
  Notes/           starter notes structure
  .agents/skills/  optional pack-specific skills
  ...              any other folders the pack needs (for example Content/)
```

The app reads the `main` branch.
