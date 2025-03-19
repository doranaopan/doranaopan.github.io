---
layout: page
title: ゲーム
permalink: /game/
---

まぁテトリスでもやっていきなよ。

上下左右キーでプレイできるよ。

<div id="game-container" style="width: 300px; height: 600px; border: 1px solid black;"></div>
<script>
    const canvas = document.createElement('canvas');
    const context = canvas.getContext('2d');
    canvas.width = 300;
    canvas.height = 600;
    document.getElementById('game-container').appendChild(canvas);

    const colors = [
        '#000000',
        '#FF0000',
        '#00FF00',
        '#0000FF',
        '#FFFF00',
        '#FFA500',
        '#800080',
        '#00FFFF'
    ];

    const shapes = [
        [[1, 1, 1, 1]],
        [[1, 1], [1, 1]],
        [[0, 1, 0], [1, 1, 1]],
        [[1, 0, 0], [1, 1, 1]],
        [[0, 0, 1], [1, 1, 1]],
        [[1, 1, 0], [0, 1, 1]],
        [[0, 1, 1], [1, 1, 0]]
    ];

    const grid = Array.from({ length: 20 }, () => Array(10).fill(0));
    let currentShape = shapes[Math.floor(Math.random() * shapes.length)];
    let currentColor = colors[Math.floor(Math.random() * colors.length)];
    let currentX = 3;
    let currentY = 0;

    function drawShape(shape, x, y, color) {
        context.fillStyle = color;
        shape.forEach((row, rowIndex) => {
            row.forEach((cell, cellIndex) => {
                if (cell) {
                    context.fillRect((x + cellIndex) * 30, (y + rowIndex) * 30, 30, 30);
                }
            });
        });
    }

    function drawGrid() {
        for (let row = 0; row < grid.length; row++) {
            for (let col = 0; col < grid[row].length; col++) {
                if (grid[row][col] !== 0) {
                    context.fillStyle = grid[row][col];
                    context.fillRect(col * 30, row * 30, 30, 30);
                }
            }
        }
    }

    function draw() {
        context.fillStyle = '#FFFFFF'; // Set background to white
        context.fillRect(0, 0, canvas.width, canvas.height);
        drawGrid();
        drawShape(currentShape, currentX, currentY, currentColor);
    }

    function moveDown() {
        currentY++;
        if (collision()) {
            currentY--;
            merge();
            clearLines();
            reset();
        }
    }

    function collision() {
        for (let row = 0; row < currentShape.length; row++) {
            for (let col = 0; col < currentShape[row].length; col++) {
                if (currentShape[row][col] && 
                    (grid[currentY + row] && grid[currentY + row][currentX + col]) !== 0) {
                    return true;
                }
            }
        }
        return false;
    }

    function merge() {
        currentShape.forEach((row, rowIndex) => {
            row.forEach((cell, cellIndex) => {
                if (cell) {
                    grid[currentY + rowIndex][currentX + cellIndex] = currentColor;
                }
            });
        });
    }

    function clearLines() {
        for (let row = grid.length - 1; row >= 0; row--) {
            if (grid[row].every(cell => cell !== 0)) {
                grid.splice(row, 1);
                grid.unshift(Array(10).fill(0));
            }
        }
    }

    function rotateShape() {
        const newShape = currentShape[0].map((_, index) => currentShape.map(row => row[index]).reverse());
        const originalX = currentX;
        const originalY = currentY;
        currentX = Math.max(0, Math.min(currentX, 10 - newShape[0].length));
        currentY = Math.max(0, Math.min(currentY, 20 - newShape.length));
        if (!collision()) {
            currentShape = newShape;
        } else {
            currentX = originalX;
            currentY = originalY;
        }
    }

    function reset() {
        currentShape = shapes[Math.floor(Math.random() * shapes.length)];
        currentColor = colors[Math.floor(Math.random() * colors.length)];
        currentX = 3;
        currentY = 0;
        if (collision()) {
            alert('Game Over');
            grid.forEach(row => row.fill(0));
        }
    }

    function main() {
        draw();
        moveDown();
        setTimeout(main, 1000);
    }

    document.addEventListener('keydown', event => {
        event.preventDefault();
        if (event.key === 'ArrowLeft') {
            currentX--;
            if (collision()) {
                currentX++;
            }
        } else if (event.key === 'ArrowRight') {
            currentX++;
            if (collision()) {
                currentX--;
            }
        } else if (event.key === 'ArrowDown') {
            moveDown();
        } else if (event.key === 'ArrowUp') {
            rotateShape();
        }
    });

    main();
</script>
