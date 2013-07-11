Pragmatic jQuery Style
========

## Why jQuery Style?
* 80% of the lifetime cost of a piece of software goes to maintenance.
* Hardly any software is maintained for its whole life by the original author.
* Code conventions improve the readability of the software, allowing engineers to understand new code more quickly and thoroughly.
* If you ship your source code as a product, you need to make sure it is as well packaged and clean as any other product you create.

## Plugin Name
   lowercase, avoid multiple words.
```
// bad
jquery.string.js
jquery.observer.js
jquery.promise.js

// good
string.js
observer.js
promise.js
```

## Plugin Skeleton

```js
// pluginName.js

(function (factory) {
    if (typeof define === 'function') {
        define(['$'], factory);
    } else {
        factory($);
    }
})(function ($) {
     'use strict';
     var pluginName = 'pluginName';
    // balabala...
})
```

There are some kinds of jQuery plugins:
  - `$.pluginName( { name:"value" } );` Attach to $ because they did not want to create a global, but just indicate it is jQuery-related functionality. They do not do anything with node lists though. 
    ```js
    // pluginName.js

    (function (factory) {
        if (typeof define === 'function') {
        ...
    })(function ($) {
        'use strict';
        var pluginName = 'defaultPluginName';

        function plugin( options ){
            // ...
        } 

        $[pluginName] = plugin;
    })
    ```
  - `$(".foo").pluginName( { name:"value" } );` Attach to $.fn because they want to participate in the chained API style when operating with a node list. 
    ```js
    // pluginName.js

    (function (factory) {
        if (typeof define === 'function') {
        ...
    })(function ($) {
        'use strict';
        var pluginName = 'defaultPluginName';
        
        function plugin( element, options ) {
            // ...
        }

        $.fn[pluginName] = function ( options ) {
            return this.each(function () {
                // ...
            })
                
        });
    })
    ```

  - `$.ajax( { dataType:"jsonpi" } );`  custom jQuery ajax request http://api.jquery.com/extending-ajax/
    ```js
    // pluginName.js

    (function (factory) {
        if (typeof define === 'function') {
        ...
    })(function ($) {
        'use strict';
        var pluginName = 'jsonpi';
        
        $.ajaxTransport( pluginName , function(opts, originalOptions, jqXHR) {
            // ...
        });
    })
    ```
  - `$( 'div:inline' );`  custom jQuery selector
    ```js
    // pluginName.js

    (function (factory) {
        if (typeof define === 'function') {
        ...
    })(function ($) {
        'use strict';
        var pluginName = 'defaultPluginName';
        
        $.expr[':'][pluginName] = function(element) {
            return $(element).css('display') === 'inline';
        };

        $(':inline');  // Selects ALL inline elements
        $('a:inline'); // Selects ALL inline anchors
    })
    ```
  - `$( '#element' ).on('cumstomEvent', function(){});`  custom jQuery Event
    ```js
    // pluginName.js

    (function (factory) {
        if (typeof define === 'function') {
        ...
    })(function ($) {
        'use strict';
        var eventName = 'customEventName';
        
        $.event.special[eventName] = {
            // called when the event is bound
            setup: function(data, namespaces) {
                var $this = $(this);
            },
            // called when event is unbound
            teardown: function(namespaces) {
                var $this = $(this);
            },
            // called when event is dispatched
            handler: function(event) {
                var $this = $(this);
            },
            // similar to setup, but is called for each event being bound
            add: function(event) {
                var $this = $(this);
            },
            // similar to teardown, but is called for each event being unbound
            remove: function(event) {
                var $this = $(this);
            }
        };

        // bind custom event
        $("#element").on("customEventName.myNamespace", function(evt) {});
        // remove all events under the myNamespace namespace
        $("#element").off(".myNamespace");

    })
    ```
  - `$( 'textarea.foo' ).val();`  custom form element value hook
    ```js
    // valHooks.js

    (function (factory) {
        if (typeof define === 'function') {
        ...
    })(function ($) {
        'use strict';

        $.valHooks.textarea = {
            get: function( elem ) {
                return elem.value.replace( /\r?\n/g, "\r\n" );
            }
        };

    })
    ```

## Indentation
   The unit of indentation is four spaces.
```js
arr.forEach(function(val, key){
....// indentation with four spaces
});
```
  
## Variable Name
   lowerCamelCase
```js
var thisIsMyVal = "foo";
```

## Method Name
   lowerCamelCase
```js
function thisIsMyFunction() {
    // balabala
}
```

### Why lowerCamelCase?

