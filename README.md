<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neural Network Animation</title>
    <style>
        body {
            margin: 0;
            padding: 20px;
            background: #000;
            color: #0f0;
            font-family: 'Courier New', monospace;
            overflow-x: hidden;
        }
        
        .container {
            max-width: 900px;
            margin: 0 auto;
        }
        
        .header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        h1 {
            font-size: 2.5em;
            margin: 10px 0;
            text-shadow: 0 0 10px #0f0;
        }
        
        h2 {
            font-size: 1.5em;
            color: #0f0;
            margin: 20px 0 10px 0;
            text-shadow: 0 0 5px #0f0;
        }
        
        .subtitle {
            font-size: 1.2em;
            color: #0a0;
        }
        
        .intro {
            text-align: center;
            line-height: 1.8;
            margin: 20px 0;
            padding: 20px;
            border: 2px solid #0f0;
            border-radius: 10px;
            background: rgba(0, 255, 0, 0.05);
        }
        
        canvas {
            display: block;
            margin: 30px auto;
            border: 2px solid #0f0;
            border-radius: 10px;
            background: #000;
            box-shadow: 0 0 20px #0f0;
        }
        
        .stack {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            margin: 30px 0;
        }
        
        .tech {
            text-align: center;
            padding: 15px;
            border: 2px solid #0f0;
            border-radius: 8px;
            background: rgba(0, 255, 0, 0.1);
            transition: all 0.3s;
        }
        
        .tech:hover {
            background: rgba(0, 255, 0, 0.2);
            box-shadow: 0 0 15px #0f0;
            transform: translateY(-5px);
        }
        
        .tech-name {
            font-size: 1.1em;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>MD MAHBUB ISLAM</h1>
            <div class="subtitle">Aspirant AI Engineer | GPU Programming Enthusiast</div>
        </div>
        
        <div class="intro">
            <p>Passionate about building intelligent systems through deep learning and GPU acceleration.</p>
            <p>Focused on optimizing neural networks and exploring high-performance computing for AI applications.</p>
        </div>
        
        <h2>⚡ Neural Network Visualization</h2>
        <canvas id="neuralCanvas" width="800" height="400"></canvas>
        
        <h2>🔢 Binary Matrix Stream</h2>
        <canvas id="binaryCanvas" width="800" height="400"></canvas>
        
        <h2>🛠️ Tech Stack</h2>
        <div class="stack">
            <div class="tech">
                <div class="tech-name">Python</div>
            </div>
            <div class="tech">
                <div class="tech-name">C++</div>
            </div>
            <div class="tech">
                <div class="tech-name">PyTorch</div>
            </div>
            <div class="tech">
                <div class="tech-name">Scikit-learn</div>
            </div>
            <div class="tech">
                <div class="tech-name">MySQL</div>
            </div>
            <div class="tech">
                <div class="tech-name">PostgreSQL</div>
            </div>
        </div>
    </div>

    <script>
        // Neural Network Animation
        const neuralCanvas = document.getElementById('neuralCanvas');
        const neuralCtx = neuralCanvas.getContext('2d');
        
        const layers = [5, 8, 8, 6, 3];
        const nodes = [];
        const connections = [];
        
        function initNeuralNetwork() {
            const spacing = neuralCanvas.width / (layers.length + 1);
            
            layers.forEach((count, layerIndex) => {
                const layerNodes = [];
                const verticalSpacing = neuralCanvas.height / (count + 1);
                
                for (let i = 0; i < count; i++) {
                    layerNodes.push({
                        x: spacing * (layerIndex + 1),
                        y: verticalSpacing * (i + 1),
                        activation: Math.random(),
                        pulseSpeed: 0.02 + Math.random() * 0.03
                    });
                }
                nodes.push(layerNodes);
            });
            
            for (let i = 0; i < layers.length - 1; i++) {
                for (let j = 0; j < nodes[i].length; j++) {
                    for (let k = 0; k < nodes[i + 1].length; k++) {
                        connections.push({
                            from: nodes[i][j],
                            to: nodes[i + 1][k],
                            weight: Math.random(),
                            pulse: Math.random()
                        });
                    }
                }
            }
        }
        
        function drawNeuralNetwork() {
            neuralCtx.fillStyle = '#000';
            neuralCtx.fillRect(0, 0, neuralCanvas.width, neuralCanvas.height);
            
            connections.forEach(conn => {
                conn.pulse += 0.02;
                if (conn.pulse > 1) conn.pulse = 0;
                
                const gradient = neuralCtx.createLinearGradient(
                    conn.from.x, conn.from.y, conn.to.x, conn.to.y
                );
                
                const alpha = 0.1 + conn.weight * 0.3;
                const pulseAlpha = Math.sin(conn.pulse * Math.PI) * 0.5;
                
                gradient.addColorStop(0, `rgba(0, 255, 0, ${alpha})`);
                gradient.addColorStop(conn.pulse, `rgba(0, 255, 0, ${alpha + pulseAlpha})`);
                gradient.addColorStop(1, `rgba(0, 255, 0, ${alpha})`);
                
                neuralCtx.strokeStyle = gradient;
                neuralCtx.lineWidth = 1 + conn.weight;
                neuralCtx.beginPath();
                neuralCtx.moveTo(conn.from.x, conn.from.y);
                neuralCtx.lineTo(conn.to.x, conn.to.y);
                neuralCtx.stroke();
            });
            
            nodes.forEach(layer => {
                layer.forEach(node => {
                    node.activation += node.pulseSpeed;
                    if (node.activation > 1) node.activation = 0;
                    
                    const size = 8 + Math.sin(node.activation * Math.PI * 2) * 3;
                    const brightness = 100 + Math.sin(node.activation * Math.PI * 2) * 155;
                    
                    neuralCtx.fillStyle = `rgb(0, ${brightness}, 0)`;
                    neuralCtx.beginPath();
                    neuralCtx.arc(node.x, node.y, size, 0, Math.PI * 2);
                    neuralCtx.fill();
                    
                    neuralCtx.strokeStyle = `rgba(0, 255, 0, 0.5)`;
                    neuralCtx.lineWidth = 2;
                    neuralCtx.beginPath();
                    neuralCtx.arc(node.x, node.y, size + 3, 0, Math.PI * 2);
                    neuralCtx.stroke();
                });
            });
            
            requestAnimationFrame(drawNeuralNetwork);
        }
        
        // Binary Matrix Animation
        const binaryCanvas = document.getElementById('binaryCanvas');
        const binaryCtx = binaryCanvas.getContext('2d');
        
        const fontSize = 14;
        const columns = binaryCanvas.width / fontSize;
        const drops = [];
        
        for (let i = 0; i < columns; i++) {
            drops[i] = Math.random() * binaryCanvas.height / fontSize;
        }
        
        function drawBinaryMatrix() {
            binaryCtx.fillStyle = 'rgba(0, 0, 0, 0.05)';
            binaryCtx.fillRect(0, 0, binaryCanvas.width, binaryCanvas.height);
            
            binaryCtx.font = fontSize + 'px monospace';
            
            for (let i = 0; i < drops.length; i++) {
                const text = Math.random() > 0.5 ? '1' : '0';
                const brightness = 150 + Math.random() * 105;
                binaryCtx.fillStyle = `rgb(0, ${brightness}, 0)`;
                
                binaryCtx.fillText(text, i * fontSize, drops[i] * fontSize);
                
                if (drops[i] * fontSize > binaryCanvas.height && Math.random() > 0.975) {
                    drops[i] = 0;
                }
                drops[i]++;
            }
        }
        
        initNeuralNetwork();
        drawNeuralNetwork();
        setInterval(drawBinaryMatrix, 50);
    </script>
</body>
</html>
