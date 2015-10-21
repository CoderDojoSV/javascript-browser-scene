# JavaScript and jQuery to create a scene in your browser

This week, we will grow our knowledge about JavaScript a bit, and we will learn a bit about jQuery.  We will use what we learn to have fun making a scene in the browser.

The material from last week is available at http://coderdojosv.github.io/javascript-browser-game/  Sorry about the bugs last week.  They are fixed.

Note: Khan Academy is an excellent environment for learning JavaScript, and you can learn a great deal about JavaScript and drawing with processing.js on their site.
**However**, we changed plans and this class isn't going to teach you Khan Academy, and will instead continue showing what you can do in a web page.
You can continue to use Khan Academy's site for your own projects, but because they do not let you use external graphics nor allow for separate javascript and css files, you should use [Thimble](https://thimble.mozilla.org) or [CodePen.io](http://codepen.io) tonight.  (Incidentally, codepen.io was used for crafting this lesson.)

The goal for this session is for you to learn a little more JavaScript by example, and to apply your knowledge to create a web page scene of your own, or alter the example page in your remixed version.  We will show fading and flying techniques.  The final page described below can be [seen here](https://d157rqmxrxj6ey.cloudfront.net/jamwave/7997).   The example shows a space scene with aliens and flying saucers.  You are encouraged to find something that matches the season (Halloween?  Christmas?) or your interests.  If your interest is UFOs, you're in luck!

# Background

By now, you should be familiar with web development basics enough to understand this HTML code.  As a quick refresher, it is linking in a css page and a javascript page, then declaring a few div's (areas) to work with, and starting with some images. (Note: CodePen lets you leave out linking in css and javascrfipt files.)    
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>My page</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <script src="script.js"></script>

  <div id="animateTop">
  </div>
  
  <div id="belowTop">
    <p><font size="+3" color="white">Having an out of this world time with CoderDojo!!</font></p>
  </div>
  <div id="bottom">
    <img src="http://www.clker.com/cliparts/u/B/R/o/T/l/my-alien-md.png" height="200px" id="bottom1" class="fadeable">
    <img src="http://www.clker.com/cliparts/u/B/R/o/T/l/my-alien-md.png" height="200px" id="bottom2" class="fadeable">
    <img src="http://www.clker.com/cliparts/u/B/R/o/T/l/my-alien-md.png" height="200px" id="bottom3" class="fadeable">
  </div>
</body>
</html>
```

Can you name the three div areas that are declared?  They are used later in the css page and much later in the JavaScript code.

Quick activity:
- Change the message in the HTML
- Begin thinking of how you might change this to an entirely different scene.

# CSS

Thimble creates a default css page for you.  You can replace it with this...
```css
body {
  background: url("http://www.nasa.gov/sites/default/files/137120main_hst_pluto1_full.jpg");
  background-size: 100% 100%;
  background-repeat: no-repeat;
  width: 800px;
  height: 600px;
}

#animateTop {
  left: 0;
  top: 0;
  position: absolute;
  height: auto;
}

#belowTop {
  left: 0;
  top: 200px;
  position: absolute;
  height: auto;
  text-align: center;
  margin: auto;
  width: 60%;
}

#bottom {
  left: 0;
  top: 400px;
  height: 200px;
  position: absolute;
  height: auto;
}

#bottom1 {
  left: 100px;
}

#bottom2 {
  left: 300px;
}

#bottom3 {
  left: 500px;
}

.fadeable {
  position: absolute;
  alpha: 0;
}

