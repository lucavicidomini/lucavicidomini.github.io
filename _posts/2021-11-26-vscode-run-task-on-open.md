---
layout: post
title: "VSCode: run task on open"
---

Hit `Ctrl+Shift+B` and select *Configure build task* > *Create tasks.json from template* > *Other*.

Now edit `tasks.json` and and put something like this:

```
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "npm",
			"script": "dev",
			"group": "build",
			"problemMatcher": [],
			"label": "npm: dev",
			"detail": "Serve",
			"runOptions": {
				"runOn": "folderOpen"
			}
		}
	]
}
```

Use `"type": "shell"` to run a shell command.
