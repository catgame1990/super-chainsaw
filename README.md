<!DOCTYPE html>
<html>
<head>
    <title>Cool Cat Coin Chase</title>
    <style>
        body { margin: 0; overflow: hidden; }
        #game { display: block; }
    </style>
</head>
<body>
    <canvas id="game" width="800" height="600"></canvas>
    <script>
        const canvas = document.getElementById("game");
        const ctx = canvas.getContext("2d");

        // Load images
        const catImg = new Image();
        catImg.src = "https://i.imgur.com/JvLmYQD.png";
        const coinImg = new Image();
        coinImg.src = "https://i.imgur.com/j4Rqho7.png";
        const bgImg = new Image();
        bgImg.src = "https://i.imgur.com/JJkzQbW.jpg";

        // Game objects
        let catX = 400, catY = 300;
        let coins = [];
        for (let i = 0; i < 10; i++) {
            coins.push({
                x: Math.random() * 750,
                y: Math.random() * 550
            });
        }
        let score = 0;

        // Touch controls (mobile-friendly!)
        canvas.addEventListener("touchmove", (e) => {
            e.preventDefault();
            const rect = canvas.getBoundingClientRect();
            catX = e.touches[0].clientX - rect.left - 25;
            catY = e.touches[0].clientY - rect.top - 25;
        });

        // Game loop
        function gameLoop() {
            // Draw background
            ctx.drawImage(bgImg, 0, 0, 800, 600);

            // Draw cat
            ctx.drawImage(catImg, catX, catY, 50, 50);

            // Draw coins
            coins.forEach((coin, i) => {
                ctx.drawImage(coinImg, coin.x, coin.y, 30, 30);
                // Collision
                if (Math.abs(catX - coin.x) < 30 && Math.abs(catY - coin.y) < 30) {
                    coins.splice(i, 1);
                    coins.push({ x: Math.random() * 750, y: Math.random() * 550 });
                    score++;
                }
            });

            // Draw score
            ctx.fillStyle = "white";
            ctx.font = "30px Arial";
            ctx.fillText("💰: " + score, 20, 40);

            requestAnimationFrame(gameLoop);
        }

        // Start game when images load
        let imagesLoaded = 0;
        [catImg, coinImg, bgImg].forEach(img => {
            img.onload = () => {
                imagesLoaded++;
                if (imagesLoaded === 3) gameLoop();
            };
        });
    </script>
</body>
</html>
