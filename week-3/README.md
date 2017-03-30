# PONG in the browser

The code in this repository tries to solve the following user story.

> I want to play a game of Pong against someone else.

The proof of concept can be found online at [nickrttn.github.io/browser-technologies/week-3/](https://nickrttn.github.io/browser-technologies/week-3/).

## Core functionality

A player moves the paddle in a device-appropriate way, to be able to play the ball back and forth, until a player scores. The game is restarted after a score.

## How it works

In this proof of concept, when the JavaScript code initializes, the first thing it does is check for `<input type="range">` support. If that element type is not supported, the inputs are hidden, and the player is able to use buttons to move their paddle. Additionally, the keyboard is always available to control the paddles.

## Problems, problems, problems

Going in, I knew this was going to be a hard nut to crack. It's part of the reason I picked it in the first place.

What I built is an animated SVG. It's controlled by one of three things: a `<input type="range">` slider input, buttons, or keyboard keys.

The different forms of input all work with event listeners. `input`, `change`, `keyup`, `keydown` and `click` are used. `change` is used as a fallback for older browsers that do not support the `input` event. In modern browsers the `change` event only fires once, after letting go of the control, while `input` fires on every input change, which is the desired behavior. In older browsers, `change` fires on every input change, whereas `input` doesn't work.

Building a progressively enhanced version of this is problematic. First things first: it's never going to do much without JavaScript, so that's basically a requirement for it to work.

Second, SVG Version 1.1 support is weird. Browser handle width and height scaling differently, and may or may not preserve aspect ratio. The only way to fix this predictably is the [padding-bottom hack](https://tympanus.net/codrops/2014/08/19/making-svgs-responsive-with-css/). In this case, that hack is not desirable, because I want to place elements directly under the SVG, and I have no way of predicting where those should be placed, without setting the width of the SVG to something in pixels.

I wanted my SVG to scale, so it would never grow bigger than the user's viewport. Without that, PONG would be unplayable. Because of this, the padding-bottom hack is unusable, so I have no way to scale my SVG predictably (mostly in IE browsers).

The demo is supported in Chrome, Firefox, IE9+, Safari (macOS/iOS), and Opera (but not Opera Mini). IE9 and up show a distorted playing field, but the paddles are fully movable.

## Uses

- Inline SVG
- requestAnimationFrame (with a fallback for older browsers)
- input[type="range"]
- keydown/-up, input, change and click event listeners

## Wishlist

- An actually moving ball
- A better experience for IE users
- The ball is not moving!1!!!!!11!!!!9

## Sources

- [Styling Cross-Browser Compatible Range Inputs with CSS](https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/)
- [MDN](https://developer.mozilla.org/)
- [Can I use _____?](http://caniuse.com/)
- [SVG padding-bottom hack](https://tympanus.net/codrops/2014/08/19/making-svgs-responsive-with-css/)
