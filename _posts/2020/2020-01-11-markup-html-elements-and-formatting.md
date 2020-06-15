---
title: "Coming soon !"
sub_title: "Preview"
categories:
  - Python
  - Automation
  - Networking
elements:
  - content
---

Welcome to Imad's Internet Corner ! Here's a random Python function :

```python
def CreatDirectory(path, directory):

	cfgPath = os.path.join(path, directory)
	try:
		os.makedirs(cfgPath)
		return cfgPath

	except FileExistsError:
		return cfgPath
```
