# Breaches 
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
  let sides = 5;
  let maxLevel = 1;
  let spread = 0.5;
  let branches = 2;

  // saved line size
  ctx.save();
  ctx.translate(canvas.width / 2, canvas.height / 2);
  ctx.scale(1, 1);
  ctx.rotate(0);
  //ctx.fillRect(0, 0, canvas.width, canvas.height)

  function drawBranch(level) {
    if (level > maxLevel) return;
    ctx.beginPath();
    ctx.moveTo(0, 0);
    ctx.lineTo(size, 0);
    ctx.stroke();
    for (let i = 0; i < branches; i++) {
      ctx.save();
      ctx.translate((size / branches) * i, 0);
      ctx.rotate(spread);
      ctx.scale(0.8, 0.8);
      drawBranch(level + 1);
      ctx.restore();

      ctx.save();
      ctx.translate((size / branches) * i, 0);
      ctx.rotate(-spread);
      ctx.scale(0.8, 0.8);
      drawBranch(level + 1);
      ctx.restore();
    }
  }

  drawBranch(0);

  ctx.restore();
});
```
