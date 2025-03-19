# Simple-game
Very simple game 
<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبتي البسيطة</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background-color: white;
        }
        canvas {
            display: block;
        }
    </style>
</head>
<body>

<canvas id="gameCanvas"></canvas>

<script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    canvas.width = 800;
    canvas.height = 400;

    const characterImg = new Image();
    characterImg.src = "character.png";

    const player = {
        x: 100,
        y: 300,
        width: 50,
        height: 50,
        speed: 5,
        dx: 0,
        dy: 0,
        gravity: 0.5,
        jumpPower: -10,
        onGround: false
    };

    const platforms = [
        { x: 50, y: 350, width: 200, height: 20 },
        { x: 300, y: 280, width: 150, height: 20 },
        { x: 500, y: 200, width: 180, height: 20 },
        { x: 700, y: 320, width: 120, height: 20 }
    ];

    let keys = {};
    window.addEventListener("keydown", (e) => { keys[e.key] = true; });
    window.addEventListener("keyup", (e) => { keys[e.key] = false; });

    function update() {
        if (keys["ArrowRight"]) {
            player.dx = player.speed;
        } else if (keys["ArrowLeft"]) {
            player.dx = -player.speed;
        } else {
            player.dx = 0;
        }

        if (keys["ArrowUp"] && player.onGround) {
            player.dy = player.jumpPower;
            player.onGround = false;
        }

        player.dy += player.gravity;
        player.y += player.dy;
        player.x += player.dx;

        if (player.x < 0) player.x = 0;
        if (player.x + player.width > canvas.width) player.x = canvas.width - player.width;

        player.onGround = false;
        for (let platform of platforms) {
            if (
                player.y + player.height > platform.y &&
                player.y + player.height - player.dy <= platform.y &&
                player.x + player.width > platform.x &&
                player.x < platform.x + platform.width
            ) {
                player.y = platform.y - player.height;
                player.dy = 0;
                player.onGround = true;
            }
        }
    }

    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.drawImage(characterImg, player.x, player.y, player.width, player.height);
        ctx.fillStyle = "black";
        for (let platform of platforms) {
            ctx.fillRect(platform.x, platform.y, platform.width, platform.height);
        }
    }

    function gameLoop() {
        update();
        draw();
        requestAnimationFrame(gameLoop);
    }

    gameLoop();
</script>

</body>
</html>
