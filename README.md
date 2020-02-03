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

### Skip Links

On most websites the main content is not the first thing in the DOM. 
We often begin with navigation, sublists, sidebars, etc. This means keyboard 
and screen reader users must navigate through all of this content before they get to 
the meat of the page. We can create a hidden link that allows keyboard and switch device
users the ability to jump straight into our page content. These links are often referred to 
as *skip links*. Below is an exmaple of how we can implement this feature.

- [ ] we make an `a` tag that refers to the ID of another element we have on the page. In this case it wil be 
`#main__content`. Then we make a class `skip__link` that we can refer in CSS later

```html
  <a href="#main__content" class="skip__link">Skip to main content</a>
```

- [ ] We want the skip link to apprear early in the DOM, so we can put it before our navigation.
```html
  <a href="#main__content" class="skip__link">Skip to main content</a>
  <nav>
    <!-- navbar content -->
  </nav>
  <main id="main__content">
    <!-- main content, now the anchor tag and main are connected  -->
  </main>
```  

In newer versions of Chrome and Firefox 🦊, the preceding exmaple will allow you 
to move down to the main-element when the skip link is pressed.

In older browsers that don't move focus when named anchors are clicked, here is a simple solution.
```html
  <main id="main__content" tabIndex="-1">
    <!-- main content, now the anchor tag and main are connected  -->
    <!-- added a tabIndex of -1  -->
  </main>
```

- [ ] Over in your CSS, here is how you can style your `skip__link` 

```css
  .skip__link {
    position: absolute; /* makes it appear in the top left corner of the screen */
    top: -40px; /* initally position it off screen */
    left: 0;
    background: #0d55ff; 
    color: white;
    padding: .8rem; /* woudl be 8px if html font size 62.5% */
    z-index: 100;
  }

  .skip__link:focus {
    top: 0; /* moves the element back on screen */
  }
```
[Removing Headaches from Focus Management](https://developers.google.com/web/updates/2016/03/focus-start-point?hl=en)

A great example by the folks at [Gatsby](https://www.gatsbyjs.org)
> When the page loads hit tab and you'll see the skip link pop-up



