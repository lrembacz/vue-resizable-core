# VueResizableCore
ResizableCore component for VueJs. Component to change element size.

It's based on https://github.com/STRML/react-resizable

# Example

- Clone project.
- run `$ npm run dev`

# Usage
## NPM
```
npm install vue-resizable-core
```

## Registration
```
import VueResizableCore from 'vue-resizable-core'
Vue.component('vue-resizable-core', VueResizableCore)
```

# API

## Props

```
className: {
	type: String,
	required: false,
	default: ""
},
w: {
	type: Number,
	required: true
},
h: {
	type: Number,
	required: true
},
grid: {
	type: Array,
	required: false,
	default: () => [1, 1]
},
lockAspectRatio: {
	type: Boolean,
	required: false,
	default: false
},
//'both' | 'x' | 'y' | 'none';
axis: {
	type: String,
	required: false,
	default: 'both'
},
minConstraints: {
	type: Array,
	required: false,
	default: () => [20, 20]
},
maxConstraints: {
	type: Array,
	required: false,
	default: () => [Infinity, Infinity]
},
onResizeStart: {
	type: Function,
	required: false,
	default: () => {}
},
onResizeStop: {
	type: Function,
	required: false,
	default: () => {}
},
onResize: {
	type: Function,
	required: false,
	default: () => {}
},
```

## Description

### w

Width of element.

### h

Height of element.

### grid

Snaps resized element to grid, default `[1,1]`.

### lockAspectRatio

Grants `draggability` to given selector, others selectors inside are undraggable.

### axis

Grants one of directions in which we can resize element `'both' | 'x' | 'y' | 'none'`;

### minConstraints

Min element size, default `[20, 20]`

### maxConstraints

Max element size, default `[Infinity, Infinity]`

### onResizeStart

Function called when `resizing` is started.

### onResize

Function called when `resizing` is moved.

### onResizeStop

Function called when `resizing` is ended.

### className

Given string is merged into css class of component.

# License
MIT