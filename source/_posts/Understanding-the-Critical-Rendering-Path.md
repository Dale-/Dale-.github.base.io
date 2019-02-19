---
title: Understanding the Critical Rendering Path
layout: post
date: 2017-05-27 10:38:04
comments: true
tags: [JavaScript]
categories: [JavaScript]
keywords: JavaScript
description: rendering path
---

## Critical Rendering Path
### There are 6 stages to the CRP -
1. Constructing the DOM Tree
2. Constructing the CSSOM Tree
3. Running JavaScript
4. Creating the Render Tree
5. Generating the Layout
6. Painting

![](/images/rendering-path/CRP-Sequence-Copy.png)

<!-- more -->

### Constructing the DOM Tree
![](/images/rendering-path/DOM.png)

### Constructing the CSSOM Tree
![](/images/rendering-path/CSSOM.png)

### Running JavaScript
```bash
<script async src="script.js">
```

### Creating the Render Tree
![](/images/rendering-path/Render-Tree.png)

### Generating the layout
```bash
  <meta name="viewport" content="width=device-width,initial-scale=1">
```

## Putting it All Together
1. Send Request - GET request sent for index.html
2. Parse HTML and Send Request - Begin parsing of HTML and DOM construction. Send GET request for style.css and main.js
3. Parse Stylesheet - CSSOM created for style.css
4. Evaluate Script - Evaluate main.js
5. Layout - Generate Layout based on meta viewport tag in HTML
6. Paint - Paint pixels on document

Copy For [Understanding the Critical Rendering Path](https://bitsofco.de/understanding-the-critical-rendering-path/)