```

This page is not intended to teach css, and in fact some of the JavaScript being presented tonight can be replaced with css that does the same animation, but let's point out a few highlights.

This page has a background image.  It uses some css tricks to stretch the image to fill the page.  (It also sets some hard limits on the size of the page.)

It uses absolute positions on the three areas so that it can simplify the layout.  If you want to changee sizes, or do more advanced animation, you can certainly change things so that they overlap.

It declares three different image elements that happen to be the same image, but at different positions.  There's no reason they have to be the same image, or even the positions here.  Feel free to play with the positions.  For the different positions, it uses 3 different element ids.  For the generic "fadeableness", it uses a class, so that those magic strings do not have to be repeated for every element that will be faded..

In the fadeable class declaration, it uses an alpha of 0 to start them invisible.  The *Alpha* value describes something's visibility amount.  100% is fully visible, 0% is invisible.  We will have fun playing with fade effects, which change the alpha channel.

Quick Activity:
- change the locations of the bottom elements.

# JavaScript

We are going to learn a tiny bit of jQuery tonight.  [jQuery](https://jquery.com/) is a library that is incredibly useful for manipulating elements on a web page.  Code that could take 1000s of lines in raw Javascript can take 1 line using jQuery. YAY!

## jQuery selector

The single most important and noticable thing about jQuery is that it makes the getElementById call (and other calls like it) be magically accessible by an operator it creates, the dollar operator.  So, you will see this a lot in jQuery code:

```javascript 
$(something)
```

*something* is replaced in your code with a string naming an element or elements (remember it would be in "quotes"), or with magic global objects, like *document*  (which is another word for 'the web page itself').  We will be manipulating the bottom1, bottom2 and bottom3 elements first.

Also, JavaScript is, at it's heart, a *functional* language, and wants functions to call as callbacks when stuff happens..  Sometimes, we declare functions in advance, but javascript allows, and jQuery encourages, anonymous functions where the code it plopped right where the callback goes.  I try to minimize that for readability here, but it's a very idiomatic way of coding JavaScript (meaning it's the accepted and usual way.)

## In your HTML

We need to add one thing to the HTML to bring in jQuery.  Put it **BEFORE** the other Javascript line, where you include your code.  (If you use "googleapis" or other CDN, it's likely to be faster, and even faster if your browser has it cached, which is very likely with jQuery.)

```html
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script> 
  <script src="script.js"></script>
