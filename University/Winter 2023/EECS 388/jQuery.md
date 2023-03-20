https://learn.jquery.com/using-jquery-core/

https://www.w3schools.com/jquery/

### Basis

#### jQuery Syntax

The jQuery syntax is tailor-mode for *selecting* HTML elements and performing some *action* on the elements. `$(selector).action();`

##### The Document Read Event

```js
$(document).ready(funciton(){
	// your code here
});

$(funciton(){
  // your code here
});
```

Prevent any jQuery code from running before the document is finished loading.

#### jQuery Selectors

* `$(this).hide()` - hides the current element.
* `$("p").hide()` - hides all `<p>` elements (tag).
* `$(".test").hide()` - hides all elements with *class*="test".
* `$("#test").hide()` - hides the element with *id*="test".
* `$("p:first")` - selects the first `<p>` element

#### jQuery Event Methods

**Common Events**

* Mouse: `click`, `dblclick`, `mouseenter`, `mouseleave`, `mousedown`, `mouseup`, `hover`
* Keyboard: `keypress`, `keydown`, `keyup`
* Form: submit, change, focus, blur
* Window: load, resize, scroll, unload

E.g. assign a click event to all paragraphs on a page: `$("p").click(function(){ $(this).hide(); });`. The code inside the document ready is executed once the document is finish loading.

**The on Method**

The `on()` methods attaches one or more handlers for the selected elements.

```js
$("p").on("click", function(){  
  $(this).hide();  
});

$("p").on({  
  mouseenter: function(){  
    $(this).css("background-color", "lightgray");  
  },  
  mouseleave: function(){  
    $(this).css("background-color", "lightblue");  
  },  
  click: function(){  
    $(this).css("background-color", "yellow");  
  }  
});
```

### jQuery HTML

* `text()` - sets or returns the text content of selected elements
* `html()` - sets or returns the content of selected elements in html format
* `val()` - sets or returns the value of form fields



### jQuery AJAX

**AJAX (asynchronous JS and XML)** is the art of exchanging data with a server, updating part of the web page without reloading the whole page.

The `load()` method loads data form a server and puts the returned data into the selected element.

##### GET

The `$.get(URL, callback)` method requests data from the server with an HTTP GET request. The optional callback parameter is the name of a function to be executed after the request succeed.
