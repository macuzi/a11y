# a11y notes
A cheat sheet and notes for accessibility 

# Focus
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
    padding: .8rem; /* would be 8px if html font size 62.5% */
    z-index: 100;
  }

  .skip__link:focus {
    top: 0; /* moves the element back on screen */
  }
```
[Removing Headaches from Focus Management](https://developers.google.com/web/updates/2016/03/focus-start-point?hl=en)

A great example by the folks at [Gatsby](https://www.gatsbyjs.org)
> When the page loads hit tab and you'll see the skip link pop-up

## ARIA

There are times when an original layout in HTML won't cut ✂️ it. To handle special situations like this
we can use `ARIA`. `ARIA` works by allowing us to specify attributes on elements, which modify the way 
those elements are translated into the accessibility tree 🌳. 

For example if you create a plain checkbox, a screen reader will annouce that it's a checkbox, its label, and its state.

```html
  <label>
    <input type="checkbox">
    Sign up for our newsletter
  </label>
```

If you have to style your own custom `div` checkbox. The below example when annouced 
by a screen reader will tell you the text this checkbox, but it won't tell you the state.

```html
  <!-- custom checkbox -->
  <div class="checkbox checked">
    Sign up for our newsletter
  </div>
```

Using ARIA you can tell the screen reader that this is indeed a checkbox ✅
Adding `role` and `aria-checked` attributes causes the node in the accessibility tree 
to have the desired role and state without changing its apperance.
```html
  <!-- Now the screen reader will annouce to the user this is a checkbox -->
  <div class="checkbox checked" role="checkbox" aria-checked="true">
    Sign up for our newsletter
  </div>
```

**Keep in mind that this is the ONLY thing ARIA changes, it's good for modifying the accessibility tree however, it doesn't**

- [ ] modify element apperance
- [ ] modify element behavior
- [ ] add focusability 
- [ ] add keyboard event handling

### Aria Live

`aria-live` allows developers to mark a part of the page as *live*. One practical 
example might be a status message which appears as a result of either a user action
or external event. If this message is important enough to grab a sighted user's attention, 
we can direct assistive technology users attention by setting an `aria-live` attribute on it.

```html
  <div class="alertbar" aria-live="assertive">
    Could not connect
  </div>
```

`aria-live` has 3 allowable values

| Values       | Description           
| ------------- |:-------------:| 
| off (default):| Updates to the region will not be presented to the user unless the assistive technology is currently focused on that region | 
| polite:      | Assistive technologies **SHOULD** announce updates at the next graceful opportunity, such as the end of speaking the current sentence or when the user pauses typing.     |   
| assertive:  | This information holds the highest priority and assistive technologies **SHOULD** notify the user immediately. Because an interpution may disorient users or cause them to not complete their task, authors **SHOULD NOT** use assertive unless the interpution is imperative |    

There is a lot to cover with **ARIA**, but in a nutshell only use it if you need it. 
Semantic HTML will get you a very long way, and can save you a lot of effort. 

## Objective accessibility guidelines

Many aspects of accessibilty are objectively testable. Here are a few examples 

- [ ] All images *must* have an `alt` attribute.
- [ ] All form input elements *must* have a label.
- [ ] The header cells of a data table *must* be marked as header cells using `<th>`.
- [ ] The page *must* have a `title`.
- [ ] Color cannot be used as only visual means of converying information.

These things are easily verifiable by an objective standard. The web page either passes or fails on these points. Any person 
with accessibility training can accurately assess whether a page passes, or fails, and it is possible to write softwares algorithms
to check for some (though not all) of the objective criteria.

### Accessibility best practices that aren't so easily testable include things like:

- [ ] Using visual cues to focus the user's attention on the main purpose of the web page. 
- [ ] Ensuring the font is easily readable (How do you define easily?)
- [ ] Minimize coginitve skills required to use the website.

## Semantic Structure and Navigation

Semantic structure is the foundation of accessibile markup.Screen readers rely on the meaning of HTML elements and attributes 
to convey information to blind users. It is through the semanntic markup that browsers are able to parse accessibility cues 
and information to through the accessibility API and pass that information on to users, through assistive technologies, such as 
screen readers.

### Page Title 

*The Page `<title>` is the first thing that screen reader users hear*

The `<title>` of the page is critical, because it's the first thing a screen reader user hears. Blind users cannot 
glace quickly at the content of a web page to see what the page is about, so they rely on the page title to give them this information.
Web pages without titles waste the time of screen reader users, because the force users to navigate through the page -- past the main menu 
into the content -- to find out what the page is about.

✅Good Example #1 => *react-document-title*

```javascript
  import React from 'react';
  import DocumentTitle from 'react-document-title';

  function HomePage() {
    return (
      <DocumentTitle title="Home Page"> // 👈🏾This will allow the specific component to have it's own page title.
        <>
          // ... 💼👔business as usual with regular functional components
        </>
      </DocumentTitle>
    )
  }
```

✅Good Example #2 => (In React if you can't use a 3rd party library), `componentDidMount()` will do the job.

```javascript
  componentDidMount() {
    document.title = 'I am awesome' // ... pure react out the box 📦
  }
```

The page `<title>` **must** be present and contain text

Screen reader users rely only on text to convey information. If you leave off the title, screen reader users have to browse the page 
and infer the identity and purpose from the content, whixh is highly inconvenient.

**The page <title> must be updated when the web address changes** 

Any time the web address changes, whether due to a true load event, due to an AJAX call, or a JavaScript event,
the page `<title>` must change to reflect the new content. Otherwise, the page title wil become out of sync with the content,
which can lead to confusion, espically if the user arrives at the page via a link sent to them or saved in a bookmark.

Page titles improve SEO as well.

Search engines and site search tools also use the page title when displaying search results. Effective titles help
users quickly sift through the search results to identify the content they need. In this sense, good page titles are an 
important component of SEO in addition to accessibility. 


**If a page is the result of a user action or scripted change of context, the text of the `<title>` should describe the result or change of context to that user**

✅Good Example: Title with Search Text 

The title includes text of the search.

```html
  <title>"Warranty" | Search Results</title>
```

**Titles should be concise**

✅Good example: Concise Page Title

There isn't a technical limit for a page title, nor is there a formal restrcition on `<title>` length 
on accessibility guidelines, but best practice is to keep it as short as possible, without sacrificing
accuracy or informativeness.

```html
  <title>Football Cleats</title>
```

🚫Bad Example: Page Title Used for Keywords Instead of a True Title 

Cramming a list of keywords into a page title makes the page title too long. People using screen readers
will get impatient listening to the long title as they want to hear other important information about the page

```html
  <title>Mortgages Inc. - Mortgage services, best mortgages in New York, low rates, low API, easy financing, variable rates, fixed rate mortgages</title>
```

**The page <title> SHOULD match (or be very similar to) the top heading in the main content.**

