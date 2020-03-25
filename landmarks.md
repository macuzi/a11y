# Creating Landmarks (HTML5, ARIA)

As a general rule, it is usually best to use native HTML elements rather than their ARIA equivalents whenever possible.
That said, the end result is effectively the same reason for screen reader users, so either one can be used.
Adding ARIA roles to existing markup can sometimes be easier to manage, because adding new HTML elements can sometimes
alter the visual apperance ( and require some extra CSS work ), whereas ARIA roles do not.

| HTML5 | ARIA Role | Listed Among Landmarks by Most Screen Readers | 
| :-------------: |:-------------:|:-------------:|    
| header | role="banner" | ✅yes |
|nav|role="navigation"| ✅yes |
|n/a|role="search"| ✅yes |
|main|role="main"| ✅yes |   
|footer|role="contentinfo"|✅yes|
|aside|role="complementary"|✅yes|
|section|role="region"| Mixed Support: Most screen readers list it only if given via `aria-label` or `aria-labelledby`. Note that if each section has a heading, screen reader users will be able to navigate by heading, so it is usually not necessary to give each section or region a name, in fact it can be a bad idea to supply too many landmarks in a page, because its slows down the user's ability to sort through them all.|
|article| role="article" | Mixed Support: JAWS list it, but other screen readers do not |
| form | role="form" | Mixed Support: Screen readers list form only if marked as role="form" |

> The most useful of these for most websites are header/banner, nav/navigation, main, and footer/contentinfo. Others may be used as well, but they are not as widely applicable across most websites.

# Best Practices for Landmarks

### ✅Good Example: All text SHOULD be contained with a landmark region.
```html
  <header>
    <div>This is the header.</div>
  </header>
  <nav>
    <div>This is the navigation.</div>
  </nav>
  <main>
    <div>This is the main content.</div>
    <section>
      <div>This is a section</div>
    </section>
    <article>
      <div>This is an article.</div>
    </article>
    <aside>
      <div>This is an aside.</div>
    </aside>
  </main>
  <footer>
    <div>This is a footer</div>
  </footer>
```

### Bad Example: No landmarks used. 🚫

```html
  <div>This is the header.</div>
  <div>This is the navigation.</div>
  <div>This is the main content.</div>
  <div>This is a section.</div>
  <div>This is an article.</div>
  <div>This is an aside.</div>
  <div>This is the footer.</div>
```

## Multiple instances of the same type of landmark SHOULD be distinguishable by different labels (aria-label or aria-labelledby).

### ✅Good Example: Landmarks of the same type are distinguished by aria-label

```html
  <nav aria-label="Main Menu">
    <ul>
      <li><a href="index.html">Home</a></li>
      <li><a href="products.html">Products</a></li>
      <li><a href="services.html">Services</a></li>
    </ul>
  </nav>
  <main>
    <div>[...other content...]</div>
      <nav aria-label="Products list">
        <ul>
          <li><a href="1.html">Product 1</a></li>
          <li><a href="2.html">Product 2</a></li>
          <li><a href="3.html">Product 3</a></li>
          <li><a href="4.html">Product 4</a></li>
        </ul> 
      </nav>
    </div>
    <div>[...other content...]</div>
  </main>
  <footer>
    <nav aria-label="Corporate and legal info">
      <ul>
          <li><a href="about.html">About us</a></li>
          <li><a href="copyright.html">Copyright notice</a></li>
          <li><a href="terms.html">Terms of use</a></li>
          <li><a href="contact.html">Contact us</a></li>
      </ul> 
    </nav>
  </footer>
```

### 🚫Bad Example
```html
  <nav>
    <ul>
      <li><a href="index.html">Home</a></li>
      <li><a href="products.html">Products</a></li>
      <li><a href="services.html">Services</a></li>
    </ul>
  </nav>
  <main>
    <div>[...other content...]</div>
      <nav>
        <ul>
          <li><a href="1.html">Product 1</a></li>
          <li><a href="2.html">Product 2</a></li>
          <li><a href="3.html">Product 3</a></li>
          <li><a href="4.html">Product 4</a></li>
        </ul> 
      </nav>
    </div>
    <div>[...other content...]</div>
  </main>
  <footer>
    <nav>
      <ul>
        <li><a href="about.html">About us</a></li>
        <li><a href="copyright.html">Copyright notice</a></li>
        <li><a href="terms.html">Terms of use</a></li>
        <li><a href="contact.html">Contact us</a></li>
      </ul> 
    </nav>
  </footer>
```

## A page SHOULD NOT contain more than one instance of each of the following landmarks: banner, main, contentinfo

### ✅Good Example: One Instance Per Page of Certain Landmarks 

```html
  <div role="banner">Visit Your Local Zoo!</div>
  <div role="main">
    <h1>The Nature of the Zoo</h1>
    <article>
      <h2>In the Zoo: From Wild to Tamed</h2>
      <p>What you see in the zoo are examples of 
      inborn traits left undeveloped. [...]</p>
    </article>
    <article>
    <h2>Feeding Frenzy: The Impact of Cohabitation</h2>
      <p>Some animals naturally group together with their own kind, 
      but others stake solitary claim on their territory. 
      Even those that typically live harmoniously together can 
      have bouts with romantic rivals, which can potentially 
      escalate in the more confined setting of a zoo. [...]</p>
    </article>
  </div>
  <div role="contentinfo">
    <p>Brought to you by North American Zoo Partnership</p>
  </div>
```

### 🚫Bad Example: Multiple Instances of banner, main or contentinfo Landmarks
 In this example there are two types of landmarks (<main> and role="contentinfo") which should be used only once have been used multiple times on the same page.


```html
  <header>Visit Your Local Zoo!</header>
  <h1>The Nature of the Zoo</h1>
  <main class="article">
    <h2>In the Zoo: From Wild to Tamed</h2>
    <p>What you see in the zoo are examples of 
    inborn traits left undeveloped. [...]</p>
    <div role="contentinfo">
      <p>[...information about this article...]</p>
    </div>
  </main>
  <main class="article">
  <h2>Feeding Frenzy: The Impact of Cohabitation</h2>
    <p>Some animals naturally group together with their own kind, 
    but others stake solitary claim on their territory. 
    Even those that typically live harmoniously together can 
    have bouts with romantic rivals, which can potentially 
    escalate in the more confined setting of a zoo. [...]</p>
    <div role="contentinfo">
      <p>[...information about this article...]</p>
    </div>
  </main>
  <footer>
    <p>Brought to you by North American Zoo Partnership</p>
  </footer>
```

Long story short 1 unique landmark per page and keep landmarks to a minimum 