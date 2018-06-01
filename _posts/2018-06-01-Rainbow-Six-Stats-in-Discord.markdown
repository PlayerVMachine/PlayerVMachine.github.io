---
layout: post
title:  "Rainbow Six Stats in Discord"
date:   2018-06-01 15:39:40 -1700
categories: jekyll update
---

Central Media is my current Discord Bot project. I'll talk about in more in an upcoming post but for now we'll focus on a part of its main objectives: game statistics. Today we'll be getting [Rainbow Six Siege](https://rainbow6.ubisoft.com/siege/en-ca/home/index.aspx) statistics. The game statistics will be collected from [R6Stats](https://r6stats.com) making use of the npm module [rainbowsix-api-node](https://www.npmjs.com/package/rainbowsix-api-node).

```javascript
const r6 = require('rainbowsix-api-node')
```

