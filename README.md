# use-viewport-sizes #

![npm](https://img.shields.io/npm/v/use-viewport-sizes.svg?color=blue) ![David](https://img.shields.io/david/rob2d/use-viewport-sizes.svg) ![npm](https://img.shields.io/npm/dw/use-viewport-sizes.svg?color=red) ![GitHub issues](https://img.shields.io/github/issues-raw/rob2d/use-viewport-sizes.svg) ![NPM](https://img.shields.io/npm/l/use-viewport-sizes.svg)

a tiny React hook which allows you to track visible window viewport size in your components w/ an optional debounce for updates for optimal rendering.

## Installation ##

```
npm install -D use-viewport-sizes
```

## Benefits ##
- extremely lightweight and dependency-free -- **2.25kb** before gzipping.
- only one `window.onresize` handler used to subscribe to any changes in an unlimited number of components.
- optional debounce to delay updates until user stops dragging their window for a moment; this can make expensive components with size-dependent calculations run much faster and your app feel smoother.
- debouncing does not create new handlers or waste re-renders in your component; the results are also pooled from only one resize result.
- supports SSR.

## Usage ##

### **See it in Action** ###

<center>
<img src="./doc/use-viewport-sizes.gif" />

*Interact with the example shown on CodeSandox* @
[https://codesandbox.io/s/j3vzpqxwww](https://codesandbox.io/s/j3vzpqxwww)

</center>

### **Without Debouncing**
*registers dimension changes on every resize event immediately*

```js
import React from 'react'
import useViewportSizes from 'use-viewport-sizes'

function MyComponent (props) {
    const [vpWidth, vpHeight] = useViewportSizes();

    ...renderLogic
}
```


### **With Debouncing**  
*registers dimension changes only when a user stops dragging/resizing the window for a specified number of miliseconds; for expensive components such as data grids which may be too
expensive to re-render during window resize dragging.*
```js
import React from 'react'
import useViewportSizes from 'use-viewport-sizes'

function MyExpensivelyRenderedComponent (props) {
    const [vpWidth, vpHeight] = useViewportSizes(1000); // 1s debounce

    ...renderLogic
}
```

### **Server Side Rendering**  

*While serverside rendering is supported, it requires an explicit update via `useEffect` since viewport does not actually exist on the server before rendering to client. The following has been tested with [NextJS](https://nextjs.org/).*

*Sidenote that you will see a `useLayoutEffect` warning from React. This is perfectly normal as there is no viewport/context to paint to when pre-rendering in SSR and will not interfere with your app once served to the client*

```js
import React, { useLayoutEffect } from 'react'
import useViewportSizes from 'use-viewport-sizes'

function MySSRComponent (props) {
    const [vpWidth, vpHeight, updateVpSizes] = useViewportSizes()

    // below, we add one post-render update
    // in order to register the client's viewport sizes
    // after serving SSR content

    useEffect(()=> { updateVpSizes(); }, []);

    ...renderLogic
}
```

## Support
If you have read the examples and have any issues which you know are glitches,or would like to request something changed, please feel free to [post an issue on Github](https://github.com/rob2d/use-viewport-sizes/issues/new). 

Otherwise, if this was useful and you'd like to show your support, no donations necessary, but please consider [checking out the repo](https://github.com/rob2d/use-viewport-sizes) and giving a star.

## License ##

- Open Source **[MIT license](http://opensource.org/licenses/mit-license.php)**
- 2019 © <a href="http://robertconcepcion.com" target="_blank">Robert Concepción III</a>