We know that when you write Ruby or Python, you use under_scored method names and UpperCamelCase class names. But JavaScript isn't Ruby or Python.

Consider:

* In browsers, all methods are lowerCamelCase.
* In node.js's standard library, all methods are lowerCamelCase.
* In commonjs, all methods are lowerCamelCase.
* In jQuery, all methods are lowerCamelCase.
* In MooTools, all methods are lowerCamelCase.
* In Prototype, all method are lowerCamelCase.
* In YUI, all method are lowerCamelCase.
* In JavaScript: The Good Parts, all methods are lowerCamelCase.

We not saying you must write JavaScript a certain way. Do what makes you the most comfortable, write code that is fun to write. But if you're trying to figure out what everyone else is doing, they're doing lowerCamelCase.

## Constant   
    
```js
var LOCALHOST = "http://localhost";
var REMORT_PORT = "8080";
```

## Class Name
   UpperCamelCase
```js
var Greeter = Class.extend({
    name: null,
 
    _constructor: function(name) {
        this.name = name;
    },
 
    greet: function() {
        alert('Good Morning ' + this.name);
    }
});
```

## Variables Declare

```js
var a = "alpha";
var b = "beta";
var c = "chi";
```

### Why?
```js
var a = "alpha",
    b = "beta" // <-- note the forgotten comma
    c = "chi"; // <-- and now c is global
```
	

## Leading Commas

```js

var hero = {
    firstName: 'Bob'
  , lastName: 'Parr'
  , heroName: 'Mr. Incredible'
  , superPower: 'strength'
};
```
### Why?
```
var hero = {
  firstName: 'Bob',
  lastName: 'Parr',
  heroName: 'Mr. Incredible', // <-- forgot remove comma, when comment the last item
  //superPower: 'strength'    
};
```


## jQuery Object
Prefix jQuery object variables with a `$`.
The dollar notation on all jQuery-related variables helps us easily distinguish jQuery variables from standard JavaScript variables at a glance.

```js
// bad
var sidebar = $('.sidebar');
var that = $(this);

// good
var $sidebar = $('.sidebar');
var $this = $(this);
```

## Method Chains
Use indentation when making long method chains, and avoid more than 6 methods chained. 
Less method chains, more friendly debugging. 

```js
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// good
$('#items')
.find('.selected')
  .highlight()
  .end()
.find('.open')
  .updateCount();
```

## Determine jQuery Object
Determine if an object is a jQuery object

```js
// bad (fast)
if( obj.jquery ){}

// good (slow)
if( obj instanceof jQuery){}
```

## Document Ready

```js
// bad
$(function() {
    // Handler for .ready() called.
});

// good
$(document).ready(function() {
    // Handler for .ready() called.
});
```

## Event Bind 
```js
// bad
$( "#members li a" ).bind( "click", function( e ) {} ); 

// good
$( "#members li a" ).on( "click", function( e ) {} ); 

// good
$( "#members li a" ).click( function( e ) {} ); 

```

## Event Live 

```js
// bad, .live() deprecated jQuery 1.7, removed jQuery 1.9
$( "#members li a" ).live( "click", function( e ) {} );

// good
$( document ).on( "click", "#members li a", function( e ) {} ); 

```

## Event Delegate 
```js
// bad, as of jQuery 1.7, .delegate() has been superseded by the .on() method
$( "#members" ).delegate( "li a", "click", function( e ) {} );

// good
$( "#members" ).on( "click", "li a", function( e ) {} ); 
```
## Event Prevent
```js
// bad
$(".btn").click(function(event){
    // @more: http://fuelyourcoding.com/jquery-events-stop-misusing-return-false/
    return false;
});

// good
$(".btn").click(function(event){
    event.preventDefault();
});

// good
$(".btn").click(function(event){
    event.preventDefault();
    event.stopImmediatePropagation()
});

// good
$(".btn").click(function(event){
    event.stopPropagation();
    event.preventDefault();
    event.stopImmediatePropagation();
});

```

## Element Create
```js
// bad
$('<a/>')  
  .attr({  
    id : 'someId',  
    className : 'someClass',  
    href : 'somePath.html'  
  });

// good
$('<a/>', {  
    id : 'someId',  
    className : 'someClass',  
    href : 'somePath.html'  
}); 

```

## Element Exists

```js
// bad
if ($('#myElement')[0]) {  
    // balabala...
}  

// good
if ($('#myElement').length) {  
    // balabala... 
}
```

