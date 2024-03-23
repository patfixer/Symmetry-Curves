
# Inside Class Effect{}
Since Im Inheriting from the `class Particles{}`, I've created a flow field inside `init()` method. 

particles inside class Effect to scan the cellSize by rows and cols

`this.cellSize = 20`
`this.rows;`
`this.cols;`

`this.rows = Math.floor(this.height / this.cellSize);`
`this.cols = Math.floor(this.width / this.cellSize);`


Cycling through row by row and cols by cols this is repeated

This anlge value creates that wavy pattern.
```js
for (let y = 0; y < this.rows; y++){
        for (let x = 0; x < this.cols; x++){
            let angle = Math.cos(x) + Math.sin(y); // anlge value
            this.flowField.push(angle);
        }
}
```

# Creating Particles Inside Class Effect
```js
class Effect {
    constructor(width, height){
        this.width = width
        this.height = height
        this.particles = []
        this.numberParticles = 50

        // below is apart of flow field
        this.cellSize = 20
        this.rows;
        this.cols;
        this.init()
    }

    init(){
       // Creating wavy flow field 
       this.rows = Math.floor(this.height / this.cellSize);
       this.cols = Math.floor(this.width / this.cellSize);
       this.flowField = [];
       // Cycling through one by one, y and x rows and cols 
       for (let y = 0; y < this.rows; y++){
        for (let x = 0; x < this.cols; x++){
            let angle = Math.cos(x) + Math.sin(y); // anlge value
            this.flowField.push(angle);
        }
        
       }
        // Creating Particles 
        for (let i = 0; i < this.numberParticles; i++){
            this.particles.push(new Particles(this))
        }
    }
}
```
Random length of the line add this to maxLength  `* 200 + 10)`.
`this.maxLength = Math.floor(Math.random() * 50 + 10)`.

Chaging the angle value with `Math.cos(x) + Math.sin(y);` And adding * 0.5 at the end
`let angle = (Math.cos(x) + Math.sin(y)) * 0.5;` 

Or create a variable `this.curve = 0.5` and then add it to angle. 
like this- `let angle = (Math.cos(x) + Math.sin(y)) * this.curve;`

## Curvies here
Or create a value called `this.zoom = 0.1`

Adding * 0.1 beside x and y, this makes curvy wavies
`let angle = (Math.cos(x * 0.1) + Math.sin(y * 0.1)) * this.curve;`

# Creating A Flow Field
```js

const canvas = document.getElementById("canvas1")
const ctx = canvas.getContext("2d")
console.log(ctx)

canvas.width = window.innerWidth 
canvas.height = window.innerHeight 

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
        this.angle += 1.5;
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

        // below is apart of flow field
        this.cellSize = 20
        this.rows;
        this.cols;
        this.init()
    }

    init(){
       // Creating wavy flow field 
       this.rows = Math.floor(this.height / this.cellSize);
       this.cols = Math.floor(this.width / this.cellSize);
       this.flowField = [];
       // Cycling through one by one, y and x rows and cols 
       for (let y = 0; y < this.rows; y++){
        for (let x = 0; x < this.cols; x++){
            let angle = Math.cos(x) + Math.sin(y); // anlge value
            this.flowField.push(angle);
        }
        
       }
        // Creating Particles 
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
    //effect.render(ctx)
    requestAnimationFrame(animate)
}

animate()
```
