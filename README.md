<!DOCTYPE html>
<html>
<head>
<title>Tap The Box - Rahul</title>

<style>

body{
 text-align:center;
 font-family:Arial;
 background:#222;
 color:white;
}

#game{
 width:350px;
 height:400px;
 background:#444;
 margin:auto;
 position:relative;
 overflow:hidden;
 border:3px solid white;
 border-radius:10px;
}

#box{
 width:50px;
 height:50px;
 background:red;
 position:absolute;
 border-radius:10px;
 cursor:pointer;
}

button{
 padding:10px 15px;
 margin:5px;
 font-size:16px;
}

footer{
 margin-top:20px;
 color:#aaa;
}

</style>

</head>

<body>

<h1>🎮 Tap The Box</h1>

<h2>🏆 Score: <span id="score">0</span></h2>
<h2>🥇 Highest Score: <span id="highScore">0</span></h2>
<h2>✅ Right Clicks: <span id="right">0</span></h2>
<h2>❌ Wrong Clicks: <span id="wrong">0</span></h2>
<h2>🖱 Total Clicks: <span id="total">0</span></h2>
<h2>⏱ Time: <span id="time">0</span> sec</h2>


<div id="game">
<div id="box"></div>
</div>


<h3>Select Level</h3>

<button onclick="startGame('easy')">
🟢 Easy
</button>

<button onclick="startGame('medium')">
🟡 Medium
</button>

<button onclick="startGame('hard')">
🔴 Hard
</button>


<footer>
<p>Tap The Box Game</p>
<p>Developed by Rahul</p>
</footer>


<script>

let score=0;
let right=0;
let wrong=0;
let total=0;

let gameTimer;
let stopwatch;

let seconds=0;

let highScore=localStorage.getItem("highScore") || 0;


let box=document.getElementById("box");
let game=document.getElementById("game");

document.getElementById("highScore").innerHTML=highScore;



// Sound

let audio=new AudioContext();


function beep(freq){

 if(audio.state=="suspended"){
  audio.resume();
 }


 let oscillator=audio.createOscillator();
 let gain=audio.createGain();


 oscillator.type="square";
 oscillator.frequency.value=freq;


 oscillator.connect(gain);
 gain.connect(audio.destination);


 gain.gain.value=0.1;


 oscillator.start();


 setTimeout(()=>{
  oscillator.stop();
 },150);

}



// Move box

function moveBox(){

 let x=Math.random()*300;
 let y=Math.random()*350;


 box.style.left=x+"px";
 box.style.top=y+"px";

}



// Right click

box.onclick=function(e){

 e.stopPropagation();


 score++;
 right++;
 total++;


 document.getElementById("score").innerHTML=score;
 document.getElementById("right").innerHTML=right;
 document.getElementById("total").innerHTML=total;


 beep(900);


 if(score>highScore){

  highScore=score;

  localStorage.setItem("highScore",highScore);

  document.getElementById("highScore").innerHTML=highScore;

 }


 moveBox();

};



// Wrong click

game.onclick=function(){

 wrong++;
 total++;


 document.getElementById("wrong").innerHTML=wrong;
 document.getElementById("total").innerHTML=total;


 beep(200);

};



// Start Game

function startGame(level){

 audio.resume();


 score=0;
 right=0;
 wrong=0;
 total=0;
 seconds=0;


 document.getElementById("score").innerHTML=0;
 document.getElementById("right").innerHTML=0;
 document.getElementById("wrong").innerHTML=0;
 document.getElementById("total").innerHTML=0;
 document.getElementById("time").innerHTML=0;



 let speed;


 if(level=="easy"){
  speed=2000;
 }

 else if(level=="medium"){
  speed=1000;
 }

 else{
  speed=500;
 }



 beep(500);


 moveBox();


 clearInterval(gameTimer);

 gameTimer=setInterval(moveBox,speed);



 // Stopwatch

 clearInterval(stopwatch);


 stopwatch=setInterval(function(){

  seconds++;

  document.getElementById("time").innerHTML=seconds;

 },1000);


}


</script>


</body>
</html>
