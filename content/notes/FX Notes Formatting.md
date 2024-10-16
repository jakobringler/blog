---
title: FX Notes Formatting
draft: false
tags:
  - obsidian
---
## Headline

text should start here
### Subheadline

text should start here
#### Subsubheadline if really necessary
## Images

All image files have to live in `../content/notes/images`

![[notes/images/imagetensor.png]]

You can scale Images by adding max pixel width value to the end of the path:

`![[notes/images/imagetensor.png|150]]`

![[notes/images/imagetensor.png|150]]
## Code Blocks

use C style colors for vex

specify what type of wrangle this goes in and the name it has (e.g. in the screenshot above)

// point wrangle "wrangle_name"

```C
vex
```

Python for Python and so on
```Python
print("Hello Houdini")
```

Attributes and other important stuff can be in single backticks:

```
`attributes`
```

will be rendered like this:

`attributes` 
## Links

`[External Link](https://fxnotes.xyz)`

[External Link](https://www.youtube.com/watch?v=dQw4w9WgXcQ)
## Callouts

you can have fancy callout boxes using this format:

```
> \[!tip] **Tips**
> 
> Hot Tip!
```

beside `tip` you can also put:
- info
- todo
- success
- quesiton
- warning
- failure
- danger
- bug
- example
- quote

> [!tip] **Tips**
> 
> Hot Tip!

> [!quote] **Sources:**
> Important Source!

## Downloads

shared files have to live in `../content/notes/sharedfiles`

```
##### Download: [File](https://github.com/jakobringler/blog/tree/hugo/content/notes/sharedfiles/filename.hip)
```

##### Download: [File](https://github.com/jakobringler/blog/tree/hugo/content/notes/sharedfiles/filename.hip)
## Page End

every page ends with this line `---` and the sources block

---

sources / further reading:
- links and shit (format: `[linkname](link)`)

