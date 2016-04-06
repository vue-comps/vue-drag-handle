# vue-drag-handle

To make something responsive to dragging, a drag-handle is needed.

### [See it in action](https://vue-comps.github.io/vue-drag-handle)

# Install

```sh
npm install --save-dev vue-drag-handle vue-touch
```
or include `build/bundle.js`

## Usage
```coffee
# somewhere
Vue.use(require('vue-touch'))

# in your component
components:
  "drag-handle": require("vue-drag-handle")
# or, when using bundle.js
components:
  "drag-handle": window.vueComps.dragHandle
```
#### Options
| Name | type | default | description |
| ---:| --- | ---| --- |
| factor | Number | 2 | factor which is multiplied with the movement |
| zIndex | Number | 1001 | z-index of the overlay |
| maxOpen | Number | null | (required) maximum position |
| widthOpened | String | "70%" | width when opened |
| widthClosed | String | "20px" | width when closed |
| left | Boolean | true | is left when closed |
| dismissable | Boolean | true | can it get closed by click or ESC? |
| onMove | Function | null | (required) will be called with the current position |
| onOpen | Function | null | (required) will be called when opened successfully |
| onOpenAbort | Function | onClose | will be called when opened unsuccessfully |
| onClose | Function | null | (required) will be called when closed successfully |
| onCloseAbort | Function | onOpen | will be called when closed unsuccessfully |

# Development
Clone repository
```sh
npm install
npm run dev
```
Browse to `http://localhost:8080/`

## License
Copyright (c) 2016 Paul Pflugradt
Licensed under the MIT license.
