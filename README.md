# sriyaa1234.github.io
<!DOCTYPE html> 
<html>
<head>
<title> Donate to help poverty! </title>  
</head>
 
<body background="https://upload.wikimedia.org/wikipedia/commons/f/f1/Field_and_Sky.jpg">
</body>
 
<body onload="startGame()">
<script>
 
var Character
var Poles = [];
var Score;
 
function startGame() {
    Character = new component(75,75, "https://cdnb.artstation.com/p/assets/images/images/002/345/245/original/sylvia-wilson-final-character-1-running.gif?1460555520.gif", 10, 120, "image");
    Character.gravity = 0.2;
    Score = new component("30px", "Times New Roman", "white", 1000, 50, "text");
 
    GameArea.start();
}
 
var GameArea = {
    canvas : document.createElement("canvas"),
    start : function() {
        this.canvas.width = 1300;
        this.canvas.height = 550;
        this.context = this.canvas.getContext("2d");
        document.body.insertBefore(this.canvas, document.body.childNodes[0]);
        this.frameNo = 0;
        this.interval = setInterval(updateGameArea, 20);
        },
    clear : function() {
        this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
    }
}
 
function component(width, height, color, x, y, type) {
 
    this.type = type;
    if (type == "image") {
    this.image = new Image();
    this.image.src = color;
    }
   
    this.score = 0;
    this.width = width;
    this.height = height;
    this.speedX = 0;
    this.speedY = 0;    
    this.x = x;
    this.y = y;
    this.gravity = 0;
    this.gravitySpeed = 0;
    this.update = function() {
        ctx = GameArea.context;
 
        if (this.type == "text") {
            ctx.font = this.width + " " + this.height;
            ctx.fillStyle = color;
            ctx.fillText(this.text, this.x, this.y);
        }
        if (type == "image") {
        ctx.drawImage(this.image, 
        this.x, 
        this.y,
        this.width, this.height);
        } 
        else {
            ctx.fillStyle = color;
            ctx.fillRect(this.x, this.y, this.width, this.height);
        }
    }
    this.newPos = function() {
        this.gravitySpeed += this.gravity;
        this.x += this.speedX;
        this.y += this.speedY + this.gravitySpeed;
        this.hitBottom();
    }
    this.hitBottom = function() {
        var rockbottom = GameArea.canvas.height - this.height;
        if (this.y > rockbottom) {
            this.y = rockbottom;
            this.gravitySpeed = 0;
        }
    }
    this.crashWith = function(otherobj) {
        var left = this.x;
        var right = this.x + (this.width);
        var top = this.y;
        var bottom = this.y + (this.height);
        var otherleft = otherobj.x;
        var otherright = otherobj.x + (otherobj.width);
        var othertop = otherobj.y;
        var otherbottom = otherobj.y + (otherobj.height);
        var crash = true;
        if ((bottom < othertop) || (top > otherbottom) || (right < otherleft) || (left > otherright)) {
            crash = false;
        }
        return crash;
    }
}
 
function updateGameArea() {
    var x, height, gap, minHeight, maxHeight, minGap, maxGap;
    for (i = 0; i < Poles.length; i += 1) {
        if (Character.crashWith(Poles[i])) {
            return;
        } 
    }
    GameArea.clear();
    GameArea.frameNo += 1;
    if (GameArea.frameNo == 1 || everyinterval(150)) {
        x = GameArea.canvas.width;
        minHeight = 20;
        maxHeight = 200;
        height = Math.floor(Math.random()*(maxHeight-minHeight+1)+minHeight);
        minGap = 160;
        maxGap = 300;
        gap = Math.floor(Math.random()*(maxGap-minGap+1)+minGap);
        Poles.push(new component(15, height, "black", x, 0));
        Poles.push(new component(15, x - height - gap, "black", x, height + gap));
    }
    for (i = 0; i < Poles.length; i += 1) {
        Poles[i].x += -1;
        Poles[i].update();
    }
    Score.text="SCORE: " + GameArea.frameNo;
    Score.update();
    Character.newPos();
    Character.update();
}
 
function everyinterval(n) {
    if ((GameArea.frameNo / n) % 1 == 0) {return true;}
    return false;
}
 
function accelerate(n) {
    Character.gravity = n;
}
</script>
<br>
<button onkeydown="accelerate(-0.2)" onkeyup="accelerate(0.05)">START GAME</button>
<p>Press the START GAME button and then any key to  to stay in the air.</p>
<p>Stay alive for as long as possible to get a higher score!</p>
<p>Please donate to help poverty! </p>
</body>
</html>
