# Design Considerations for Blindness

| Design consderation | Why?     
| ------------- |:-------------:|       
| All content must be presented in text or via text equivalent | Screen Readers can't read non-content, but it can read the alt=text you provide. | 
| All functionality must be available using the keyboard. | Even though most blind users can use a mouse, it doesn't do much good because they can't see where the moust pointer is. It's more effective for them to navigate by keyboard. |
| The content must use markup with good structure and semantics | Screen reader users often pull up lists of headings landmarks and other semantic elements to help understand what is on the page. They can also navigate these elements.
| All custom controls, must have correct name / label, role and value. They also must change when appropriate (aria-expanded="false" changes to aria-expanded="true" after activating the button) | Unlike native HTML elements, custom controls have no semantic parts natively, so screen readers can't tell users what the widget is and can't update users on the properties of that widget unless you supply that information via ARIA names, roles, states, and properties. |

