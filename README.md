### CPSI design system

Install:

```bash
npm install cpsi-ds --save
```

Import:

```javascript
import Vue from 'vue'
import CPSIDs from 'cpsi-ds'

Vue.use(CPSIDs)
```
```javascript
//In your nuxt.config.js
styleResources: {
  scss: ['cpsi-ds/src/assets/scss/main.scss'],
},
css: ['cpsi-ds/src/assets/scss/_indents.scss', 'cpsi-ds/src/assets/css/reset.css'],
```