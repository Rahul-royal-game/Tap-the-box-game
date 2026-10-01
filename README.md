<!DOCTYPE html>
<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport"
content="width=device-width, initial-scale=1.0">

<title>Tap The Box - Rahul</title>

<style>

*{
    box-sizing:border-box;
}

body{
    margin:0;
    padding:15px;
    text-align:center;
    font-family:Arial,sans-serif;
    background:#222;
    color:white;
}

h1{
    margin-top:10px;
    margin-bottom:10px;
}

h2{
    margin:8px 0;
    font-size:20px;
}

#nameScreen{
    max-width:400px;
    margin:40px auto;
    background:#333;
    padding:25px;
    border-radius:15px;
    border:2px solid #777;
}

#nameScreen h1{
    font-size:30px;
}

#playerName{
    width:90%;
    padding:14px;
    font-size:18px;
    border:none;
    border-radius:8px;
    margin:10px 0;
    text-align:center;
}

button{
    padding:12px 18px;
    margin:5px;
    font-size:16px;
    border:none;
    border-radius:8px;
    cursor:pointer;
    font-weight:bold;
}

#startButton{
    background:#00c853;
    color:white;
    width:90%;
}

.levelButtons button:nth-child(1){
    background:#19a974;
    color:white;
}

.levelButtons button:nth-child(2){
    background:#e6b800;
    color:black;
}

.levelButtons button:nth-child(3){
    background:#e53935;
    color:white;
}

#gameScreen{
    display:none;
}

#currentPlayer{
    color:#00e676;
}

#game{
    width:350px;
    height:400px;
    max-width:95vw;
    background:#444;
    margin:20px auto;
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
    display:none;
}

#message{
    min-height:25px;
    font-size:17px;
    color:#ffd740;
}

#leaderboard{
    max-width:500px;
    margin:30px auto;
    background:#333;
    padding:20px;
    border-radius:15px;
    border:2px solid #666;
}

#leaderboard h2{
    color:#ffd700;
}

table{
    width:100%;
    border-collapse:collapse;
    margin-top:10px;
}

th,td{
    padding:10px 5px;
    border-bottom:1px solid #555;
}

th{
    color:#ffd700;
}

#noScores{
    color:#aaa;
}

#resetScores{
    background:#555;
    color:white;
    font-size:13px;
}

footer{
    margin-top:30px;
    color:#aaa;
    font-size:14px;
}

</style>

</head>


<body>


<!-- NAME SCREEN -->

<div id="nameScreen">

    <h1>🎮 Tap The Box</h1>

    <p>Enter your name to play</p>

    <input
        type="text"
        id="playerName"
        placeholder="Enter your name"
        maxlength="20"
        autocomplete="off"
    >

    <br>

    <button id="startButton" onclick="enterGame()">
        ▶️ Continue
    </button>

</div>



<!-- GAME SCREEN -->

<div id="gameScreen">

    <h1>🎮 Tap The Box</h1>

    <h3>
        👤 Player:
        <span id="currentPlayer">Player</span>
    </h3>

    <h2>
        🏆 Score:
        <span id="score">0</span>
    </h2>

    <h2>
        🥇 Highest Score:
        <span id="highScore">0</span>
    </h2>

    <h2>
        ✅ Right Clicks:
        <span id="right">0</span>
    </h2>

    <h2>
        ❌ Wrong Clicks:
        <span id="wrong">0</span>
    </h2>

    <h2>
        🖱 Total Clicks:
        <span id="total">0</span>
    </h2>

    <h2>
        ⏱ Time:
        <span id="time">0</span> sec
    </h2>


    <div id="game">

        <div id="box"></div>

    </div>


    <div id="message">
        Select a level to start
    </div>


    <div class="levelButtons">

        <button onclick="startGame('easy')">
            🟢 Easy
        </button>

        <button onclick="startGame('medium')">
            🟡 Medium
        </button>

        <button onclick="startGame('hard')">
            🔴 Hard
        </button>

    </div>


    <button onclick="changePlayer()">
        👤 Change Player
    </button>

