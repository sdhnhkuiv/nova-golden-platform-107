# 打砖块



## 说明

用挡板接住弹球，消除所有砖块即获胜。左右方向键或拖动鼠标控制挡板。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>打砖块</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#1a237e;color:white}

canvas{background:#111;border:2px solid #448aff}

</style></head>

<body>

<h2>🧱 打砖块</h2>

<canvas id="c" width="500" height="400"></canvas>

<script>

const c=document.getElementById("c"),ctx=c.getContext("2d");

let paddle={x:200,w:80,h:10,y:380},ball={x:250,y:200,dx:3,dy:-3,r:6};

let bricks=[],score=0,lives=3;

for(let r=0;r<5;r++)for(let col=0;col<8;col++)bricks.push({x:col*60+10,y:r*25+10,w:50,h:20,alive:true});



function loop(){

  ctx.clearRect(0,0,500,400);

  // 砖块

  bricks.forEach(b=>{if(b.alive){ctx.fillStyle="#ff9800";ctx.fillRect(b.x,b.y,b.w,b.h);}});

  // 挡板

  ctx.fillStyle="#4CAF50";ctx.fillRect(paddle.x,paddle.y,paddle.w,paddle.h);

  // 球

  ball.x+=ball.dx;ball.y+=ball.dy;

  if(ball.x<0||ball.x>500)ball.dx*=-1;if(ball.y<0)ball.dy*=-1;

  if(ball.y>400){lives--;if(lives<=0){alert("Game Over! 得分:"+score);location.reload();}ball.x=250;ball.y=200;ball.dx=3;ball.dy=-3;}

  ctx.beginPath();ctx.arc(ball.x,ball.y,ball.r,0,Math.PI*2);ctx.fillStyle="white";ctx.fill();

  // 碰撞检测

  if(ball.y+ball.r>paddle.y&&ball.x>paddle.x&&ball.x<paddle.x+paddle.w){ball.dy*=-1;ball.y=paddle.y-ball.r;}

  bricks.forEach(b=>{if(b.alive&&ball.x>b.x&&ball.x<b.x+b.w&&ball.y>b.y&&ball.y<b.y+b.h){b.alive=false;ball.dy*=-1;score+=10;}});

  if(bricks.every(b=>!b.alive)){alert("🎉 你赢了! 得分:"+score);location.reload();}

  requestAnimationFrame(loop);

}

document.addEventListener("mousemove",e=>{let r=c.getBoundingClientRect();paddle.x=e.clientX-r.left-paddle.w/2;paddle.x=Math.max(0,Math.min(420,paddle.x));});

loop();

</script></body></html>

```