```

When using Thimble, you have to remember to create a new JavaScript file.  In Thimble, click the "green and white + button" beside the word "Files" in the upper left corner.  Choose JavaScript.  Click the name "script-1.js" and immediately click the name and delete the -1 characters so that it is named "script.js" and you can match the code you used earlier.
(Alternatively, A nice thing about codepen.io is that you automatically get all three, visible at one time, and your html code doesn't have to do the effort of including the others.  Also, you choose your libraries, like jQuery, by a simple dropdown.  However, it is only for 1-page web sites, and it is a little less oriented towards learning.)

## JavaScript in the js file

You will want all the HTML elements to be ready to use before you start making JavaScript calls.  Web pages make a very simple call out to the javascript ready() function when everything else is ready.  [This is the page where jQuery describes what is happening](https://learn.jquery.com/using-jquery-core/document-ready/).
```javascript
$(document).ready(function() {
...
}
```

This is another bit of jQuery magic.   Everything in the curly braces will be run when the document is ready.  This is one of those anonymous JavaScript functions mentioned earlier.  It's called anonymous because there is no name, just function()


## Our first jQuery effect: fading

Since we want to continually fade the images along the bottom, this is the place to start doing that.
You may remember the setInterval() call from weeks past.   This is a great time to use that for things that will repeatedly run forever (or as long as the page is up).

So, the code in the curly brackets of this anonymous function, represented above by ..., will have a setInterval call per thing we are going to fade.  In this example, we'll start again every 5 seconds.  And, we'll use another anonymous function.
```javascript
  setInterval(function() {
     ...
  }, 5000);
```

OK, so now time for a bit more jQuery magic, and this is where it gets amazing.  jQuery uses a trick of the JavaScript language to allow for chaining together calls to functions. What functions might you want to chain together?  

Many of the [jQuery effects](http://api.jquery.com/category/effects/) are interesting.  
The fade in and fade out effects are useful for this project.  It helps to put in a brief delay in to let the alien linger a bit.   They are chained together like this:

```javascript
    $('#bottom1').fadeIn(1000).delay(2000).fadeOut(1500).delay(5000);
```

You can see this is the code for just one of the things at the bottom.  The HTML is prepared for three, and we want them to have different timings, because it looks cooler!  So, here is the complete code for the aliens at the bottom:

```javascript
$(document).ready(function() {
  setInterval(function() {
    $('#bottom1').fadeIn(1000).delay(2000).fadeOut(1500).delay(5000);
  }, 5000);
  setInterval(function() { 
    $('#bottom2').delay(4000).fadeIn(1000).delay(2000).fadeOut(1500).delay(4000);
  }, 5000);
  setInterval(function() {
    $('#bottom3').delay(7000).fadeIn(1000).delay(2000).fadeOut(1500).delay(5000);
  }, 5000);
});
```

You can take a look at [checkpoint 1 code](https://d157rqmxrxj6ey.cloudfront.net/jamwave/7997) if you are stuck.

**Tonight's Activities**:
- Add more aliens
- Use different aliens.
- Change the timing values
-
** Advanced Challenges **
- Change from the fade effect to the slideUp and slideDown
- Use an entirely different scene.  Maybe ghosts in a spooky setting?  Hands coming up from below?

## Tonight's other jQuery effect: flying by animation

jQuery has a simple effect for making things go up and down, mentioned above.  Let's also try a different way and learn about going across the screen with the [jQuery animate()](http://api.jquery.com/animate/) call.  

### HTML
Now we will use the area across the top that was reserved earlier.  Now add something to animate:

```html
  <div id="animateTop">
    <img src="http://images.clipartpanda.com/spaceship-clipart-spaceship1.png" height="100px">
  </div>
```

### CSS
It appears that Thimble wants to own the top of the screen.  (If you are trying codePen, click "Change View" to debug mode to see the whole page.) 
In the css, you might want to use a different top value for the animateTop position
```css
  top: 150px;
```

### JavaScript
At its simplest, this is the code that will make something start at a start position and then go across.

```javascipt
function animateFwd() {
  // make sure we are at the left margin
  $('#animateTop').css({
    'top': '0px',
    'left': '0px'
  });
  //now call animate and have jQuery tween it across
  $('#animateTop').animate({
    'left': '1000px',
    'top': '0px'
  }, 2000, "linear", animateFwd);
};
```

Also, don't forget to call this function.  It just has to be in the document.ready function.  If it's the first line, then it should look like this:
```javascript
$(document).ready(function() {
  animateFwd();
  ...
```

The [jQuery api page for animate](http://api.jquery.com/animate/) has plenty of good information on what you can tweak.
Here's a quick dscription of a few tricks you see here.  
- The most tricky code is that the last parameter of animate is used to tell javascript/jquery what to do when done with the animation.  In this case, we just want to repeat the animation, so it calls itself.  
There's a concept in computer science called recursion, where a function call itself to get more work done.  This is sort of like that, but it's closer to just an infinite loop, and not actually calling itself within itself.  
- Next, this hard codes the distance to go and time to take (which indirectly sets the speed of pixels per second).
- This code uses the "linear" easing function which makes it appear steady.  If you replace it with "", you will see the default easing function, which gains and loses speed at the edges.  

This is visible at [checkpoint 2](https://d157rqmxrxj6ey.cloudfront.net/jamwave/8079)

If you are comfortable with the way the saucer goes across repeatedly, you can skip these activities.
Activity: 
- use other art or different positions for the art
Challenge:
- See if you can apply fadeability to the flying UFO
- if you are up for an advanced challenge, link a few animation calls together and alter the path of the saucer!


## Even More Advanced flying

There is a different version of code that allows the top thing to goback and forth, and can even turn around the image.
This project was originally going to be Halloween-themedwith a witch going back and forth.  (A witch starting again and again, left to right, looked weird).  It's not hard to reverse the animation, but a witch flying backward looked really wrong.  Get the witch to look good going in either direction finally looked good, but the code was too complex to teach.  
If you want to see it, look at [this codepen.io page](http://codepen.io/jimwhitfield/full/YyEjxa/)

This code does not wire up the constant repeat cycle.  It uses buttons to test the behaviors, but the functions are there, and if you want the... 

Very optional Challenges:
- If you are up for the master challenge, take the back-and-forth code, merge it into the always-repeating going forward code
- If you are up for the super master challenge, have it alter the path along the way.   

Good Luck!

Another codepen that served as an inspiration is (glowing bubbles that you can pop)[http://codepen.io/embrilliant/pen/bNvGJb]

We would love for you to show your project, even if you only do a simple challenge.  Also, rememeber that with Thimble or CoderPen, you can publish your projects and share it with friends and family.

