안녕하세요 레나정 대표님, 최다은 변호사님. 

늘 여러가지 의미있고 재미있는 이야기를 해주셔서 감사해요

오늘도 행복하시고, 아래 게임을 하시며서 스트레스 푸세요

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Dino Game</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
margin:0;
background:#f7f7f7;
display:flex;
justify-content:center;
align-items:center;
height:100vh;
}

canvas{
border:2px solid #333;
background:white;
touch-action:none;
}
</style>
</head>

<body>

<canvas id="game"></canvas>

<script>
const c = document.getElementById("game");
const ctx = c.getContext("2d");

function resize(){
c.width = Math.min(innerWidth-20,600);
c.height = 200;
}
resize();
addEventListener("resize",resize);

let dino={x:50,y:140,w:30,h:40,vy:0};
let gravity=0.7;
let jumping=false;

let cactus=[];
let score=0;
let speed=4;
let gameOver=false;

document.addEventListener("keydown",e=>{
if(e.code==="Space") jump();
});

c.addEventListener("touchstart",e=>{
e.preventDefault();
jump();
});

function jump(){
if(!jumping && !gameOver){
dino.vy=-13;
jumping=true;
}
}

function spawn(){
if(!gameOver) cactus.push({x:c.width,y:150,w:20,h:30});
}

setInterval(spawn,1400);

function drawDino(){
ctx.fillStyle="red";
ctx.fillRect(dino.x,dino.y,dino.w,dino.h);
}

function collide(a,b){
return(
a.x<b.x+b.w &&
a.x+a.w>b.x &&
a.y<b.y+b.h &&
a.y+a.h>b.y
);
}

function loop(){
ctx.clearRect(0,0,c.width,c.height);

if(!gameOver){
dino.vy+=gravity;
dino.y+=dino.vy;

if(dino.y>=140){
dino.y=140;
jumping=false;
}
}

drawDino();

ctx.fillStyle="black";
cactus.forEach((ca,i)=>{
if(!gameOver) ca.x-=speed;
ctx.fillRect(ca.x,ca.y,ca.w,ca.h);

if(collide(dino,ca)){
gameOver=true;
setTimeout(()=>{
alert("GAME OVER\nScore: "+score);
location.reload();
},50);
}

if(ca.x<0){
cactus.splice(i,1);
score++;
}
});

ctx.fillText("Score: "+score,10,20);

requestAnimationFrame(loop);
}

loop();
</script>

</body>
</html>
