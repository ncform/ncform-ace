# ncform-ace

ncform-ace widget for [ncform](https://github.com/ncform/ncform)

![](https://raw.githubusercontent.com/ncform/ncform-ace/master/preview.gif)

## Install and basic usage

```
npm i -s @ncform/ncform-ace
```

**Add the widget**

```
import ncformAce from '@ncform/ncform-ace';

Vue.use(vueNcform, { extComponents: {ncformAce} });

// or vm.$ncformAddWidget({name: 'ncformAce', widget: ncformAce});

```

**Use the widget**

```
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "widget": "ncform-ace",
      "widgetConfig": {
        "height": "300px"    
      }
    }
  }
}
```

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

You only need to care about `src/components/index.vue`

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Run your end-to-end tests
```
npm run test:e2e
```

### Run your unit tests
```
npm run test:unit
```
