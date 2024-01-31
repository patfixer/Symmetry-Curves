
# Cool Symmetrical Moving Curves
Symmetrical or Symmetrical Curves that can face any which way or move in a straight line, by change  `this.curve = 5.1;` or zoom  `this.zoom = .30;`; Is what its call not sure if this is the right name `this.zoom` ?? 
```js
const canvas = document.getElementById("canvas1");
const ctx = canvas.getContext("2d");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

// Canvas setting
ctx.fillStyle = "white";
ctx.strokeStyle = "yellow";
ctx.lineWidth = 1;

let one = Math.floor() * 5 - 2.5;
let two = Math.floor() * 5 - 2.5;

class Particle {
  constructor(effect) {
    this.effect = effect;
    this.x = Math.floor(Math.random() * this.effect.width);
    this.y = Math.floor(Math.random() * this.effect.height);
    this.speedX;
    this.speedY;

    this.speedModifier = Math.floor(Math.random() * 5 + 1);
    this.history = [{ x: this.x, y: this.y }];
    this.maxLength = Math.floor(Math.random() * 100 + 10);
    this.angle = 0;
    this.timer = this.maxLength * 2;
  }

  draw(context) {
    context.beginPath();
    context.moveTo(this.history[0].x, this.history[0].y);
    for (let i = 0; i < this.history.length; i++) {
      context.lineTo(this.history[i].x, this.history[i].y);
    }
    context.stroke();
  }

  update() {
    this.timer--;
    if (this.timer >= 1) {
      let x = Math.floor(this.x / this.effect.cellSize);
      let y = Math.floor(this.y / this.effect.cellSize);
      let index = y * this.effect.cols + x;
      this.angle = this.effect.flowField[index];

      this.speedX = Math.cos(this.angle);
      this.speedY = Math.sin(this.angle);
      this.x += this.speedX * this.speedModifier;
      this.y += this.speedY * this.speedModifier;

      this.history.push({ x: this.x, y: this.y });
      if (this.history.length > this.maxLength) {
        this.history.shift();
      }
    } else if(this.history.length > 1){
      this.history.shift();
    } else {
      this.reset()
    }
  }

  reset() {
    this.x = Math.floor(Math.random() * this.effect.width);
    this.y = Math.floor(Math.random() * this.effect.height);
    this.history = [{ x: this.x, y: this.y }];
    this.timer = this.maxLength * 2
  }
}

class Effect {
  constructor(width, height) {
    this.width = width;
    this.height = height;
    this.particles = [];
    this.numberOfParticles = 300;
    this.cellSize = 20;
    this.rows;
    this.cols;
    this.flowField = [];
    this.curve = 5.1; /* 2.4 Makes curves i like 5.1 */
    this.zoom = .30; /*0.4-give you more of a curves. 0.0-give you more of a strate lines*/
    this.init();
  }

  init() {
    // Create Flow Field
    this.rows = Math.floor(this.height / this.cellSize);
    this.cols = Math.floor(this.width / this.cellSize);
    this.flowField = [];
    for (let y = 0; y < this.rows; y++) {
      for (let x = 0; x < this.cols; x++) {
        let angle =
          (Math.cos(x * this.zoom) + Math.sin(y * this.zoom)) * this.curve;
        this.flowField.push(angle);
      }
      // console.log(this.flowField);
    }

    // Create Particles
    for (let i = 0; i < this.numberOfParticles; i++) {
      this.particles.push(new Particle(this));
    }
  }

  render(context) {
    this.particles.forEach((particles) => {
      particles.draw(context);
      particles.update();
    });
  }
}

const effect = new Effect(canvas.width, canvas.height);
console.log(effect);

function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  effect.render(ctx);
  requestAnimationFrame(animate);
}
animate();
```
