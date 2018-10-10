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
const c = document.getElementById("canvas").getContext("2d");
```
It basically gets the canvas element -> specifys that we will be working in two dimensions -> sets the value of c tothat.

Now we need a way to draw the player at its location with its size. To do this we will create a **draw function**, inside it we will need to specify the fill colour of the player and how to draw the player.\
To specify the colour to fill the next object drawn, you update the canvas' fill colour.
```javascript
c.fillStyle = "red";
```
You can set the colour to whatever you want.\
On the next line you need to actually draw the player rectangle using a canvas function called **fillRect** which draws and fills the rectangle at the same time. The **fillRect** function takes four parameters in the following order: an x value, a y value, a width and a height.
```javascript
c.fillRect(player.x, player.y, player.width, player.height);
```
As shown above, you access the attributes of an object using the template: **objectName.attribute**.\
To call this draw function: at the bottom of the javascript file put:
```javascript
draw();
```
If you followed the steps above correctly your Javascript code should look like this:
```javascript
const c = document.getElementById("canvas").getContext("2d");

const player = {
    x: 256,
    y: 256,
    width: 32,
    height: 32
}

function draw(){
  c.fillStyle = "red";
  c.fillRect(player.x, player.y, player.width, player.height);
}

draw();
```
When you run this code you should see a red square appear somewhere in the top-left of your screen.\
Now remove the line calling the draw function as it was only for testing.

---
## Going Loopy
Every meaningful game needs to have a main function that is looped, to update and draw new things so fast that it is almost inperciptible to the human eye. For our game we need a main function that is called when all the updating for one frame is done. So create a function called main, and inside you can put a call to the draw function:
```javascript
function main(){
  draw();
}
```
In this state the main function will only be called once, to fix this we need to put another line below the call to draw that re-runs the main function.
```javascript
function main(){
  draw();
  requestAnimationFrame(main);
}
```
It basically allows the HTML to render what is drawn and then calls the main function again.

---
## Startup
If you run the code you will notice that nothing happens, this is because nothing calls the main function in the first place. What we need is something that runs when the page is fully loaded, luckily javascript has a function just for this:
```javascript
window.onload = function(){
  main();
}
```
When the page loads, whatever is in the function will be executed, so if you run the code now you should now see the red square appear again!

---
## Level Making And Drawing
We will be making this game's level using a tile map based system because it is easy to use and design. The main concept is that there will be a multi-line string with each line being a certain length long filled with either a 0 or a 1 (Air or a Wall). You can define the level variable like:
```javascript
const level = `0000000000000000
0000000000000000
0010000000000000
0000000000001111
0000111000000000
0000000000011111
0000000000000000
0000000000111111
0000000000011000
1110000000000000
0000000010000110
0001111111100000
0000000000000000
0000000000000000
0000000001111110
0000000000000000`;
```
This gives us the ability to create multiple levels really easily. Feel free to tweak the positions of the 1s and 0s until you see fit.

*Side Note: Levels should really be defined in external text files, but file handling would distract us too much from the making of the game so I decided just to define it in the javascript.*\
Before being able to use this level data we need to make a function that parses it into a 2D Array (Arrays within arrays (but only to one layer)). We need to:
+ Split the string up by line
+ Split each line up by character
+ Return that result
To split by line and store the result in an array we can do:
```javascript
const lines = lvl.split("\n");
```
Where lvl is the level data that we pass to the function.\
To split each line in the array by characters we need to use the **map function** *(Introduced in ES5)*.
```javascript
const characters = lines.map(l => l.split(""));
```
If you are unsure on how to use the map function refer to [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).
The final level-parsing function should look like:
```javascript
function parse(lvl){
  const lines = lvl.split("\n");
  const characters = lines.map(l => l.split(""));
  return characters;
}
```
To make the level data accessible to all of the program we need to define a variable in the global scope, at the top of the Javascript file, below the c variable, write:
```javascript
let currentLevel;
```
This just tells javascript that we want this variable to be declared but that we don't need to use it yet.\
In the window.onload function update the value of currentLevel to the return value of the parse function:
```javascript
window.onload = function(){
  currentLevel = parse(level);
  main();
}
```
Now all we need to do is draw the level!\
In the draw function we need to loop through each of the level data lines and on each line we need to loop through each character, and if the character is a 1 then we need to draw a square at the position on canvas relative to the character's position. Also before the looping we need to define a new colour for walls!
```javascript
function draw(){
  c.fillStyle = "red";
  c.fillRect(player.x, player.y, player.width, player.height);
  c.fillStyle = "black";
  for (let row = 0; row < currentLevel.length; row++) {
    for (let col = 0; col < currentLevel[0].length; col++) {
      if (currentLevel[row][col] === "1") {
        c.fillRect(col * 32, row * 32, 32, 32);
      }
    }
  }
}
```
Here we are using that **fillRect** function again, but instead of the player we are drawing each wall tile.\
If you now run the code you should see squares that match what is written in the level variable.

---
## Handling User Input
Our game right now is pretty boring as there is nothing to do, only stare at the beautifully arranged squares that should now populate you screen. To fix this we need to add a way to listen for keyboard input and move the player based on which key is pressed. At the top of the Javascript file write the line:
```javascript
let keysDown = {};
```
This is an empty **object** which will hold the keys currently pressed. If you are wondering why we need to store current keys down it is because this way allows multiple keys to be pressed at once.\
To listen for key presses, and then store that key press in the *keysDown* variable we will use an **eventListener**:
```javascript
addEventListener("keydown", function(event){
  keysDown[event.keyCode] = true;
});
```
This function executes whenever the user presses a key on their keyboard, the key pressed information is stored in the event parameter, it then sets the key to true in *keysDown*. Now we need a similar function that executes when a key is released:
```javascript
addEventListener("keyup", function(event){
  delete keysDown[event.keyCode];
});
```
It removes the entry of the released key from *keysDown*.

---

To detect and act upon keys that are pressed we need to create an input function that checks whether certain keys are pressed.
```javascript
function input(){
  if(65 in keysDown){
    player.x -= 3;
  }
  
  if(68 in keysDown){
    player.x += 3;
  }
}
```
