# Design Considerations for Blindness

| Design consderation | Why?     
| ------------- |:-------------:|       
| All content must be presented in text or via text equivalent | Screen Readers can't read non-content, but it can read the alt=text you provide. | 
| All functionality must be available using the keyboard. | Even though most blind users can use a mouse, it doesn't do much good because they can't see where the moust pointer is. It's more effective for them to navigate by keyboard. |
| The content must use markup with good structure and semantics | Screen reader users often pull up lists of headings landmarks and other semantic elements to help understand what is on the page. They can also navigate these elements.
| All custom controls, must have correct name / label, role and value. They also must change when appropriate (aria-expanded="false" changes to aria-expanded="true" after activating the button) | Unlike native HTML elements, custom controls have no semantic parts natively, so screen readers can't tell users what the widget is and can't update users on the properties of that widget unless you supply that information via ARIA names, roles, states, and properties. |



# Landmarks

As a general rule, it is usually best to use native HTML elements rather than their ARIA equivalents whenever possible.
That said, the end result is effectively the same reason for screen reader users, so either one can be used.
Adding ARIA roles to existing markup can sometimes be easier to manage, because adding new HTML elements can sometimes
alter the visual apperance ( and require some extra CSS work ), whereas ARIA roles do not.

| HTML5 | ARIA Role | Listed Among Landmarks by Most Screen Readers | 
| :-------------: |:-------------:|:-------------:|    
| <header> | role="banner" | ✅yes |
|<nav>|role="navigation"| ✅yes |
|n/a|role="search"| ✅yes |
|<main>|role="main"| ✅yes |
|<footer>|role="contentinfo"|✅yes|
|<aside>|role="complementary"|✅yes|
|<section>|role="region"| Mixed Support: Most screen readers list it only if given via `aria-label` or `aria-labelledby`. Note that if each section has a heading, screen reader users will be able to navigate by heading, so it is usually not necessary to give each section or region a name, in fact it can be a bad idea to supply too many landmarks in a page, because its slows down the user's ability to sort through them all.
