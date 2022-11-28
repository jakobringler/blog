---
title: "Python Scripting in Resolve"
draft: false
tags:
- resolve
---

## Setup

### Environment Variables
When setting up the API access on windows with environment variables make sure to NOT use nested variables as python will read those as strings. 

Instead of using the variables as shown in the provided README.txt from Black Magic Design make sure to replace all nested variables with hardcoded paths.

Windows:

```bash
RESOLVE_SCRIPT_API="C:\\ProgramData\\Blackmagic Design\\DaVinci Resolve\\Support\\Developer\\Scripting\\"
RESOLVE_SCRIPT_LIB="C:\\Program Files\\Blackmagic Design\\DaVinci Resolve\\fusionscript.dll"
PYTHONPATH="%PYTHONPATH%;C:\\ProgramData\\Blackmagic Design\\DaVinci Resolve\\Support\\Developer\\Scripting\\Modules\\"
```

Explained here: [Python scripting error - Blackmagic Forum](https://forum.blackmagicdesign.com/viewtopic.php?f=21&t=137340)

### Imp Module



---

sources / further reading:
- [Unofficial DaVinci Resolve Scripting Documentation](https://deric.github.io/DaVinciResolve-API-Docs/)
- [Python scripting error - Blackmagic Forum](https://forum.blackmagicdesign.com/viewtopic.php?f=21&t=137340)