</div>



<!-- LEADERBOARD -->

<div id="leaderboard">

    <h2>🏆 Top Scores</h2>

    <table>

        <thead>

            <tr>
                <th>Rank</th>
                <th>Player</th>
                <th>Score</th>
                <th>Level</th>
            </tr>

        </thead>

        <tbody id="scoreList">

        </tbody>

    </table>

    <p id="noScores">
        No scores yet. Be the first!
    </p>

    <button id="resetScores" onclick="resetScores()">
        🗑 Clear Scores
    </button>

</div>



<footer>

    <p>Tap The Box Game</p>

    <p>Developed by Rahul</p>

</footer>



<script>


/* =========================
   GAME VARIABLES
========================= */

let score = 0;
let right = 0;
let wrong = 0;
let total = 0;

let gameTimer;
let stopwatch;

let seconds = 0;

let currentLevel = "";

let player = "";

let box = document.getElementById("box");

let game = document.getElementById("game");



/* =========================
   AUDIO
========================= */

let audio;

function createAudio(){

    if(!audio){

        audio = new AudioContext();

    }

}


function beep(freq){

    createAudio();

    if(audio.state === "suspended"){

        audio.resume();

    }

    let oscillator = audio.createOscillator();

    let gain = audio.createGain();

    oscillator.type = "square";

    oscillator.frequency.value = freq;

    oscillator.connect(gain);

    gain.connect(audio.destination);

    gain.gain.value = 0.08;

    oscillator.start();

    setTimeout(function(){

        oscillator.stop();

    },120);

}



/* =========================
   ENTER PLAYER
========================= */

function enterGame(){

    let nameInput =
        document.getElementById("playerName").value.trim();


    if(nameInput === ""){

        alert("Please enter your name!");

        return;

    }


    player = nameInput;


    document.getElementById("currentPlayer").innerHTML =
        player;


    document.getElementById("nameScreen").style.display =
        "none";


    document.getElementById("gameScreen").style.display =
        "block";


    updatePlayerHighScore();

    displayLeaderboard();

}



/* =========================
   CHANGE PLAYER
========================= */

function changePlayer(){

    clearInterval(gameTimer);

    clearInterval(stopwatch);

    box.style.display = "none";

    document.getElementById("gameScreen").style.display =
        "none";

    document.getElementById("nameScreen").style.display =
        "block";

    document.getElementById("playerName").value = "";

}



/* =========================
   MOVE BOX
========================= */

function moveBox(){

    let maxX =
        game.clientWidth - box.offsetWidth;

    let maxY =
        game.clientHeight - box.offsetHeight;


    let x = Math.random() * maxX;

    let y = Math.random() * maxY;


    box.style.left = x + "px";

    box.style.top = y + "px";

}



/* =========================
   RESET GAME
========================= */

function resetGameStats(){

    score = 0;

    right = 0;

    wrong = 0;

    total = 0;

    seconds = 0;


    document.getElementById("score").innerHTML = "0";

    document.getElementById("right").innerHTML = "0";

    document.getElementById("wrong").innerHTML = "0";

    document.getElementById("total").innerHTML = "0";

    document.getElementById("time").innerHTML = "0";

}



/* =========================
   START GAME
========================= */

function startGame(level){

    if(player === ""){

        alert("Please enter your name first!");

        return;

    }


    createAudio();

    audio.resume();


    clearInterval(gameTimer);

    clearInterval(stopwatch);


    resetGameStats();


    currentLevel = level;


    let speed;


    if(level === "easy"){

        speed = 2000;

    }

    else if(level === "medium"){

        speed = 1000;

    }

    else{

        speed = 500;

    }


    box.style.display = "block";


    document.getElementById("message").innerHTML =
        "🎯 Tap the red box!";


    beep(500);


    moveBox();


    gameTimer = setInterval(function(){

        moveBox();

    }, speed);


    stopwatch = setInterval(function(){

        seconds++;

        document.getElementById("time").innerHTML =
            seconds;

    },1000);

}



