---
title: "Lua Scripting in Resolve"
draft: false
tags:
- resolve
- lua
---

Overview and snippets for [[notes/Lua |Lua]] coding in DaVinci Resolve.

> A good API overview and multiple examples can be found in Deric's [Unofficial DaVinci Resolve Scripting Documentation](https://github.com/deric/DaVinciResolve-API-Docs)

## Basics

### Getting Base Objects

```lua
resolve = Resolve()
projectManager = resolve:GetProjectManager()
project = projectManager:GetCurrentProject()
mediaPool = project:GetMediaPool()
rootFolder = mediaPool:GetRootFolder()
clips = rootFolder:GetClips()
timelineCount = project:GetTimelineCount()
```

### Switching Pages

```lua
resolve:OpenPage("color") -- ("media", "cut", "edit", "fusion", "color", "fairlight", "deliver").
```



---

sources / further reading:
- [Unofficial DaVinci Resolve Scripting Documentation](https://github.com/deric/DaVinciResolve-API-Docs)

