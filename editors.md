---
description: This page shows how to create and implement Custom Editors
---

# Editors

## Integer Editor

This Editor is already in place. To upgrade it to a NumericUpDown do the following

In Serenity.CoreLib.js search for IntegerEditor. Find the Element tag and change it:

![](.gitbook/assets/image.png)

From:

```markup
<input type="text"/>
```

&#x20;To:

```markup
<input type="number" min="0" max="2000" step="1"/>
```

