# Mobile JavaScipt Application Development 


---

##Chapter 1 HTML5 for Mobile Application
- `<!DOCTYPE html>`
- `<meta charset="utf-8">`
- `<link rel="stylesheet" href="style.css">`
- `<script src="app.js">` 
- The new HTML5 spec clearly expects style sheets to be CSS files, and scripts to be JavaScript; We don't need to add closing `/` to standalone tags anymore.( if you want, you can, but pay attention to the fact that HTML5 is not based in XHTML, but in HTML 4.01).

Obsolete:
`<frame>`,`<frameset>`,`<noframes>`,`<acronym>`,`<font>`,`<big>`,`<center>`,`<strike>`
New:
`<canvas>`,`<audio>`,`<video>`,`<mark>`,`<time>`,`<meter>`,`<progress>`,`<section>`,`<header>`,`<article>`

###Metadata for HTML5 Applications
- `apple-mobile-web-app-capable`
- check whether the application is running is `standalone` mode: `window.navigator.standalone`
- `viewport` allows developers to disable the typical pinching and zooming available to users of Mobile safari: `<meta name="viewport" content="initial-scale=1.0, user-scalable=no">`
- ` apple-touch-icon`pecifies the icon to be displayed on the home screen of the device when the HTML5 application is used in standalone mode: `<link rel="apple-touch-icon" href="icon.png"/>`
- `<link rel="apple-touch-icon-precomposed" href="icon.png"/>`
- `<meta name="apple-mobile-web-app-status-bar-style" content="black">`
- `<link rel="apple-touch-startup-image" href="Default.png" />`

###HTML5 Application Cache
- 
