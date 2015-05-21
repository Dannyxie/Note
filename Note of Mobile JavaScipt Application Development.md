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
- The application cache is simply a text file (encoded in UTF-8) that consists of major sections:
	A CACHE section
		This part specifies the relative and/or absolute URLs of the resources that the device should keep offline.
	A NETWORK section
		This part of the application cache lists the URLs of the resources that cannot (or must not) be kept offline. For example, in this case ( a real cache manifest from an application written by the author) we are specifying all the URLs of the Google Maps API, which, according to Google's Terms of Service, cannot be cached offline.
	A FALLBACK section
		This section provides a URL that will be served whenever a resource specified in the NETWORK section is required when the application is offline.
	