/* =========================
   BOX CLICK
========================= */

box.onclick = function(e){

    e.stopPropagation();


    score++;

    right++;

    total++;


    document.getElementById("score").innerHTML =
        score;

    document.getElementById("right").innerHTML =
        right;

    document.getElementById("total").innerHTML =
        total;


    beep(900);


    moveBox();


    savePlayerHighScore();

};



/* =========================
   WRONG CLICK
========================= */

game.onclick = function(){

    wrong++;

    total++;


    document.getElementById("wrong").innerHTML =
        wrong;

    document.getElementById("total").innerHTML =
        total;


    beep(200);

};



/* =========================
   PLAYER HIGH SCORE
========================= */

function getScores(){

    let scores =
        localStorage.getItem("tapTheBoxScores");


    if(scores){

        return JSON.parse(scores);

    }


    return [];

}



/* =========================
   SAVE HIGH SCORE
========================= */

function savePlayerHighScore(){

    let scores = getScores();


    let existing =
        scores.find(function(item){

            return item.name.toLowerCase() ===
                   player.toLowerCase();

        });


    if(!existing){

        existing = {

            name: player,

            score: score,

            level: currentLevel

        };


        scores.push(existing);

    }

    else if(score > existing.score){

        existing.score = score;

        existing.level = currentLevel;

    }


    localStorage.setItem(
        "tapTheBoxScores",
        JSON.stringify(scores)
    );


    updatePlayerHighScore();

    displayLeaderboard();

}



/* =========================
   SHOW PLAYER HIGH SCORE
========================= */

function updatePlayerHighScore(){

    let scores = getScores();


    let existing =
        scores.find(function(item){

            return item.name.toLowerCase() ===
                   player.toLowerCase();

        });


    if(existing){

        document.getElementById("highScore").innerHTML =
            existing.score;

    }

    else{

        document.getElementById("highScore").innerHTML =
            "0";

    }

}



/* =========================
   LEADERBOARD
========================= */

function displayLeaderboard(){

    let scores = getScores();


    let scoreList =
        document.getElementById("scoreList");

    let noScores =
        document.getElementById("noScores");


    scoreList.innerHTML = "";


    if(scores.length === 0){

        noScores.style.display = "block";

        return;

    }


    noScores.style.display = "none";


    scores.sort(function(a,b){

        return b.score - a.score;

    });


    /*
       Show top 10
    */

    let topScores = scores.slice(0,10);


    topScores.forEach(function(item,index){

        let row =
            document.createElement("tr");


        let rank =
            document.createElement("td");

        let name =
            document.createElement("td");

        let scoreCell =
            document.createElement("td");

        let level =
            document.createElement("td");


        rank.innerHTML =
            index + 1;

        name.innerHTML =
            escapeHTML(item.name);

        scoreCell.innerHTML =
            item.score;

        level.innerHTML =
            item.level.toUpperCase();


        row.appendChild(rank);

        row.appendChild(name);

        row.appendChild(scoreCell);

        row.appendChild(level);


        scoreList.appendChild(row);

    });


}



/* =========================
   SECURITY
========================= */

function escapeHTML(text){

    let div =
        document.createElement("div");

    div.textContent = text;

    return div.innerHTML;

}



/* =========================
   CLEAR SCORES
========================= */

function resetScores(){

    let confirmDelete =
        confirm(
            "Are you sure you want to clear all scores?"
        );


    if(confirmDelete){

        localStorage.removeItem(
            "tapTheBoxScores"
        );


        displayLeaderboard();


        updatePlayerHighScore();

    }

}



/* =========================
   LOAD LEADERBOARD
========================= */

displayLeaderboard();


</script>


</body>

</html>
