# a11y notes
A cheat sheet and notes for accessibility 

## Focus
Focus determines where keyboard events go in the page <br>
`Input, button, and dropdowns` have are implicitly focusable. They are already in the `tab order + keyboard` event handling.<br>

**Not all elements are focusable**
- [ ] headers
- [ ] paragraphs
- [ ] images <br> 
  
Which is by design, since most cases they will not be interactable.

> tab = move focus forward <br>
> shift + tab = move focus backwards <br>
> arrow keys = moves focus within a component

### Tabindex
```html
  <div tabindex="0">Focus Me!</div>
```

If there are instances where you want to modify the tab order, for example 
building a component without a native analog, or if you have something offscreen that 
should be focusable when it appears like a modal window.

- [ ] tabindex can be applied to any html element 
- [ ] it takes a range of numeric values

if `tabindex="-1"`

- [ ] not in the natural tab order
- [ ] can be programmatically with `focus()` method

When new content is displayed, you can call its focus method which will direct 
future keyboard events to it.

```javascript
  document.querySelector('#modal').focus()
```

if `tabindex="0"`

- [ ] in the natural tab order
- [ ] can be programmatically focused

```html
  <div id="dropdown" tabindex="0">Settings</div>
```
In the preceding example we added a `tabindex="0"` on a div element.
This element now gets focused and has access to keyboardf events.

if `tabindex="greater > 0"` 

- [ ] in the natural tab order
- [ ] jumped to the front of the tab listner 
- [ ] anti-pattern!

Any `tabindex > 0` is considered bad practice. Can make it confusing for screen reader users, ideally
if you want to put something earlier in the tab order, the best bet is to move it earlier in the dom.

[tabindex on MDN](https://linkwww.w3.org/TR/html5/editing.html#sequential-focus-navigation-and-the-tabindex-attribute)