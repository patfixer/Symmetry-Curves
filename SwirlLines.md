# Swirl lines 
```js
window.addEventListener("load", function () {
  const canvas = document.getElementById("canvas-one");
  const ctx = canvas.getContext("2d");

  canvas.width = this.window.innerWidth * 0.8;
  canvas.height = this.window.innerHeight * 0.8;

  ctx.fillStyle = "darkred";
  ctx.strokeStyle = "lightblue";
  ctx.lineWidth = 15;
  ctx.lineCap = "round";

  //effect
  let size = 200;
  let sides = 30;
  let maxLevel = 90

  // saved line size
  ctx.save();
  ctx.translate(canvas.width / 2, canvas.height / 2);
  ctx.scale(1, 1);
  ctx.rotate(0);
  //ctx.fillRect(0, 0, canvas.width, canvas.height)

  function drawBranch(level) {
    if (level > maxLevel) return
    ctx.beginPath(); // start drawing
    ctx.moveTo(0, 0);// start point
    ctx.lineTo(size, 0);// end point
    ctx.stroke();// draw line
    ctx.translate(100,0)
    ctx.rotate(0.6)// rotate by 90
    ctx.scale(0.8, 0.8)

    drawBranch(level + 1)
    
  }

  drawBranch(0)

  ctx.restore();
});
```
