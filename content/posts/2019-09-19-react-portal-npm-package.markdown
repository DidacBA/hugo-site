---
title:  "Hijacker: a React component using portals"
date:   2019-09-19 19:27:00 -0400
excerpt: "Published my first NPM package, a React component using portals to hijack other nodes"
showDate: true
draft: false
tags: ["react","portals"]
---

This was something that I wanted to do since completing our last project at Ironhack Barcelona. During that project, we were faced with a problem while using Mapbox and React.

We wanted to put a React component in the popups that Mapbox can create. ReactDOM.render would have been useful if we hadn't been building a React application already, but we needed something that would allow us to insert a React component from another component, and that's how we reached portals.

It uses a mutation observer to look for nodes created with the class name passed with the nodeSelector prop. It's useful if you're using any other library that manipulates the DOM and your React code to be aware of that to insert some content in there.

```react
function App() {
  return (
    <Hijacker nodeSelector='test-div' >
      <>
        <h1>PORTAL!</h1>
        <h2>WOOOHOO</h2>
      </>
    </Hijacker>
  )
}
```

Check it out, break it, tell me how awful it is!

[Link to NPM page](https://www.npmjs.com/package/react-portal-hijacker)
