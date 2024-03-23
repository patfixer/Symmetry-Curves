
# Boxes Template
Just renders boxes on screen only.
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
    }

    draw(context){
        context.fillRect(this.x, this.y , 20, 20)
    }
}


class Effect {
    constructor(width, height){
        this.width = width
        this.height = height
        this.particles = []
        this.numberParticles = 50
    }

    init(){
       
        for (let i = 0; i < this.numberParticles; i++){
            this.particles.push(new Particles(this))
        }
    }

    render(context){
        this.particles.forEach(parts => {
            parts.draw(context);
        })
    }
}

const effect = new Effect(canvas.width, canvas.height);
effect.init()
effect.render(ctx)
console.log(effect)
```


# Moving shapes or boxes on screen 
But slowly the boxes float outside the canvas 
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
    }

    draw(context){
        context.fillRect(this.x, this.y , 20, 20)
    }

    update(){
        this.x += this.speedX;
        this.y += this.speedY;
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


### ############################

# Lines Moving Behined Boxes
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
        this.x += this.speedX;
        this.y += this.speedY;
        this.historyLines.push({x: this.x, y: this.y})
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

#  And I can make lines wiggly 
```js
// + Math.random() * 3 - 1.5
    update(){
        this.x += this.speedX + Math.random() * 3 - 1.5;
        this.y += this.speedY;
        this.historyLines.push({x: this.x, y: this.y})
    }
```