## Element Access
```
// bad
$('#myElement').click(funtion(){
    var id = $(this).attr('id');
})

// good
$('#myElement').click(funtion(){
    var id = this.id;
})
```

## Number Compare 
```
var a = '1';
var b = 1;

// bad
parseInt(a) === b

// good, implicit coercion
+a === b

// better, expressive conversion, and conversion over coercion
Number(a) === b
```

## Array Traversal
```
var array = new Array(100);
// bad (faster)
for (var i=0,l=array.length; i<l; i++) {
    console.log(i, array[i]);    
}
  
// good (slow)
$.each (array, function (i, value) {  
    console.log(i, value);  
});

// better (fast), use es5-shim(https://github.com/kriskowal/es5-shim) for legacy JavaScript engines
array.forEach(function(value, i){
    console.log(i, value);
});
 
```

## Async Programing
```js
// bad
$.ajax({
    url: "cgi/example"
    , success: function() { alert("success"); }
    , error: function() { alert("error"); }
    , complete: function() { alert("complete"); }
});

// good
var jqxhr = $.ajax( "cgi/example" )
    .done(function() { alert("success"); })
    .fail(function() { alert("error"); })
    .always(function() { alert("complete"); });
```
## Type Determine
```
var obj = {};
var arr = [];
var reg = /test/g;
var fn = function(){};

// bad
typeof obj === 'object'
typeof arr === 'array'
typeof fn === 'function'
reg instanceof RegExp

// good 
// http://api.jquery.com/jQuery.type/
$.type(obj) === 'object'
$.type(arr) === 'array'
$.type(reg) === 'regexp'
$.type(fn) === 'function'

// better
$.isPlainObject(obj)
$.isArray(arr)
$.isFunction(fn)
```

## Ajax Retry
```
// not bad, inspried by http://zeroedandnoughted.com/defensive-ajax-and-ajax-retries-in-jquery/
$.ajax({
    url : 'path/to/url',
    type : 'get',
    data :  {name : 'value'},
    dataType : 'json',
    timeout : 25000,
    tryCount : 0,
    retryLimit : 3,
    success : function(json) {
        //do something
    },
    error : function(xhr, textStatus, errorThrown ) {
        if ($.inArray(textStatus, ['timeout', 'abort', 'error']) > -1) {
            this.tryCount++;
            if (this.tryCount <= this.retryLimit) {
                //try again
                return $.ajax(this);
            }
            return alert('Oops! There was a problem, please try again later.');
        }
    }
});

// better, inspried by https://github.com/mberkom/jQuery.retryAjax
$.retryAjax = function (ajaxParams) {
	var errorCallback;
	ajaxParams.tryCount = ajaxParams.tryCount > 0 ? ajaxParams.tryCount : 0;
	ajaxParams.retryLimit = ajaxParams.retryLimit > 0 : ajaxParams.retryLimit : 2;
	// Custom flag for disabling some jQuery global Ajax event handlers for a request
	ajaxParams.suppressErrors = true;
	
	if (ajaxParams.error) {
	    errorCallback = ajaxParams.error;
	    ajaxParams.error = null;
	} else {
	    errorCallback = $.noop;
	}
	
	ajaxParams.complete = function (jqXHR, textStatus) {
	    if ($.inArray(textStatus, ['timeout', 'abort', 'error']) > -1) {
	        this.tryCount++;
	        if (this.tryCount <= this.retryLimit) {
	            // fire error handling on the last try
	            if (this.tryCount === this.retryLimit) {
	                this.error = errorCallback;
	                this.suppressErrors = null;
	            }
	            //try again
	            $.ajax(this);
	            return true;
	        }
	
	        alert('Oops! There was a problem, please try again later.');
	        return true;
	    }
	};

	$.ajax(ajaxParams);
};

$.retryAjax({
	url : 'path/to/url',
    type : 'get',
    data :  {name : 'value'},
    dataType : 'json',
    timeout : 25000,
    success : function(json) {
        //do something
    }
})
```

## Performance
  - Cache jQuery lookups
    ```js
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > .ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` with scoped jQuery object queries.
    ```js
    // bad
    $('.sidebar', 'ul').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good (slower)
    $sidebar.find('ul');

    // good (faster)
    $($sidebar[0]).find('ul');
    ```

# Ref
* jQuery Core Style Guide - http://docs.jquery.com/JQuery_Core_Style_Guidelines
* Airbnb JavaScript Style Guide - https://github.com/airbnb/javascript
* Idiomatic.js - https://github.com/rwldrn/idiomatic.js/
* NPM's "funny" coding style - https://npmjs.org/doc/coding-style.html
