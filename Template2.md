# Full Code Making changes
```js
const canvas = document.getElementById("canvas1")
const ctx = canvas.getContext("2d")

canvas.width = window.innerWidth / 1
canvas.height = window.innerHeight / 1

ctx.fillStyle = ""
ctx.strokeStyle = "red"
ctx.lineWidth = 2


class Particles{
    constructor(effect){
        this.effect = effect
        this.x = Math.floor(Math.random() * this.effect.width);
        this.y = Math.floor(Math.random() * this.effect.height);
        this.speedX = Math.random() * 1 + 1.5; // * 5 - 2.5
        this.speedY = Math.random() * 1 + 1.5; // * 5 - 2.5
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
        this.angle += 4.5;
        this.x += this.speedX * Math.sin(this.angle) * 20;
        this.y += this.speedY * Math.cos(this.angle) * 20;
        this.historyLines.push({x: this.x, y: this.y})
        if (this.historyLines.length > this.maxLength){
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


## In Particles class
With-in this particles class the speed and the position with these values here 
make the particles move downward at a right angle that moves off canvas looks like shooting stars. 
`this.speedX = Math.random() * 1 + 1.5;`
`this.speedY = Math.random() * 1 + 1.5;`


## In update() method
In here is the position of the particles speed - the angle which is inside the `Math.sin()` method.
`this.x += this.speedY - Math.sin(this.angle);`
`this.y += this.speedY - Math.sin(this.angle);`


```js
 class Particles{
    constructor(effect){
        this.effect = effect
        this.x = Math.floor(Math.random() * this.effect.width);
        this.y = Math.floor(Math.random() * this.effect.height);
        this.speedX = Math.random() * 1 + 1.5; // * 5 - 2.5
        this.speedY = Math.random() * 1 + 1.5; // * 5 - 2.5
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
        this.angle += 1.5;
        this.x += this.speedY - Math.sin(this.angle);
        this.y += this.speedY - Math.sin(this.angle);
        this.historyLines.push({x: this.x, y: this.y})
        if (this.historyLines.length > this.maxLength){
            this.historyLines.shift();
        }
    }
}
```

## Changes inside class Particles{}
This display really cool circles that spin in a pattern 
`this.speedX = Math.random() * 1 + 1.5;`
`this.speedY = Math.random() * 1 + 1.5;`

## Changes inside update()
This is when I changed angle to `+= 1.5` from `+= 0.5` 
`this.angle += 1.5;` 
`this.x += this.speedX * Math.sin(this.angle) * 20;`
`this.y += this.speedY * Math.cos(this.angle) * 20;`

```js
class Particles{
    constructor(effect){
        this.effect = effect
        this.x = Math.floor(Math.random() * this.effect.width);
        this.y = Math.floor(Math.random() * this.effect.height);
        this.speedX = Math.random() * 1 + 1.5; // * 5 - 2.5
        this.speedY = Math.random() * 1 + 1.5; // * 5 - 2.5
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
    update(){
        this.angle += 1.5;
        this.x += this.speedX * Math.sin(this.angle) * 20;
        this.y += this.speedY * Math.cos(this.angle) * 20;
        this.historyLines.push({x: this.x, y: this.y})
        if (this.historyLines.length > this.maxLength){
            this.historyLines.shift();
        }
    }
}
```

