


<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>2063</title>
    <style>
    #myCanvas {
        background: #000000;
        border-style: solid;
    }

    #score {
        font-size: 150%;
    }
    </style>
</head>

<body>
    <div>Avery's Bot Game ~ Score: <span id="score">0</span> ~ Radius: <span id="radius">...</span></div>

     <div id="timerdiv">
  Timer: <span id="theTimer">0</span>
</div> 
    <canvas id="myCanvas" width="600" height="400">
        Your browser does not support canvas
    </canvas>
    <script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js'></script>
    <script>
    console.clear();

    const canvas = document.getElementById("myCanvas");
    const ctx = canvas.getContext("2d");

    const bots = [];

    function makeBot(x, y, r, dx, dy, color) {
        const bot = {};
        bot.r = r || Math.floor(Math.random() * 30 + 10);
        bot.x = x || Math.floor(Math.random() * (canvas.width - 2 * bot.r) + bot.r);
        bot.y = y || Math.floor(Math.random() * (canvas.height - 2 * bot.r) + bot.r);
        bot.dx = dx || Math.floor(Math.random() * 20 - 10);
        bot.dy = dy || Math.floor(Math.random() * 20 - 10);
        //https://css-tricks.com/snippets/javascript/random-hex-color/
        const randomColor = Math.floor(Math.random()*16777215).toString(16);
        bot.color = color || '#' + randomColor;
        return bot;
    }
   
    for (i = 0; i < 5; i++) {
        bots.push(makeBot());
    }

    bots[0].color = "lightpink";

    function drawBot(bot) {
        ctx.beginPath();
        ctx.arc(bot.x, bot.y, bot.r, 0 * Math.PI, 2 * Math.PI);
        ctx.stroke();
        ctx.fillStyle = bot.color;
        ctx.fill();
        ctx.moveTo(bot.x, bot.y);
        const theta = Math.atan2(bot.dy, bot.dx);
        ctx.lineTo(bot.x + bot.r * Math.cos(theta), bot.y + bot.r * Math.sin(theta));
        ctx.stroke();
    }

    function drawBots() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        for (i = 0; i < bots.length; i++) {
            moveBot(bots[i]);
        }
    }


    function moveBot(bot) {
        bot.x = bot.x + bot.dx;
        bot.y = bot.y + bot.dy;
        if (bot.y - bot.r < 0) {
            bot.y = bot.r;
            bot.dy = -bot.dy;
            addOne()
        }

        if (bot.x - bot.r < 0) {
            bot.x = bot.r
            bot.dx = -bot.dx;
            addOne()
        }

        if (bot.x + bot.r >= canvas.width) {
            bot.x = canvas.width-bot.r;
            bot.dx = -bot.dx;
            addOne()
        }

        if (bot.y + bot.r >= canvas.height) {
            bot.y = canvas.height-bot.r;
            bot.dy = -bot.dy;
            addOne()
        }
        drawBot(bot);
        postRadius();
    }

    function addOne() {
        var score = $("#score").text();
        $("#score").text(Number(score) + 1)
    }

    function postRadius()
    {
       $("#radius").text(bots[0].r);
    }
    
    setInterval(drawBots, 250, bots[0]);

    $("body").keydown(onKeyDown);

    function onKeyDown(e) {
        e.preventDefault();

        if (e.key == " ") {
            bots[0].dx = 0;
            bots[0].dy = 0;
        }

        if (e.key == "ArrowUp") {
            bots[0].dy = bots[0].dy - 1;
        }

        if (e.key == "ArrowDown") {
            bots[0].dy = bots[0].dy + 1;
        }

        if (e.key == "ArrowLeft") {
            bots[0].dx = bots[0].dx - 1;
        }

        if (e.key == "ArrowRight") {
            bots[0].dy = bots[0].dy - 1;
        }

        if (e.key == "w") {
            bots[0].y = bots[0].y - 1;
        }


        if (e.key == "]") {
            bots[0].r = bots[0].r + 2;
        }

    }
    </script>
</body>

</html>
