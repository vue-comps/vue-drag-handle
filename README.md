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
```html
  <drag-handle
    @move="move"
    @opened="open"
    @open-aborted="close"
    @closed="close"
    @close-aborted="open"
    :max-width="200"
  ></drag-handle>
```
#### Props
| Name | type | default | description |
| ---:| --- | ---| --- |
| factor | Number | 2 | factor which is multiplied with the movement |
| z-index | Number | 1002 | z-index of the overlay |
| max-width | Number | null | (required) maximum width |
| width-opened | String | "70%" | width when opened |
| width-closed | String | "20px" | width when closed |
| right | Boolean | false | is right or left when closed |
| not-dismissible | Boolean | false | should it stay open on click or ESC |
| is-opened | Boolean |  false | can two-way sync. Will not emit `opened` or `closed` |


#### Events
| Name |  description |
| ---:| --- |
| move |  will be called with the current position |
| opened |  will be called when opened successfully |
| open-aborted | will be called when opened unsuccessfully |
| closed |  will be called when closed successfully |
| close-aborted |  will be called when closed unsuccessfully |


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
