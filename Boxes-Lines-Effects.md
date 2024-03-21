
# More Effects Wiggly
```js
   // Pushing the lines behined the boxes 
   // And I can make lines wiggly + Math.random() * 3 - 1.5
    update(){
        this.x += this.speedX + Math.random() * 15 - 7.0;
        this.y += this.speedY + Math.random() * 15 - 7.0;
        this.historyLines.push({x: this.x, y: this.y})
    }
```


## Random length of the line
```js
 this.maxLength = Math.floor(Math.random() * 100 + 10)  //  Random length of the line
```


## Adding this angle
Makes the lines more wavy moving in a different dicrection: 
`this.angle = 0;` First you create a variable in the Particles class `constructor(){}`
`this.angle += 0.5;` This will go in the `update(){}` method 
`this.x += this.speedX + Math.sin(this.angle)` And added it to the speedX and speedY with Math sin method inside it's parameter like this-`sin(this.angle)`.

## Playing with the position
Can give you a different effect by changing the operators `- * +` 
`this.x += this.speedX - Math.sin(this.angle);`
`this.y += this.speedY - Math.sin(this.angle);` Or 

## Draws circles up and down the canvas
Some of the moving circles will slow move up or down on canvas 
```js
this.x += this.speedX * Math.sin(this.angle) * 3;
this.y += this.speedY + Math.cos(this.angle) * 10;
```

## Or Change the speed inside the constructor()
If you increase the number will make circles bigger:
`this.speedX = Math.random() * 15 - 2.5;`
`this.speedY = Math.random() * 15 - 2.5;`
 
`constructor(effect)` The effect inside the parameter is holding a reference in memory
```js

  constructor(effect){
        this.effect = effect
        this.x = Math.floor(Math.random() * this.effect.width);
        this.y = Math.floor(Math.random() * this.effect.height);
        this.speedX = Math.random() * 5 - 2.5;
        this.speedY = Math.random() * 5 - 2.5;
        this.historyLines = [{x: this.x, y: this.y}]; // Putting positions on the lines
        this.maxLength = Math.floor(Math.random() * 50 + 10)  //  Random length of the line
        this.angle = 0;
    }
```


## Making this lines move in a different dicrection
```js
class Particles{
    constructor(effect){
        this.effect = effect
        this.x = Math.floor(Math.random() * this.effect.width);
        this.y = Math.floor(Math.random() * this.effect.height);
        this.speedX = Math.random() * 5 - 2.5;
        this.speedY = Math.random() * 5 - 2.5;
        this.historyLines = [{x: this.x, y: this.y}]; // Putting positions on the lines
        this.maxLength = Math.floor(Math.random() * 50 + 10)  //  Random length of the line
        this.angle = 0;
    }
    // creating lines move behine the boxes
    draw(context){ 
        context.fillRect(this.x, this.y , 10, 10)
        context.beginPath(); 
        context.moveTo(this.historyLines[0].x, this.historyLines[0].y);
        for (let i = 0; i < this.historyLines.length; i++){
            context.lineTo(this.historyLines[i].x, this.historyLines[i].y)
        }
        context.stroke()
    }
   // Pushing the lines behined the boxes 
   // And I can make lines wiggly + Math.random() * 3 - 1.5  or  15 - 7.5;
    update(){
        this.angle += 0.5;
        this.x += this.speedX + Math.sin(this.angle) 
        this.y += this.speedY + Math.random() * 15 - 7.5;
        this.historyLines.push({x: this.x, y: this.y})
        if (this.historyLines.length > this.maxLength){
            this.historyLines.shift();
        }
    }
}

```


# The Boxes move outside the canvas 
```js

const canvas = document.getElementById("canvas1")
const ctx = canvas.getContext("2d")

canvas.width = window.innerWidth
canvas.height = window.innerHeight

ctx.fillStyle = "green"
ctx.strokeStyle = "red"
ctx.lineWidth = 2


class Particles{
    constructor(effect){
        this.effect = effect
        this.x = Math.floor(Math.random() * this.effect.width);
        this.y = Math.floor(Math.random() * this.effect.height);
        this.speedX = Math.random() * 5 - 2.5;
        this.speedY = Math.random() * 5 - 2.5;
        this.historyLines = [{x: this.x, y: this.y}]; // Putting positions on the lines
        this.maxLenght = 30   //  max number of seconds
    }
    // creating lines move behine the boxes
    draw(context){
        context.fillRect(this.x, this.y , 10, 10)
        context.beginPath(); 
        context.moveTo(this.historyLines[0].x, this.historyLines[0].y);
        for (let i = 0; i < this.historyLines.length; i++){
            context.lineTo(this.historyLines[i].x, this.historyLines[i].y)
        }
        context.stroke()
    }
   // Pushing the lines behined the boxes 
   /*This method `this.historyLines.shift()` moves the lines in different patterns, 
   and here Im add line to the array `shift()` removes the old segmeant. */ 
    update(){
        this.x += this.speedX + Math.random() * 15 - 7.5;
        this.y += this.speedY + Math.random() * 15 - 7.5;
        this.historyLines.push({x: this.x, y: this.y})
        if (this.historyLines.length > this.maxLenght){
            this.historyLines.shift();
        }
    }
}


class Effect {
    constructor(width, height){
        this.width = width
        this.height = height
        this.particles = []
        this.numberParticles = 50
        this.init()
    }

    init(){
        for (let i = 0; i < this.numberParticles; i++){
            this.particles.push(new Particles(this))
        }
    }

    render(context){
        this.particles.forEach(parts => {
            parts.draw(context);
            parts.update();
        })

    }
}

const effect = new Effect(canvas.width, canvas.height);

console.log(effect)


function animate(){
    ctx.clearRect(0, 0, canvas.width, canvas.height)
    effect.render(ctx)
    requestAnimationFrame(animate)
}

animate()
```
