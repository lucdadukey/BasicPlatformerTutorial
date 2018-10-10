# BASIC PLATFORMER TUTORIAL

**End Result:** https://basic-platformer--luchutton.repl.co/ \
**The Code:** https://repl.it/@LucHutton/Basic-Platformer

## Prequisities And Advice:
For this tutorial you should already have a basic understanding of programming (preferrably in Javascript) as **I will not be explaining basic Javascript concepts in this tutorial.** If you wish to learn some/more Javascript I reccommend you visit: https://developer.mozilla.org/en-US/docs/Web/JavaScript

## Setup:
It is best if you follow along using a **HTML + CSS + JS REPL**, as when created the REPL will already have all the boiler plate stuff, but also because you will be able to see the result straight away on the same screen (when you run the code).

The first (and most important) line of code you will write will be in the **index.html** file. Don't worry this is the only bit of HTML you will have to write :). \
Place the line of code after the opening body tag but before the script tag, like so:

```html
<body>
  
  <canvas id="canvas" width="512" height="512"></canvas>
                                          
  <script src="script.js"></script>
</body>
```

Make sure the id of the canvas is "canvas", but you can change the width and height to whatever number you want as long as the **width and height are multiples of 32!**

----

Navigate yourself to the script.js file where we will be spending the remainder of this tutorial. The first thing that we need to do is to create a place in which we can store the attributes of the player, this will be done using an **Object**. For starters our player needs to have an X coordinate, a Y coordinate, a Width and a Height. For simplicity's sake the player will be a 32 x 32 pixel square, and the initial X will be set to half the canvas width, and Y will be set to half the canvas height.

```javascript
  const player = {
    x: 256,
    y: 256,
    width: 32,
    height: 32
  }
```

**Make sure the final value doesn't have a comma after it!**\
At the top of the Javascript file we need to create a reference to the canvas element we created in the first step like so:
```javascript
const c = document.getElementById("canvas").getContext("2d);
```
It basically gets the canvas element -> specifys that we will be working in two dimensions -> sets the value of c that.

Now we need a way to draw the player at its location with its size. To do this we will create a **draw function**, inside it we will need to specify the fill colour of the player and how to draw the player. 

