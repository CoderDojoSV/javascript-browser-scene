# JavaScript and jQuery to create a scene in your browser

This week, we will grow our knowledge about javascript a bit, and we will learn a bit about jQuery.  We will use what we learn to have fun making a scene in the browser.

The material from last week is available at http://coderdojosv.github.io/javascript-browser-game/  Sorry about the bugs last week.  They are fixed.

Khan Academy is an excellent environment for learning JavaScript, and you can learn a great deal about javascript and drawing with processing.js on their site.
**However**, this class actually isn't going to teach you Khan Academy, and will instead continue showing what you can do in a web page.
You can continue to use Khan Academy's site for your own projects, but because they do not let you use external graphics nor allow for separate javascript ans css files, you should use (Thimble)[https://thimble.mozilla.org) or http://codepen.io tonight.  (I used codepen.io for crafting this lesson.)

The goal for this session is for you to learn a little more JavaScript by example, and to apply your knowledge to create a web page scene of your own, or alter the example page in your remixed version.  We will show fading and flying techniques.  The final page described below can be (seen here)[https://d157rqmxrxj6ey.cloudfront.net/jamwave/7997].   The example shows a space scene with aliens and flying saucers.  You are encouraged to find something that matches the season (Halloween?  Christmas?) or your interests. 

# Background

By now, you should be familiar with web development basics enough to understand this HTML code.  As a quick refresher, it is linking in a css page and a javascript page, then declaring a few div's (areas) to work with, and starting with some images.   
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
  
  <p id="belowTop">
    <font size="+3" color="white">Having an out of this world time with CoderDojo!!</font>
  </p>
  <div id="bottom">
    <img src="http://www.clker.com/cliparts/u/B/R/o/T/l/my-alien-md.png" height="200px" id="bottom1" class="fadeable">
    <img src="http://www.clker.com/cliparts/u/B/R/o/T/l/my-alien-md.png" height="200px" id="bottom2" class="fadeable">
    <img src="http://www.clker.com/cliparts/u/B/R/o/T/l/my-alien-md.png" height="200px" id="bottom3" class="fadeable">
  </div>
</body>
</html>
```

Can you name the three div areas that are declared?  They are used later in the css page and much later in the JavaScript code.

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

This material is not intended to teach css, and in fact some of the JavaScript being presented tonight can be replaced with css that does the same animation, but let me point out a few highlights.

This page has a background image.  I used some css tricks to stretch it to fill the page.  (I also set some hard limits on the size of the page.)

I used absolute positions on the three areas so that I could simplify my layout.  If you want to do more advanced animation, you can certainly change things so that they overlap.

I declared three different image elements that happen to be the same image, but at different positions.  There's no reason they have to be the same image, or even the positions I created.  Feel free to play with the positions.  For the different positions, I used element ids.  For the fadeableness, I used a class, so that I didn't have to repeat the same magic every time.

In the fadeable class declaration, I used an alpha of 0 to start them invisible.  The *Alpha* value describes something's visibility amount.  100% is fully visible, 0% is invisible.  We will have fun playing with fade effects, which change the alpha channel.

# JavaScript

We are going to learn a tiny bit of jQuery tonight.  jQuery is a library that is incredibly useful for manipulating elements on a web page.  Code that could take 1000s of lines in raw Javascript can take 1 line using jQuery. YAY!

## jQuery selector

The single most important and noticable thing about jQuery is that it makes the getElementById call (and other calls like it) be magically accessible by an operator it creates, the dollar operator.  So, you will see this a lot in jQuery code:

```javascript 
$(something)
```

*something* is replaced in your code with an element or elements, or with other magic global objects, like "document"  (which is another word for 'the web page itself').  We will be manipllating the bottom1, bottom2 and bottom3 elements first.

Also, JavaScript is at it's heart a *functional* language, and wants functions to call as callbacks when stuf happens..  Sometimes, we declare functions in advance, but javascript allos, and jQuery encourages anonymous functions where the code it plopped right where the callback goes.  I try to minimize that for readability here, but it's a very idiomatic way (meaning it's the accepted and usual way.)

## In your html

We need to add one thing to the HTML to bring in jQuery.  Put it **BEFORE** the other Javascript line, where you include your code.

```html
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script> 
  <script src="script.js"></script>
```

When using Thimble, you have to remember to create a new JavaScript file.  In Thimble, click the green + beside the word "Files" in the upper left corner.  Choose JavaScript.  Click the name "script-1.js" and immediately click the name and delete the -1 characters so you can match the code you used earlier.
(A nice thing about codepen.io is that you automatically get all three, visible at one time, and your html code doesn't have to do the effort of including the others.  Also, you choose your libraries, like jQuery, by a simple dropdown.  The more material I develop, the more I like codepen.)

## Javascript in the js file

You will want all the HTML elements to be ready to use before you start making javascript calls.  Web pages make a very simple call out to any listening javascript when everything else is ready.  [This is the page where jQuery describes what is happening](https://learn.jquery.com/using-jquery-core/document-ready/).
```javascript
$(document).ready(function() {
...
}
```

This is another bit of jQuery magic.   Everything in the curly braces will be run when the document is ready.


## Our first jQuery effect: fading

Since I wanted to continually fade the images along the bottom, this is the place to start doing that.
You may remember the setInterval call from weeks past.   This is a great time to use that for things that will repeatedly run forever (or as long as the page is up).

So, the code in the curly brackets, represented above by ..., will have a setInterval call per thing we are going to fade.  In this example, we'll start again every 5 seconds.
```javascript
  setInterval(function() {
     ...
  }, 5000);
```

OK, so now time for a bit more jQuery magic, and this is where it gets amazing.  jQuery uses a trick of the avaScript language to allow for chaining together calls to functions. What functions might you want to chain together?  

I think that many of the [jQuery effects](http://api.jquery.com/category/effects/) are interesting.  
I decided to use the fade in and fade out effects for my project.  I also put a brief delay in to let the alien linger a bit.   I chain them together like this:

```javascript
    $('#bottom1').fadeIn(1000).delay(2000).fadeOut(1500).delay(5000);
```

You can see this is the code for just one of the things at the bottom.  I prepared for three, and I want them to have different timings.  So, here is the complete code for the aliens at the bottom:

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

You can take a look at [my checkpoint 1 code](https://d157rqmxrxj6ey.cloudfront.net/jamwave/7997) if you are stuck.

**Tonight's Activities**:
- Change the message in the HTML
- Add more aliens
- Use different aliens.
- Change the timing values
** Advanced Challenges **
- Change from the fade effect to the slideUp and slideDown
- Use an entirely different scene.  Maybe ghosts in a spooky setting?  Hands coming up from below?

## Tonights other jQuery: flying

jQuery has a simple effect for making things go up and down, mentioned above.  Lets try a different way and learn about going across the screen with the [jQuery animate()](http://api.jquery.com/animate/) call.  

### HTML
I am using the area across the top that I reserved earlier.  Add something to animate:

```html
  <div id="animateTop">
    <img src="http://images.clipartpanda.com/spaceship-clipart-spaceship1.png" height="100px">
  </div>
```

### CSS
It appears that Thimble wants to own the top of the screen.
In the css, you might want to use a different top for the animateTop ppsition
```css
  top: 150px;
```

### JavaScript
At the core, this is the code that will make something start at a start position and then go across.

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

The [jQuery api page for animate](http://api.jquery.com/animate/) has plenty of good information on what you can tweak.
I want to mention a few tricks I did.  
- The most tricky code is that I used the last parameter of animate to tell it what to do when done with the animation.  In this case, I just wanted to repeat the animation, so I called itself.
There's a concept in computer science called recursion, where a function call itself to get more work done.  This is sort of like that, but I hope it's just an infinite loop, and not actually calling itself within itself.  
- Next, I am hard coding the distance to go and time to take (which sets the pixels per second).
- I used the "linear" easing function which maked it appear steady.  If you replace it with "", you will see the default, which gains and loses speed at the edges.

This is visible at (checkpoint 2)[https://d157rqmxrxj6ey.cloudfront.net/jamwave/8079]

If you are comfortable with the way the saucer goes across repeatedly, you can skip these activities.
Activity: 
- use other art or different positions for the art
Challenge:
- if you are up for an advanced challenge, link a few together and alter the path of the saucer!


## Even More Advanced flying

I have a different version that can go back and forth, and can even turn around the image.

When I first created this project, I was going for a Halloween theme, and I wanted a witch to go back and forth.
I thought that a witch starting again and again looked weird, so I wanted it to go back and forth.  I got that working, but then I saw that a witch flying backward looked really wrong.  I spent way too much time trying to get the witch to look good going in either direction and I think I succeeded.  
If you want to see that, look at [this codepen.io page](http://codepen.io/jimwhitfield/full/YyEjxa/)

However, I decided that the code was getting too big for easy understanding, and I didn't wire up the constant repeat cycle.  (I hooked things up to buttons to test the action.  That us scrubbed from all the other code you have seen tonight.)

Very optional Challenges:
- If you are up for the master challenge, take the back-and-forth code, merge it into the always-repeating going forward code
- If you are up for the super master challenge, have it alter the path along the way.   

Another codepen that served as an inspiration is (glowing bubbles)(http://codepen.io/embrilliant/pen/bNvGJb)

Whereever you are at the end of the session, we would love for you to show your work.  You can also publish it and share it with friends and family.

