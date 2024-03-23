
## Where I Left off
```js
const canvas = document.getElementById("canvas1");
const ctx = canvas.getContext("2d");
// console.log(ctx);

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

// ctx.fillStyle = "blue";
// ctx.strokeStyle = "lightblue";
ctx.lineWidth = 2;

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
    this.colors = ["#ffc400", "#3cff00", "#ff0000", "#b5f505", "#0077ff", "#ff00b3"];
    this.color = this.colors[Math.floor(Math.random() * this.colors.length)]
  }
  // creating lines move behine the boxes
  draw(context) {
    // context.fillRect(this.x, this.y , 10, 10)
    context.beginPath();
    context.moveTo(this.historyLines[0].x, this.historyLines[0].y);
    for (let i = 0; i < this.historyLines.length; i++) {
      context.lineTo(this.historyLines[i].x, this.historyLines[i].y);
    }
    context.strokeStyle = this.color
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

class Effect {
  constructor(canvas) {
    this.canvas = canvas;
    this.width = this.canvas.width;
    this.height = this.canvas.height;
    this.particles = [];
    this.numberParticles = 300;

    // below is apart of flow field
    this.cellSize = 30; // Now this is apart of the gridGrid()
    this.rows;
    this.cols;
    this.flowField = [];

    /* curve 5 - 2.5 transforms the lines differently. Curve Right 0.3. Curve Left 3*/
    this.curve = 1; 
    /*zoom 0.1 for smooth wavys. 3.1 for straighter wiggly lines
    or zomm 0.05.  Curves to the left 0.02. */
    this.zoom = 0.25; 
    this.debug = false;
    this.init();

    window.addEventListener("keydown", e => {
        if (e.key === 'd') this.debug = !this.debug;       
    })

    window.addEventListener("resize", e => {
      this.resize(e.target.innerWidth, e.target.innerHeight);
    })
  }

  init() {
    // Creating wavy flow field cell by cell
    this.rows = Math.floor(this.height / this.cellSize);
    this.cols = Math.floor(this.width / this.cellSize);
    this.flowField = [];
    // Cycling through one by one, y and x rows and cols
    for (let y = 0; y < this.rows; y++) {
      for (let x = 0; x < this.cols; x++) {
        let angle = (Math.cos(x * this.zoom) + Math.sin(y * this.zoom)) * this.curve; // * 5 - 2.5
        this.flowField.push(angle);
      }
    }
    this.particles = [];
    // Creating Particles
    for (let i = 0; i < this.numberParticles; i++) {
      this.particles.push(new Particles(this));
    }
  }

  // Draw grid 
  drawGrid(context){
    context.save();
    context.strokeStyle = "green"
    context.lineWidth = 1.1
    for (let c = 0; c < this.cols; c++){
        context.beginPath();
        context.moveTo(c * this.cellSize, 0);
        context.lineTo(c * this.cellSize, this.height);
        context.stroke();
    }

    for (let r = 0; r < this.rows; r++){
        context.beginPath();
        context.moveTo(0, r * this.cellSize);
        context.lineTo(this.width, this.cellSize * r);
        context.stroke();
    }
    context.restore();
  }

  resize(width, height){
    this.canvas.width = width;
    this.canvas.height = height;
    this.width = this.canvas.width;
    this.height = this.canvas.height;
    // this.init();
  }

  render(context) {
    if (this.debug) this.drawGrid(context);
    this.particles.forEach((parts) => {
      parts.draw(context);
      parts.update();
    });
  }
}

const effect = new Effect(canvas);

// console.log(effect);

function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  effect.render(ctx);
  requestAnimationFrame(animate);
}

animate();

```
