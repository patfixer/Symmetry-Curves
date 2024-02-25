# Six Sides Shape with Lines
```js
import './style.css'

window.addEventListener('load', function () {

  const canvas = document.getElementById('canvas')
  const ctx = canvas.getContext('2d')
  canvas.width = window.innerWidth * 0.8
  canvas.height = window.innerHeight * 0.8

  ctx.strokeStyle = "goldenrod"
  ctx.lineWidth = 10
  ctx.lineCap = "round"

  // effect setting
  let size = 150
  let pos = 20
  let sides = 6

  let maxLines = 20
  let spread = 0.5
  
  ctx.save()
  ctx.translate(canvas.width / 2, canvas.height / 2)
  ctx.scale(1, 1)
  ctx.rotate(0)

  function drawBranches1(level){
    if(level > maxLines) return
    ctx.beginPath()
    ctx.moveTo(0,0)  
    ctx.lineTo(size,0) 
    ctx.stroke() 

    ctx.translate(100,0)
    ctx.rotate((Math.PI * 2) / sides)
    ctx.scale(0.97,0.97)

    drawBranches1(level + 1)
  }
  drawBranches1(0)

})
```

# CSS Styles
```css
* {
  margin-top: 10px;
  margin-bottom: 10px;
  margin-left: 10px;
  margin-right: 10px;
  padding: 0;
  box-sizing: border-box;
}

canvas{
  border: 5px solid rgb(240, 197, 8);
  position: absolute;
  background: rgb(78, 116, 116);
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
```

# HTML Page
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <canvas id="canvas"></canvas>
    <script type="module" src="/main.js"></script>
  </body>
</html>

```

