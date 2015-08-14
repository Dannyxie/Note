# Node of DOM Scripting


---

##Chapter 2 JavaScript Syntax


-  Placing the `<script>` tag at the end of the document lets the browser load the page faster (we'll  discuss this more in Chapter 5, "Best Practices")..
-  The `<script>` tags in the preceding examples don't contain the traditional type="text/javascript" attribute. This attribute is considered unnecessary as the script is already assumed to be JavaScript.
-  If there are any errors in the code written in a compiled language, those errors will pop up when the code is compiled. In the case of an interpreted language, errors won't become apparent until the interpreter executes the code.
-  You don't have to declare variables separately. You can declare multiple variables at the same time:var mood = "happy", age = 33;
-  Array:Specifying the number of elements is optional. You can just declare an array with an unspecified number of elements: var beatles = Array();
-   arrays can hold other arrays! Any element of an array can contain an array as its value:var lennon = [ "John", 1940, false ];var beatles = [];beatles[0] = lennon;
-  Associative arrays:The index can be a string


##Chapter 3:The Document Object Model

-  Five very handy DOM methods: getElementById, getElementsByTagName,getElementsByClassName, getAttribute, and setAttribute

##Chapter 4:A JAVASCRIPT IMAGE GALLERY

-  The childNodes property returns an array containing all types of nodes, not just element nodes. It will bring back all the attribute nodes and text nodes as well.
-  The text within the paragraph is a different node. This text is the first child node of the paragraph.
Therefore, you want to retrieve the nodeValue of this child node.
This alert statement will give you the value you’re looking for:
alert(Node.childNodes[0].nodeValue)
-  node.firstChild  = node.childNodes[0]
-  node.lastChild   = node.childNodes[nodes.childNodes.length-1]


nodeType value:
-  Element: 1.
-  Attribute: 2.
-  Text: 3.



##Chapter 6:

```javascript
 function addLoadEvent(func) {
    var oldonload = window.onload;
    if (typeof window.onload != 'function') {
        window.onload = func;
    } else {
    window.onload = function() {
        oldonload();
        func();
    }
}
```
-  if element does not have attribute which getAttribute() function specificed ,it will returns `null`：         ```element.getAttribute(AttributeNameNotExist) == null```
-  Ternary operator:    variable = condition ? if true : if false;
-  in HTML documents, nodeName always a returns an uppercase value, even if the element is lowercase in the markup



##Chapter 7:Creating Markup on the Fly

- DOM method: createElement, createTextNode, appendChild, insertBefore
```javascript
function insertAfter(newElement,targetElement) {
    var parent = targetElement.parentNode;
    if (parent.lastChild == targetElement) {
     parent.appendChild(newElement);
    } else {
       parent.insertBefore(newElement,targetElement.nextSibling);
    }
}
```
-  createElement:create some element node.
-  appendChild:insert into the last place, included as a child
- insertBefore  syntax:`parentElement.insertBefore(newElement,targetElement)`  and cooperate with parentNode,instance: `a.parentNode.insertBefore(newNode,a)`

```javascript
function getHTTPObject(){
    if(typeof XMLHttpRequest == undefined){
        XMLHttpRequest = function(){
            try{    return new ActiveXObject("Msxml2.XMLHttp.6.0");} catch(e){}
            try{    return new ActiceXObject("Msxml2.XMLHttp.3.0");} catch(e){}
            try{    return new ActiveXObject("Msxml2.XMLHttp");} catch(e){}
        }
    }
    return new XMLHttpRequest();
}
```


readyState:

    0:uninitialized
    1:loading
    2:loaded
    3:interative
    4:complete


##Chapter 9:CSS-DOM

-  Every element node has style property
-  When you want to reference a style property that uses a minus sign, the DOM requires you to use camel-casing. The CSS property font-family becomes the DOM property fontFamily:     `element.style.fontFamily`
- In some browsers, the color property will be returned in RGB
-  The style object doesn't include stylistic information that has been declared in an external style
sheet. It also doesn't include stylistic information that has been declared in the head of a document.

```javascript
addClass(element, value){
    if(!element.className){
        element.className = value;
    }else {
        element.className + = " " + value;
    }
}
```

##Chapter 10 An animated slideshow

- Valid value of the `position` property are "static", "fixed", "relative", and "absolute". Elements have position value of "static" by default, which simply means that they appear one after the other in the same sequence as they accur in the markup. The "relative" value is similar. The difference is that relatively positioned elements can be token out of the regular flow of the document by appliying the float property.
- By applying a value of "absolute" to an element's position,you can place the element wherever you want in relation to its container. The container is either the document itself or a parent element with a position of "fixed" or "absolute". It doesn't matter where the element appears in the original markup, because its position will be determined by the properties like `top`, `left`, `right` and `bottom`.

###time
- The JavaScript function `setTimeout` allows us to execute a function after a specified amount of time has elapsed.
- `clearTimeout`: cancel a pending action.
- The JavaScript function parseInt can extract numeric information from a string. If you pass it a
string that begins with a number, it will return the number  

###CSS overflow
- The CSS `overflow` property dictates how content within an element should be displayed when the content is larger that its container element. When an element contains content that is larger that itself, there is an overflow. In that situation, you can clip the content so that only a portion of it is visible. You can also specify whether or not the web browser should display a scrollbars, allowing the user to see the rest of the content.

1. "visible": no clipping occurs. The content overflows and is rendered outside the element.
2. "hidden": A value of "hidden" will cause the excess content to be clipped. Only a protion of the content will be visible.
3. "scroll": The content will be clipped, but the web browser will display scrollbars so that the rest of the content can be viewed.
4. "auto": like "scroll", except that the scrollbars will be displayed only if the content overflows its container element. if there is no overflow, no scrollbars appear.




##Chapter 12 Putting it all together

-  Each  Form object has a property called  elements.length . This value returns the number of form elements contained by a form: form.elements.length
This is different from  childNodes.length , which returns the total number of nodes contained by an element. The elements.length property of a  Form object returns only those elements that are form elements, such as  input elements,  textarea elements, and so on.
-  encode the values to a URL-safe string:  encodeURIComponent()


##DOM Scripting Libraries

JQuery CSS Selector
- $('*') selects all elements.
- $('tag') selects all tag elements for each of the available HTML tags.
- $('tagA tagB') selects all tagB elements that are descendants of tagA elements.
- $('tagA,tagB,tagC') selects all tagA elements, tagB elements, and tagC elements.
- $('#id') and $('tag#id') are ID selectors for any ID or a specific tag-ID combination.
- $('.className') and $('tag.className') are selectors that can be used for any class or tag class combination.
You can also combine selectors such as $('#myList li') or $('ul li a.selectMe') to select any mix of descendant selectors separated by a space.


jQuery also includes the following CSS 2.1 attribute selectors:
-  $('tag[attr]') selects all tag elements that have attribute attr.
-  $('tag[attr=value]') selects all tag elements where value is exactly equal to the attr attribute value.
-  $('tag[attr*=value]') selects all tag elements where value is contained in the attr attribute value.
-  $('tag[attr~=value]') selects all tag elements where value is a word contained in the attr attribute value (separated by spaces).
-  $('tag[attr^=value]') selects all tag elements where value matches the beginning of the attr attribute value.
-  $('tag[attr$=value]') selects all tag elements where value matches the end of the attr attribute value.
-  $('tag[attr|=value]') selects all tag elements where value matches the first part of a hyphenated attr attribute value.
-  $('tag[attr!=value]') selects all tag elements where value is not equal to attr.


You can also use a number of different child and sibling selectors, as follows:
-  $('tagA > tagB') selects all tagB elements that are direct child descendants of
tagA elements.
-  $('tagA + tagB') selects all tagB sibling elements that immediately follow tagA
elements.
-  $('tagA ~ tagB') selects all elements with a preceding sibling tagB element.
Also, you can use several pseudo-class and pseudo-element selectors including these:
-  $('tag:root') selects the tag element that is the root of the document.
-  $('tag:nth-child(n)') selects all tag elements that are the nth children of their
parents, counting from the first one.
-  $('tag:nth-last-child(n)') selects all tag elements that are the nth child of their
parents, counting from the last one.
-  $('tag:nth-of-type(n)') selects all tag elements that are the nth sibling of their
type, counting from the first one.
-  $('tag:nth-last-of-type(n)') selects all tag elements that are the nth sibling of
their type, counting from the last one.
-  $('tag:first-child') selects any tag element that is the first child of its parents.
-  $('tag:last-child') selects any tag element that is the last child of its parent.
-  $('tag:first-of-type') selects any tag element that is the first sibling of its type.
-  $('tag:last-of-type') selects any tag element that is the last sibling of its type.
-  $('tag:only-child') selects any tag element that is an only child of its parent.
-  $('tag:only-of-type') selects any tag element that is the only sibling of its type.
-  $('tag:empty') selects all tag elements that have no children.
-  $('tag:enabled') selects all user interface tag elements that are enabled.
-  $('tag:disabled') selects all user interface tag elements that are disabled.
-  $('tag:checked') selects all user interface tag elements that are checked, such as
check boxes and radio buttons.
-  $('tag:not(s)') selects all tag elements that don't match the selectors.

Other custom selectors in jQuery include these:
-  $('tag:even') selects even-numbered elements from the matched element set—
great for highlighting table rows!
-  $('tag:odd') selects odd-numbered elements from the matched element set.
-  $('tag:eq(0)') and $('tag:nth(0)') select the nth element from the matched
element set, such as the first paragraph on the page.
-  $('tag:gt(n)') selects all matched elements whose index is greater than n.
-  $('tag:lt(n)') selects all matched elements whose index is less than n.
-  $('tag:first') is equivalent to :eq(0).
-  $('tag:last') selects the last matched element.
-  $('tag:parent') selects all elements that have child elements (including text).
-  $('tag:contains('test')') selects all elements that contain the specified text.
-  $('tag:visible') selects all visible elements (this includes items that have a
display property using block or inline or a visibility property using visible, and
that aren't form elements of type hidden).
-  $('tag:hidden') selects all hidden elements (this includes items that have a
display property using none, or a visibility property using hidden, or are form
elements of type hidden).

Finally, jQuery also includes a number of form-specific expressions you can use to access form
elements:
-  :input selects all form elements (input, select, textarea, button).
-  :text selects all text fields (type="text").
-  :password selects all password fields (type="password").
-  :radio selects all radio fields (type="radio").
-  :checkbox selects all checkbox fields (type="checkbox").
-  :submit selects all submit buttons (type="submit").
-  :image selects all form images (type="image").
-  :reset selects all reset buttons (type="reset").
-  :button selects all other buttons (type="button").
