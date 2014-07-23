### Viewport

http://www.quirksmode.org/mobile/viewports.html
http://www.quirksmode.org/mobile/viewports2.html

- Device pixels
- CSS pixels
- Viewport


#### Desktop browsers


##### document.documentElement.clientWidth / -Height
> Viewport dimensions


##### window.innerWidth / -Height
> Total size of the browser window, including scrollbars.


##### window.pageXOffset / pageYOffset
> Scrolling offset of the page.


##### Event coordinates
> *pageX/Y* gives the coordinates relative to the <html> element in CSS pixels.

> *clientX/Y* gives the coordinates relative to the viewport in CSS pixels.

> *screenX/Y* gives the coordinates relative to the screen in device pixels.


##### document.documentElement.offsetWidth / -Height.
> Dimensions of the <html> element (and thus of the page)


##### screen.width / height
> Total size of the user’s screen.
> Measured in Device pixels (CSS pixels in IE7, 8)