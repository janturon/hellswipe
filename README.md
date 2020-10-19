# Hellswipe - swipe elements

Touch displays are common devices today and swipes are very natural actions to perform. Unfortunately, there is no natural support for time being and there are some issues to keep in mind:
 - Browsers may or may not bind touch actions (like `touchmove`) to mouse actions (like `mousemove`) and touch (unlike mouse action) can fire both. Touch action always fire before mouse action, so `e.preventDefault()` should be called in it.
 - Some devices (tablets with mouse, notebooks with touch display) may use both touch and mouse and swipe actions should intuitively react to both in the same way.
 - Trying to swipe with mouse usually selects text instead, so `ondragstart` should be intercepted and prevented.
 - Swipe usually ends outside of the element of its origin, so move actions should be attached to document. To ease the heavy load, the move actions should be attached on swipe start and detached on swipe end.
 - Users tends to swipe further on larger elements and less aggresively on smaller elements. There should be some threshold settings.
 - Swipes are oftentimes used with dragging (to peek/scroll) which can suddenly speed up and change to swipe.

All these issues are solved in the hellswipe.js published under the HELL licence having only 666 bytes long code. The syntax is:

```
hellswipe(element, settings, onswipe, ondrag)
```

- `element` is the element you want to swipe
- `settings = {dx,dy,dt}` means delta x, delta y and delta time: how many pixels per time (in milliseconds) needs the user to drag in the respective axis to be recognized as swipe. The distance is measured in `dt` interals, so the threshold should be set to half if the measurement happens in the middle of the interval and the `dt` reasonably low (like 200-300 ms). You can omit one axis (`dx` or `dy`) if you need only one-dimensional swiping (most of the cases)
- `onswipe(element, e={x,y})` fires on swipe. `element` is the source element and `e` consists of `x` and `y`: pixels dragged in the `dt` which can roughly tell the speed of the swipe (see the `settings` explanation).
- `ondrag(element, e={x,y,s})` fires on drag. `x` and `y` contains the event clientX resp. clientY. `s` contains number 0-2:
	- 1 = touch drag just started
	- 2 = mouse drag just started
	- 0 = drag continues

For instance, this ondrag handler tells you what is the source of the event and what is the horizontal distance dragged from the mousedown or touchstart:

```
var x = 0;
const dragHandler = (src, d) => {
	if(d.s==1) console.log("touch");
	if(d.s==2) console.log("mouse");
	if(d.s) x = d.x;
	else console.log(d.x - x);
};
```