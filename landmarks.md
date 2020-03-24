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

## All text SHOULD be contained with a landmark region.
✅Good Example
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