# Wavys Lines or changing

add this to speedX and speedY
`this.speedModifier = Math.floor(Math.random() * 3 + 1);`

like this:
`this.x += this.speedX * this.speedModifier;`
`this.y += this.speedY * this.speedModifier;`


```js

const canvas = document.getElementById("canvas1")
const ctx = canvas.getContext("2d")
console.log(ctx)

canvas.width = window.innerWidth 
canvas.height = window.innerHeight 

ctx.fillStyle = "lightblue"
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
        this.maxLength = Math.floor(Math.random() * 200 + 10)  //  Random length of the line
        this.angle = 0;
    }
    // creating lines move behine the boxes
    draw(context){ 
        // context.fillRect(this.x, this.y , 10, 10)
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
        // helper 
        let x = Math.floor(this.x / this.effect.cellSize);
        let y = Math.floor(this.y / this.effect.cellSize);
        let index = y * this.effect.cols + x;
        this.angle = this.effect.flowField[index];


        // Creating the speed corrdinates effect movement of the wavys
        // slicing canvas into an individual grid
        this.speedX = Math.cos(this.angle);
        this.speedY = Math.sin(this.angle);
        this.x += this.speedX;
        this.y += this.speedY;


        this.historyLines.push({x: this.x, y: this.y})
        if (this.historyLines.length > this.maxLength){
            this.historyLines.shift(); // remves the first line
        } 
    }
}


class Effect {
    constructor(width, height){
        this.width = width
        this.height = height
        this.particles = []
        this.numberParticles = 150

        // below is apart of flow field
        this.cellSize = 20
        this.rows;
        this.cols;
        this.flowField = [];
        this.curve = 0.5;  //5 - 2.5 transforms the lines differently
        this.zoom = 3.1; //0.1 for smooth wavys. 3.1 for straighter wiggly lines 
        this.init()
    }

    init(){
       // Creating wavy flow field cell by cell 
       this.rows = Math.floor(this.height / this.cellSize);
       this.cols = Math.floor(this.width / this.cellSize);
       this.flowField = [];
       // Cycling through one by one, y and x rows and cols 
       for (let y = 0; y < this.rows; y++){
         for (let x = 0; x < this.cols; x++){
            let angle = (Math.cos(x * this.zoom) + Math.sin(y * this.zoom)) * this.curve; // * 5 - 2.5
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
    effect.render(ctx)
    requestAnimationFrame(animate)
}

animate()
```

# Changing Update()
Adding a reset() Method
This slicing of the individual grid I put this inside an if statement

```js
this.timer--;
if (this.timer >= 1) {
    // helper goes here
} eles if (this.historyLines.length > 1) {
    this.historyLines.shift(); // That if you add the helper 
}
```

```js
 update() {
    this.timer--;
    if (this.timer >= 1) {
      // helper
      let x = Math.floor(this.x / this.effect.cellSize);
      let y = Math.floor(this.y / this.effect.cellSize);
      let index = y * this.effect.cols + x;
      this.angle = this.effect.flowField[index];

      // Creating the speed corrdinates effect movement of the wavys
      // slicing canvas into an individual grid
      this.speedX = Math.cos(this.angle);
      this.speedY = Math.sin(this.angle);
      this.x += this.speedX * this.speedModifier;
      this.y += this.speedY * this.speedModifier;

      this.historyLines.push({ x: this.x, y: this.y });
      if (this.historyLines.length > this.maxLength) {
        this.historyLines.shift(); // remves the first line
      }
    }
  }
```


# To Reset the lines
```js
class Particles {
  constructor(effect) {
    this.effect = effect;
    this.x = Math.floor(Math.random() * this.effect.width);
    this.y = Math.floor(Math.random() * this.effect.height);
    this.speedX;
    this.speedY;

    // add this to speedX and speedY
    this.speedModifier = Math.floor(Math.random() * 5 + 1);

    this.historyLines = [{ x: this.x, y: this.y }]; // Putting positions on the lines
    this.maxLength = Math.floor(Math.random() * 200 + 10); //  Random length of the line
    this.angle = 0;

    this.timer = this.maxLength * 2;
  }
  // creating lines move behine the boxes
  draw(context) {
    // context.fillRect(this.x, this.y , 10, 10)
    context.beginPath();
    context.moveTo(this.historyLines[0].x, this.historyLines[0].y);
    for (let i = 0; i < this.historyLines.length; i++) {
      context.lineTo(this.historyLines[i].x, this.historyLines[i].y);
    }
    context.stroke();
  }
  // Pushing the lines behined the boxes
  // And I can make lines wiggly + Math.random() * 3 - 1.5  or  15 - 7.5;
  update() {
    this.timer--;
    if (this.timer >= 1) {
      // helper
      let x = Math.floor(this.x / this.effect.cellSize);
      let y = Math.floor(this.y / this.effect.cellSize);
      let index = y * this.effect.cols + x;
      this.angle = this.effect.flowField[index];

      // Creating the speed corrdinates effect movement of the wavys
      // slicing canvas into an individual grid
      this.speedX = Math.cos(this.angle);
      this.speedY = Math.sin(this.angle);
      this.x += this.speedX * this.speedModifier;
      this.y += this.speedY * this.speedModifier;

      this.historyLines.push({ x: this.x, y: this.y });
      if (this.historyLines.length > this.maxLength) {
        this.historyLines.shift(); // remves the first line
      }
    } else if (this.historyLines.length > 1){
        this.historyLines.shift();
    }else{
        this.reset();
    }
  }

  reset() {
    this.x = Math.floor(Math.random() * this.effect.width);
    this.y = Math.floor(Math.random() * this.effect.height);
    this.historyLines = [{ x: this.x, y: this.y }];
    this.timer = this.maxLength * 2;
  }
}
```     
