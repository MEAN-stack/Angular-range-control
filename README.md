# Angular-range-control

Demo here:

http://mean-stack.github.io/slidertest.html

# A Reusable Control in AngularJS

I want to share an AngularJS learning experience. My goal is to develop a reusable range slider control. 

The end result probably won't be all that useful - range slider controls are available in HTML5, and have been done before in JavaScript, JQuery, and in AngularJS.

Still, it's a good exercise and a fun way to practice and experiment with Angular. Hopefully, when it's finished, I will have a control which I understand well enough to add further features and customized behaviour.

## Development plan
Since I'm fairly new to Angular, and I don't really know what I'm doing, I'll start with a tiny fragment of HTML and CSS, constructing the control from the ground up, adding features and behaviour as I go.

I'll show my work, bugs and all, as I go along.

## Specification
I am working on an AngularJS project which needs a range slider control.

This image, cropped from a mock-up, is my spec.   

![Screenshot](https://github.com/MEAN-stack/Angular-range-control/blob/master/Screenshot.jpg)

I need to be able to specify the min and max values, and, if the range extends from a negative to a positive value, then I want the negative and positive parts of the control shown in different colours (red and blue in the mock-up).

## Step 1 - Starting point

I had a quick look at the HTML for some existing range slider controls. I soon came up with the following HTML fragment, comprising three nested `div` elements:

```HTML
<div class="slider-target">
  <div class="slider-origin" style="left:60%">
    <div class="slider-handle"></div>
  </div>
</div>
```
The outermost element is the background of the control - the CSS looks like this:

```CSS
.slider-target {
  position: relative;
  width: 90%;
  height: 20px;
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #e0e0e0;
  background-color: #f0f0f0;
}
```

The next element is used to anchor the slider's handle. Its left edge position (I have set it to 60% in an HTML style attribute) is given as a percentage of the target width, overriding the value given in the CSS:

```CSS
.slider-origin {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
}
```

The third element is the handle itself. I have arranged the CSS so that it is centred horizontally on the left edge of the parent element (the origin):

```CSS
.slider-handle {
  position: relative;
  width: 26px;
  height: 26px;
  left: -12px;
  top: -4px;
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #e0e0e0;
  background-color: #808080;
}
```

Here's what it looks like in the browser:

![Screenshot](https://github.com/MEAN-stack/Angular-range-control/blob/master/Slider1.jpg)

## Step 2 - Angularize the page

Here is the full HTML:

```HTML
<!DOCTYPE html>
<html ng-app="exampleApp">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.9/angular.js"></script>
    <link rel="stylesheet" href="https://netdna.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
    <link rel="stylesheet" href="slider2.css">
    <script>
      angular.module("exampleApp", [])
        .controller("sliderCtrl", function($scope) {
          $scope.settings = {
            value: "60"
          }
        })
    </script>
  </head>
  <body ng-controller="sliderCtrl">
    <div class="container" style="padding:20px">
      <div class="slider-target">
        <div class="slider-origin" ng-style="{'left':settings.value+'%'}">
          <div class="slider-handle"><span class="glyphicon glyphicon-expand slider-icon"></span></div>
        </div>
      </div>
    </div>
    <input type="text" ng-model="settings.value">
  </body>
</html>
```

I have used a Bootstrap glyphicon to add some styling to the slider handle. Here's the CSS for the handle and the glyphicon:

```CSS
.slider-handle {
  position: relative;
  width: 20px;
  height: 18px;
  left: -10px;
  display: inline-block;
  background-color: #202020;
}
.slider-icon {
  position: relative;
  font-size: 28px;
  top: -5px;
  left: -3px;
  color: green;
}
```

You can see that I have achieved the green-on-black effect by reducing the size of the slider-handle element, and giving it a dark background. Then I position the larger glyphicon over the top of the handle.

The AngularJS is pretty basic:
* I have added a controller which exposes (via its scope) a value representing the handle position.
* I have used an angular ng-style directive to bind the left edge position of the slider-origin element to the value in the controller's scope. 
* Finally, I have added an HTML input element with a two-way binding between the view and the controller, which allows me to change the value, and hence the position of the handle.

Here's what it looks like in a browser:

![Screenshot](https://github.com/MEAN-stack/Angular-range-control/blob/master/Slider2.jpg)

## Step 3 - Background colours

The next thing I want to do is to add a variable to my controller's scope which represents the zero position in the slider's range of values. To the left of the zero position, the slider will have a red background, and to the right it will be blue.

Here's the whole HTML file:

```HTML
<!DOCTYPE html>
<html ng-app="exampleApp">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.9/angular.js"></script>
    <link rel="stylesheet" href="https://netdna.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
    <link rel="stylesheet" href="slider3.css">
    <script>
      angular.module("exampleApp", [])
        .controller("sliderCtrl", function($scope) {
          $scope.settings = {
            handlepos: "60",
            zeropos: "33"
          }
        })
    </script>
  </head>
  <body ng-controller="sliderCtrl">
    <div class="container" style="padding:20px;">
      <div class="slider-target" style="margin:20px">
        <div class="slider-origin slider-negative" ng-style="{'right':settings.zeropos+'%'}"></div>
        <div class="slider-origin slider-positive" ng-style="{'left':settings.zeropos+'%'}"></div>
        <div class="slider-origin" ng-style="{'left':settings.handlepos+'%'}">
          <div class="slider-handle"><span class="glyphicon glyphicon-expand slider-icon"></span></div>
        </div>
      </div>
      <form>
        <div class="form-group">
          <label for="handlepos">Handle Position</label>
          <input id="handlepos" type="text" ng-model="settings.handlepos">
        </div>
        <div class="form-group">
          <label for="zeropos">Zero Position</label>
          <input id="zeropos" type="text" ng-model="settings.zeropos">
        </div>
      </form>
    </div>
  </body>
</html>
```

And here's the CSS for the two background elements:

```CSS
 .slider-negative {
   background-color:red;
   border-top-left-radius:4px;
   border-bottom-left-radius:4px;
 }

 .slider-positive {
   background-color:blue;
   border-top-right-radius:4px;
   border-bottom-right-radius:4px;
 }
``` 

I have just added a couple of background elements coloured red and blue respectively.
Here's a screenshot from the browser:

![Screenshot](https://github.com/MEAN-stack/Angular-range-control/blob/master/Slider3a.jpg)

That looks ok, but I'm thinking that the new elements are not actually both necessary, and there's a bug in the positioning:

![Screenshot](https://github.com/MEAN-stack/Angular-range-control/blob/master/Slider3b.jpg)

I don't actually know what the problem is here, but I'm going to fix it by removing the slider-negative element, and giving the target element a red background:

![Screenshot](https://github.com/MEAN-stack/Angular-range-control/blob/master/Slider3c.jpg)

That's better!

## Step 4 - Range settings

My control needs to encapsulate a range of values, from a given minimum to a given maximum. There will be some maths required in order to convert between the value represented by the slider handle, and its position given as a percentage of the width of the slider. Here's the Angular controller:

```JavaScript
angular.module("exampleApp", [])
.controller("sliderCtrl", function($scope) {
  $scope.settings = {
    range: {
      min: -50,
      max: 100
    },
    value: 50
  }

  // Calculate the handle position as a percentage of the slider's width
  $scope.handlePos = function() {
    var val = ((($scope.settings.value-$scope.settings.range.min) * 100)/($scope.settings.range.max - $scope.settings.range.min + 1))

    // clip the value so that it lies in the range 0 <= val <= 100
    if ( val > 100 ) {
      val = 100
    }
    else if ( val < 0 ) {
      val = 0
    }
    return val + "%"
  }
		  
  // Calculate the zero position as a percentage of the slider's width
  $scope.zeroPos = function() {
    var val = (($scope.settings.range.min * -100)/($scope.settings.range.max - $scope.settings.range.min + 1))

    // clip the value so that it lies in the range 0 <= val <= 100
    if ( val > 100 ) {
      val = 100
    }
    else if ( val < 0 ) {
      val = 0
    }
    return val + "%"
  }
})
```

I have changed the scope's settings object to represent the minimum and maximum values of the slider, and the current value, indcated by the slider's handle.

I have also added behaviours which convert between slider values and percentages of the slider width.

Here's how I use the controller behaviours in the HTML:

```HTML
<body ng-controller="sliderCtrl">
  <div class="container" style="padding:20px;">
    <div class="slider-target" style="margin:20px">
      <div class="slider-origin slider-positive" ng-style="{'left':zeroPos()}"></div>
      <div class="slider-origin" ng-style="{'left':handlePos()}">
        <div my-draggable class="slider-handle"><span class="glyphicon glyphicon-expand slider-icon"></span></div>
      </div>
    </div>
    <form>
      <div class="form-group">
        <label for="handlepos">Handle Position</label>
        <input id="handlepos" type="text" ng-model="settings.value">
      </div>
      <div class="form-group">
        <label for="min">Min Value</label>
        <input id="min" type="text" ng-model="settings.range.min">
      </div>
      <div class="form-group">
        <label for="max">Max Value</label>
        <input id="max" type="text" ng-model="settings.range.max">
      </div>
    </form>
  </div>
</body>
```

Here's a screenshot from the browser:

![Screenshot](https://github.com/MEAN-stack/Angular-range-control/blob/master/Slider4.jpg)

## Step 5 - Make the handle draggable

Next, I want to make the slider handle draggable. There is an example of a draggable div on the AngularJS site [here](https://docs.angularjs.org/guide/directive)

Here's the code, copied from that example:

```JavaScript
 angular.module('dragModule', [])
 .directive('myDraggable', ['$document', function($document) {
   return {
     link: function(scope, element, attr) {
       var startX = 0, startY = 0, x = 0, y = 0;

       element.css({
        position: 'relative',
        border: '1px solid red',
        backgroundColor: 'lightgrey',
        cursor: 'pointer'
       });

       element.on('mousedown', function(event) {
         // Prevent default dragging of selected content
         event.preventDefault();
         startX = event.pageX - x;
         startY = event.pageY - y;
         $document.on('mousemove', mousemove);
         $document.on('mouseup', mouseup);
       });

       function mousemove(event) {
         y = event.pageY - startY;
         x = event.pageX - startX;
         element.css({
           top: y + 'px',
           left:  x + 'px'
         });
       }

       function mouseup() {
         $document.off('mousemove', mousemove);
         $document.off('mouseup', mouseup);
       }
     }
   };
 }]);
```

I'm going to modify this directive so that it only allows horizontal movement, then I'm going to include the directive in my Angular controller and add the my-draggable attribute to my slider's handle. Here's the modified directive:

```JavaScript
.directive('myDraggable', ['$document', function($document) {
   return {
     link: function(scope, element, attr) {
       var startX = 0, x = 0;
       var targetWidth = 0;
       var valueRange = 0;
       var startValue = 0;
 	  
       // clip a value to lie in a given range
       // range.min <= value <= range.max
       var clip = function(range, value){
         return Math.min(range.max, Math.max(range.min, value));
       }

       element.on('mousedown', function(event) {
         // get the width in pixels of the slider-target
         targetWidth = parseInt(element.parent().parent().prop('offsetWidth'));
         
         // get the range of the slider in terms of values
         valueRange = scope.settings.range.max - scope.settings.range.min + 1;

         // Prevent default dragging of selected content
         event.preventDefault();

         // start the drag
         startX = event.pageX;
         startValue = parseInt(scope.settings.value);
         $document.on('mousemove', mousemove);
         $document.on('mouseup', mouseup);
       });

       function mousemove(event) {
         x = event.pageX - startX;
         // the Angular controller needs to be told about this change to its scope
         scope.$apply(function() {
           scope.settings.value = clip(scope.settings.range, startValue + (x*valueRange)/targetWidth);
         });
         lastX = x;
       }

       function mouseup() {
         $document.off('mousemove', mousemove);
         $document.off('mouseup', mouseup);
       }
     }
   };
``` 

I have moved the cursor: pointer CSS into my css file, and, in the mousemove handler, instead of setting the position directly in the element's css, I update the value variable in the scope. This way, the scope takes care of setting the handle position, and reflects the value in the first `<input>` element.

Here's the HTML which uses the directive:

```HTML
<div class="slider-target" style="margin:20px">
  <div class="slider-origin slider-positive" ng-style="{'left':zeroPos()}"></div>
  <div class="slider-origin" ng-style="{'left':handlePos()}">
    <div my-draggable class="slider-handle"><span class="glyphicon glyphicon-expand slider-icon"></span></div>
  </div>
</div>
```

Here's a screenshot:

![Screenshot](https://github.com/MEAN-stack/Angular-range-control/blob/master/Slider4a.jpg)

This works ok, but really the directive should encapsulate the whole range control, not just the handle. Here are the changes needed to achieve this:

```JavaScript
.directive('mySlider', ['$document', function($document) {
  return {
    link: function(scope, element, attr) {
      var startX = 0;
      var x = 0;
      var targetWidth = 0;
      var range = 0;
      var startValue = 0;
	  
      // find the handle element
      var elHandle = null;
      var children = element.children();
      for (var i=0; i<children.length; i++) {
        if (children.eq(i).hasClass('slider-origin')) {
          var grandchildren = children.eq(i).children();
          for (var j=0; j<grandchildren.length; j++) {
            if (grandchildren.eq(j).hasClass('slider-handle')) {
              elHandle = grandchildren.eq(j);
            }
          }
        }
      }

...

<div my-slider class="slider-target" style="margin:20px">
  <div class="slider-positive" ng-style="{'left':zeroPos()}"></div>
  <div class="slider-origin" ng-style="{'left':handlePos()}">
    <div class="slider-handle"><span class="glyphicon glyphicon-expand slider-icon"></span></div>
  </div>
</div>
```

Notice that I have renamed the directive 'mySlider'.

I have added a function to traverse the slider's child elements and locate the slider handle. I don't know much about jqLite - is there a better/easier way than this?

## Step 6 - Give the directive its own scope

To make this control reusable, it needs to encapsulate its properties and behaviours. That means it needs its own scope.
Let's have a go at that.

There are lots of different ways in which directives can be isolated or can have one-way or two-way bindings to the controller's scope.

I need to get this right, so I am going to modify the HTML to include two sliders in a single controller's view. That way I can check whether the sliders operate independently of each other.

The controller doesn't need it's behaviours any more. Those calculations are done in the slider directive now. All I need is the variables to bind to the properties exposed by the directive - namely the min, max, and value of the sliders. My two sliders will have the same min and max so my controller looks like this:

```JavaScript
angular.module("exampleApp", [])
.controller("sliderCtrl", function($scope, $interval) {
  $scope.settings = {
    range: {
      min: -250,
      max: 100
    },
    value1: 50,
    value2: -50
  } 
})
``` 

Now I'm going to move the slider's HTML into a template which the directive can use:

```HTML
<script type="text/ng-template" id="rangetemplate">
  <div class="slider-positive" ng-style="{'left':zeroPos()}"></div>
  <div class="slider-origin" ng-style="{'left':handlePos()}">
    <div class="slider-handle"><span class="glyphicon glyphicon-expand slider-icon"></span></div>
  </div>
</script>

...
 
 .directive('mySlider', ['$document', function($document) {
   return {
     template: function() {
       return angular.element(document.querySelector("#slidertemplate")).html();
     },
     scope: {
       min: "=slidermin",
       max: "=slidermax",
       value: "=sliderval"	  
     },
 ...
```

There are three variables in the directive's scope, and they have two-way bindings to the controller's scope (the two-way binding is indicated by the '=' symbol). The bindings are achieved via attributes in the HTML named slidermin, slidermax, and sliderval. Like this:

```HTML
<div my-slider slidermin="settings.range.min" slidermax="settings.range.max" sliderval="settings.value1" ...
<div my-slider slidermin="settings.range.min" slidermax="settings.range.max" sliderval="settings.value2" ...
```
 
What happens when the user changes a value in one of the `<input>` elements?

Angular will update the value of the variable in controller's scope, and the two-way binding will ensure that the corresponding value is updated in the directive's scope, but in order for the directive to adjust and re-render itself I need to set up some watchers:

```JavaScript
 ...
 //watchers
 scope.$watch('value', function(newValue) {
   elOrigin.css('left', handlePos())
 });
 scope.$watch('min', function(newValue) {
   elZero.css('left', zeroPos());
   elOrigin.css('left', handlePos());
 });
 scope.$watch('max', function(newValue) {
   elZero.css('left', zeroPos());
   elOrigin.css('left', handlePos());
 });
 ...	  
```

## Last Step - package the directive in a module

The last step required to make my slider reusable is to package it in a module.

I have replaced the template function in the directive with a simple string containing the template HTML.
I'm not sure what to do about the CSS - perhaps some kind person will help me?

I want the directive, or the new module to contain the default styles for the slider elements, but I want it to be clear to other developers how to override these styles, for example to change the glyphicon on the handle, or the background colour of the target.

For now, I will set the styles explicitly in the directive.

You can see a demo [here](http://mean-stack.github.io/slidertest.html)

For completeness, here is the finished code:

`slider.js`

```JavaScript
angular.module("slider", [])
.directive('mySlider', ['$document', function($document) {
  return {
    template: 
    '<style>'+
    '.slider-target {'+
    '  position: relative;'+
    '  width: 90%;'+
    '  height: 20px;'+
    '  display: inline-block;'+
    '  border-radius: 4px;'+
    '  border: 1px solid #e0e0e0;'+
    '  background-color: red;'+
    '}'+
    '.slider-origin {'+
    '  position: absolute;'+
    '  top: 0;'+
    '  left: 0;'+
    '  bottom: 0;'+
    '  right: 0;'+
    '}'+
    '.slider-positive {'+
    '  position: absolute;'+
    '  top: 0;'+
    '  left: 0;'+
    '  bottom: 0;'+
    '  right: 0;'+
    '  background-color:blue;'+
    '  border-top-right-radius:4px;'+
    '  border-bottom-right-radius:4px;'+
    '}'+
    '.slider-handle {'+
    '  position: relative;'+
    '  width: 20px;'+
    '  height: 18px;'+
    '  left: -10px;'+
    '  display: inline-block;'+
    '  background-color: #202020;'+
    '  cursor: "pointer";'+
    '}'+
    '.slider-icon {'+
    '  position: relative;'+
    '  font-size: 28px;'+
    '  top: -5px;'+
    '  left: -3px;'+
    '  color: green;'+
    '}'+
    '</style>'+
    '<div class="slider-positive"></div>'+
    '<div class="slider-origin">'+
    '  <div class="slider-handle">'+
    '    <span class="glyphicon glyphicon-expand slider-icon"></span>'+
    '  </div>'+
    '</div>',
    scope: {
      min: "=slidermin",
      max: "=slidermax",
      value: "=sliderval",	  
    },
    link: function(scope, element, attr) {
      var startX = 0;
      var x = 0;
      var targetWidth = 0;
      var valueRange = 0;
      var startValue = 0;
	  
      // make sure the value for the slider handle position
      // is within the range of the control
      var clip = function(range, value){
        return Math.min(range.max, Math.max(range.min, value));
      }
	  
      // calculate the percentage position of the zero position
      // for controls where the range goes from negative to positive
      var zeroPos = function() {
        var zero = ((scope.min * -100)/(scope.max - scope.min + 1))
        if ( zero > 100 ) {
          zero = 100
        }
        else if ( zero < 0 ) {
          zero = 0
        }
        return zero + "%";	  
      } 
	  
      // calculate the percentage position of the left edge
      // of the slider-origin element
      var handlePos = function() {
        var val = (((scope.value-scope.min) * 100)/(scope.max - scope.min + 1))
        if ( val > 100 ) {
          val = 100
        }
        else if ( val < 0 ) {
          val = 0
        }
        return val + "%";
      }

      element.addClass('slider-target');
      // find the handle element;
      var elHandle = null;
      var elOrigin = null;
      var elZero = null;
	  
      var children = element.children();
      for (var i=0; i<children.length; i++) {
        if (children.eq(i).hasClass('slider-origin')) {
          elOrigin = children.eq(i);
          var grandchildren = elOrigin.children();
          for (var j=0; j<grandchildren.length; j++) {
            if (grandchildren.eq(j).hasClass('slider-handle')) {
              elHandle = grandchildren.eq(j);
            }
	  }
	}
        else if (children.eq(i).hasClass('slider-positive')) {
	  elZero = children.eq(i);
        }
      }
      elZero.css('left', zeroPos());
      scope.value = clip({min:scope.min, max:scope.max}, scope.value);	  
      elOrigin.css('left', handlePos());
	  
      //watchers
      scope.$watch('value', function(newValue) {
        elOrigin.css('left', handlePos())
      });
      scope.$watch('min', function(newValue) {
        elZero.css('left', zeroPos());
        elOrigin.css('left', handlePos());
      });
      scope.$watch('max', function(newValue) {
        elZero.css('left', zeroPos());
        elOrigin.css('left', handlePos());
      });
	  
      elHandle.on('mousedown', function(event) {
        targetWidth = parseInt(element.prop('offsetWidth'));
        valueRange = scope.max - scope.min + 1;
	
        // Prevent default dragging of selected content
        event.preventDefault();
        startX = event.pageX;
        startValue = parseInt(scope.value);
        $document.on('mousemove', mousemove);
        $document.on('mouseup', mouseup);
      });

      function mousemove(event) {
        x = event.pageX - startX;
        scope.$apply(function() {
          scope.value = clip({min:scope.min, max:scope.max}, startValue + (x*valueRange)/targetWidth);
        })
        elOrigin.css('left', handlePos());
          lastX = x;
      }

      function mouseup() {
        $document.off('mousemove', mousemove);
        $document.off('mouseup', mouseup);
      }
    }
  };
}]);
```

`slidertest.html`

```HTML
<!DOCTYPE html>
<html ng-app="exampleApp">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.9/angular.js"></script>
	<script src="slider.js"></script>
    <link rel="stylesheet" href="https://netdna.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
    <script>
      angular.module("exampleApp", ["slider"])
      .controller("sliderCtrl", function($scope, $interval) {
        $scope.settings = {
          range: {
            min: -250,
            max: 100
          },
          value1: 50,
          value2: -50
        } 
      })
    </script>
  </head>
  <body ng-controller="sliderCtrl">
    <div class="container" style="padding:20px;">
      <div my-slider slidermin="settings.range.min" slidermax="settings.range.max" sliderval="settings.value1" style="margin:20px"></div>
      <div my-slider slidermin="settings.range.min" slidermax="settings.range.max" sliderval="settings.value2" style="margin:20px"></div>
      <form>
        <div class="form-group">
          <label for="handlepos">Slider Value</label>
          <input id="handlepos" type="text" ng-model="settings.value1">
        </div>
        <div class="form-group">
          <label for="handlepos">Slider2 Value</label>
          <input id="handlepos" type="text" ng-model="settings.value2">
        </div>
        <div class="form-group">
          <label for="handlepos">Min Value</label>
          <input id="min" type="text" ng-model="settings.range.min">
        </div>
        <div class="form-group">
          <label for="handlepos">Max Value</label>
          <input id="max" type="text" ng-model="settings.range.max">
        </div>
      </form>
    </div>
  </body>
</html>
```

## What next?

Send me some feedback.
Sometime I'll try to implement the same thing with React - It will be interesting to see how it compares...

