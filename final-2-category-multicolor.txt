<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Categorical Data Analysis</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/jstat@latest/dist/jstat.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-light: #f8f8f8;
            --primary-medium: #cccccc;
            --primary-dark: #333333;
            --button-gradient: linear-gradient(135deg, #e0e0e0 0%, #b0b0b0 100%);
            --button-hover: linear-gradient(135deg, #d0d0d0 0%, #909090 100%);
        }
        
        body {
            font-family: 'Space Mono', monospace;
            background-color: var(--primary-light);
            color: #333;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            line-height: 1.6;
            position: relative;
            min-height: 100vh;
            padding-bottom: 40px;
        }
        
        header {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            margin-bottom: 30px;
            background: linear-gradient(135deg, #ffffff 0%, #f0f0f0 100%);
            border-left: 5px solid var(--primary-dark);
            text-align: center;
        }
        
        h1 {
            font-size: 2.5rem;
            color: var(--primary-dark);
            margin-bottom: 10px;
        }
        
        h2 {
            color: var(--primary-dark);
            border-bottom: 2px solid var(--primary-medium);
            padding-bottom: 8px;
            margin-top: 15px;
            text-align: center;
            font-size: 1.4rem;
        }
        
        .subtitle {
            font-size: 1.2rem;
            color: #666;
            margin-bottom: 20px;
        }
        
        .main-container {
            display: flex;
            gap: 30px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }
        
        .data-section {
            flex: 1;
            min-width: 300px;
            order: 1;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .visualization-section {
            flex: 2;
            min-width: 300px;
            order: 2;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .data-card {
            background: white;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            background: linear-gradient(135deg, #ffffff 0%, #f8f8f8 100%);
            flex: 1;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 10px;
            font-size: 0.9em;
        }
        
        th, td {
            padding: 8px 10px;
            border: 1px solid #ddd;
            text-align: left;
        }
        
        th {
            background-color: var(--primary-medium);
            color: #333;
            font-weight: 600;
        }
        
        input[type="number"] {
            width: 100%;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 0.9em;
            background-color: rgba(255,255,255,0.8);
            font-family: 'Space Mono', monospace;
        }
        
        .button-group {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 15px;
            justify-content: center;
        }
        
        button {
            padding: 8px 16px;
            border: none;
            border-radius: 20px;
            font-size: 0.9em;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            font-family: 'Space Mono', monospace;
        }
        
        .primary-btn {
            background: var(--button-gradient);
            color: #333;
        }
        
        .primary-btn:hover {
            background: var(--button-hover);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.15);
        }
        
        .secondary-btn {
            background: white;
            border: 1px solid var(--primary-medium);
            color: #333;
        }
        
        .secondary-btn:hover {
            background: #f0f0f0;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.15);
        }
        
        .chart-container {
            background: white;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            background: linear-gradient(135deg, #ffffff 0%, #f8f8f8 100%);
            height: 450px;
            display: flex;
            flex-direction: column;
        }
        
        .chart-wrapper {
            flex: 1;
            position: relative;
            margin-top: 10px;
        }
        
        #comparisonChart {
            width: 100% !important;
            height: 100% !important;
        }
        
        .results {
            background: white;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            background: linear-gradient(135deg, #ffffff 0%, #f8f8f8 100%);
            font-size: 0.9em;
        }
        
        .color-options {
            display: flex;
            gap: 8px;
            align-items: center;
            margin: 10px 0;
            justify-content: center;
        }
        
        .color-option {
            width: 25px;
            height: 25px;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid transparent;
            transition: all 0.3s ease;
            transform-origin: center;
        }
        
        .color-option:hover {
            transform: scale(1.2);
        }
        
        .color-option.selected {
            border: 2px solid #333;
            box-shadow: 0 0 0 2px white;
            transform: scale(1.2);
        }
        
        .confidence-level {
            display: flex;
            gap: 8px;
            align-items: center;
            margin: 10px 0 15px;
            justify-content: center;
            flex-wrap: wrap;
        }
        
        .confidence-btn {
            padding: 6px 12px;
            border-radius: 15px;
            background: #f0f0f0;
            border: 1px solid var(--primary-medium);
            cursor: pointer;
            font-size: 0.85em;
        }
        
        .confidence-btn.selected {
            background: var(--primary-medium);
            font-weight: bold;
        }
        
        .status-message {
            font-style: italic;
            color: #666;
            margin-left: 8px;
            font-size: 0.85em;
        }
        
        .output-cell {
            font-weight: bold;
            color: var(--primary-dark);
        }
        
        .developer-credit {
            position: absolute;
            bottom: 10px;
            right: 20px;
            font-size: 0.8em;
            color: #666;
        }

        /* New styles for color palettes */
        .palette-selector {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 15px 0;
        }

        .palette-options {
            display: flex;
            gap: 10px;
            margin-top: 5px;
        }

        .palette {
            display: flex;
            transition: all 0.3s ease;
            cursor: pointer;
            border-radius: 20px;
            overflow: hidden;
            height: 30px;
            position: relative;
        }

        .palette-main {
            display: flex;
            width: 50px;
            transition: width 0.5s cubic-bezier(0.25, 0.1, 0.25, 1);
        }

        .palette-small {
            display: flex;
            width: 50px;
        }

        .palette-color {
            height: 100%;
            flex: 1;
            transition: all 0.3s ease;
        }

        .palette:hover .palette-color {
            transform: scaleY(1.2);
        }

        .palette.selected {
            transform: scale(1.05);
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
        }

        .palette.selected .palette-main {
            width: 100px;
        }

        .palette::after {
            content: '';
            position: absolute;
            bottom: -2px;
            left: 0;
            right: 0;
            height: 1px;
            background-color: #ccc;
            transition: all 0.3s ease;
        }

        .palette:hover::after {
            background-color: #999;
        }

        @media (max-width: 768px) {
            .main-container {
                flex-direction: column;
            }
            .developer-credit {
                position: static;
                text-align: center;
                margin-top: 20px;
            }
            .palette-options {
                flex-direction: column;
                align-items: center;
            }
            .palette-main {
                width: 80px;
            }
            .palette-small {
                width: 40px;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>CATEGORICAL DATA ANALYSIS</h1>
        <div class="subtitle"> </div>
    </header>
    
    <div class="main-container">
        <div class="data-section">
            <div class="button-group">
                <button class="primary-btn" onclick="plotData()">Let's Plot</button>
                <button class="secondary-btn" onclick="clearData()">Clear Data</button>
                <button class="secondary-btn" onclick="clearGraph()">Clear Graph</button>
            </div>
            
            <div class="palette-selector">
                <span>Select color palette:</span>
                <div class="palette-options">
                    <div class="palette selected" data-palette="0">
                        <div class="palette-main">
                            <div class="palette-color" style="background-color: #3366ff;"></div>
                            <div class="palette-color" style="background-color: #ff5733;"></div>
                            <div class="palette-color" style="background-color: #33cc33;"></div>
                            <div class="palette-color" style="background-color: #cc33ff;"></div>
                        </div>
                    </div>
                    <div class="palette" data-palette="1">
                        <div class="palette-main">
                            <div class="palette-color" style="background-color: #332288;"></div>
                            <div class="palette-color" style="background-color: #117733;"></div>
                            <div class="palette-color" style="background-color: #44AA99;"></div>
                            <div class="palette-color" style="background-color: #cc33ff;"></div>
                        </div>
                    </div>
                    <div class="palette" data-palette="2">
                        <div class="palette-main">
                            <div class="palette-color" style="background-color: #332288;"></div>
                            <div class="palette-color" style="background-color: #88CCEE;"></div>
                            <div class="palette-color" style="background-color: #DDCC77;"></div>
                            <div class="palette-color" style="background-color: #cc33ff;"></div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="data-card">
                <h2>Group 1</h2>
                <table>
                    <tr>
                        <th>Total Number (n)</th>
                        <td><input type="number" id="group1-n" value="100" oninput="updateGroup1()"></td>
                    </tr>
                    <tr>
                        <th>Number of 1st category</th>
                        <td><input type="number" id="group1-success1" value="45" oninput="updateGroup1()"></td>
                    </tr>
                    <tr>
                        <th>Number of 2nd category</th>
                        <td id="group1-success2" class="output-cell">55</td>
                    </tr>
                    <tr>
                        <th>Confidence Interval</th>
                        <td id="group1-ci" class="output-cell">35.2% - 54.8%</td>
                    </tr>
                </table>
                
                <div class="color-options">
                    <span>Select color:</span>
                    <div class="color-option selected" style="background-color: #3366ff;" data-group="1" data-color="#3366ff"></div>
                    <div class="color-option" style="background-color: #ff5733;" data-group="1" data-color="#ff5733"></div>
                    <div class="color-option" style="background-color: #33cc33;" data-group="1" data-color="#33cc33"></div>
                    <div class="color-option" style="background-color: #cc33ff;" data-group="1" data-color="#cc33ff"></div>
                </div>
            </div>
            
            <div class="data-card">
                <h2>Group 2</h2>
                <table>
                    <tr>
                        <th>Total Number (n)</th>
                        <td><input type="number" id="group2-n" value="100" oninput="updateGroup2()"></td>
                    </tr>
                    <tr>
                        <th>Number of 1st category</th>
                        <td><input type="number" id="group2-success1" value="65" oninput="updateGroup2()"></td>
                    </tr>
                    <tr>
                        <th>Number of 2nd category</th>
                        <td id="group2-success2" class="output-cell">35</td>
                    </tr>
                    <tr>
                        <th>Confidence Interval</th>
                        <td id="group2-ci" class="output-cell">55.2% - 74.8%</td>
                    </tr>
                </table>
                
                <div class="color-options">
                    <span>Select color:</span>
                    <div class="color-option selected" style="background-color: #33cc33;" data-group="2" data-color="#33cc33"></div>
                    <div class="color-option" style="background-color: #ff5733;" data-group="2" data-color="#ff5733"></div>
                    <div class="color-option" style="background-color: #3366ff;" data-group="2" data-color="#3366ff"></div>
                    <div class="color-option" style="background-color: #cc33ff;" data-group="2" data-color="#cc33ff"></div>
                </div>
            </div>
        </div>
        
        <div class="visualization-section">
            <div class="confidence-level">
                <span>Select confidence interval:</span>
                <div class="confidence-btn" data-level="0.90">90%</div>
                <div class="confidence-btn selected" data-level="0.95">95%</div>
                <div class="confidence-btn" data-level="0.99">99%</div>
                <span class="status-message">Click to update confidence intervals</span>
            </div>
            
            <div class="chart-container">
                <h2>Graphed Data</h2>
                <div class="chart-wrapper">
                    <canvas id="comparisonChart"></canvas>
                </div>
            </div>
            
            <div class="results">
                <h2>Statistical Results</h2>
                <table>
                    <tr>
                        <th>Metric</th>
                        <th>Group 1</th>
                        <th>Group 2</th>
                    </tr>
                    <tr>
                        <td>Success Proportion (1st category)</td>
                        <td id="result-g1-prop1" class="output-cell">45.00%</td>
                        <td id="result-g2-prop1" class="output-cell">65.00%</td>
                    </tr>
                    <tr>
                        <td>Success Proportion (2nd category)</td>
                        <td id="result-g1-prop2" class="output-cell">55.00%</td>
                        <td id="result-g2-prop2" class="output-cell">35.00%</td>
                    </tr>
                    <tr>
                        <td id="result-ci-label">95% CI Lower (1st category)</td>
                        <td id="result-g1-ci-lower1" class="output-cell">35.2%</td>
                        <td id="result-g2-ci-lower1" class="output-cell">55.2%</td>
                    </tr>
                    <tr>
                        <td id="result-ci-upper-label">95% CI Upper (1st category)</td>
                        <td id="result-g1-ci-upper1" class="output-cell">54.8%</td>
                        <td id="result-g2-ci-upper1" class="output-cell">74.8%</td>
                    </tr>
                </table>
            </div>
        </div>
    </div>

    <div class="developer-credit">Developed by Angela Xu</div>

    <script>
        let myChart = null;
        let groupColors = {
            1: '#3366ff', // Default blue for group 1
            2: '#33cc33'  // Default green for group 2
        };
        let confidenceLevel = 0.95;
        
        // Define color palettes
        const colorPalettes = [
            ['#3366ff', '#ff5733', '#33cc33', '#cc33ff'],
            ['#332288', '#117733', '#44AA99', '#cc33ff'],
            ['#332288', '#88CCEE', '#DDCC77', '#cc33ff']
        ];
        
        // Initialize palette selection
        document.querySelectorAll('.palette').forEach(palette => {
            palette.addEventListener('click', function() {
                const paletteIndex = parseInt(this.getAttribute('data-palette'));
                const selectedPalette = colorPalettes[paletteIndex];
                
                // Update selected state
                document.querySelectorAll('.palette').forEach(p => {
                    p.classList.remove('selected');
                    p.querySelector('.palette-main').style.width = '50px';
                });
                this.classList.add('selected');
                this.querySelector('.palette-main').style.width = '100px';
                
                // Update color options for both groups
                updateColorOptions(1, selectedPalette);
                updateColorOptions(2, selectedPalette);
                
                // Update the chart if it exists
                if (myChart) {
                    myChart.data.datasets[0].backgroundColor = groupColors[1];
                    myChart.data.datasets[0].borderColor = groupColors[1];
                    myChart.data.datasets[1].backgroundColor = groupColors[2];
                    myChart.data.datasets[1].borderColor = groupColors[2];
                    myChart.update();
                }
            });
            
            // Add smooth hover effect with staged expansion
            palette.addEventListener('mouseenter', function() {
                if (!this.classList.contains('selected')) {
                    const paletteMain = this.querySelector('.palette-main');
                    paletteMain.style.transition = 'none';
                    paletteMain.style.width = '50px';
                    
                    // Animate in 20 stages
                    for (let i = 1; i <= 20; i++) {
                        setTimeout(() => {
                            paletteMain.style.width = `${50 + (i * 2.5)}px`;
                        }, 100 + (i * 20));
                    }
                    
                    // Set final transition for smoothness
                    setTimeout(() => {
                        paletteMain.style.transition = 'width 0.3s ease';
                    }, 100 + (20 * 20));
                }
            });
            
            palette.addEventListener('mouseleave', function() {
                if (!this.classList.contains('selected')) {
                    const paletteMain = this.querySelector('.palette-main');
                    paletteMain.style.width = '50px';
                }
            });
        });
        
        // Function to update color options when palette changes
        function updateColorOptions(group, palette) {
            const colorOptions = document.querySelectorAll(`.color-option[data-group="${group}"]`);
            
            colorOptions.forEach((option, index) => {
                const newColor = palette[index];
                option.style.backgroundColor = newColor;
                option.setAttribute('data-color', newColor);
                
                // If this was the selected color, update the group color
                if (option.classList.contains('selected')) {
                    groupColors[group] = newColor;
                }
            });
        }
        
        // Initialize color selection
        document.querySelectorAll('.color-option').forEach(option => {
            option.addEventListener('click', function() {
                const group = this.getAttribute('data-group');
                const color = this.getAttribute('data-color');
                groupColors[group] = color;
                
                // Update selected state
                document.querySelectorAll(`.color-option[data-group="${group}"]`).forEach(opt => {
                    opt.classList.remove('selected');
                });
                this.classList.add('selected');
                
                if (myChart) {
                    // Update the chart colors immediately
                    if (group == 1) {
                        myChart.data.datasets[0].backgroundColor = color;
                        myChart.data.datasets[0].borderColor = color;
                    } else {
                        myChart.data.datasets[1].backgroundColor = color;
                        myChart.data.datasets[1].borderColor = color;
                    }
                    myChart.update();
                }
            });
        });
        
        // Initialize confidence level selection
        document.querySelectorAll('.confidence-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                confidenceLevel = parseFloat(this.getAttribute('data-level'));
                
                document.querySelectorAll('.confidence-btn').forEach(b => {
                    b.classList.remove('selected');
                });
                this.classList.add('selected');
                
                // Update labels
                const ciPercent = Math.round(confidenceLevel * 100);
                document.getElementById('result-ci-label').textContent = `${ciPercent}% CI Lower (1st category)`;
                document.getElementById('result-ci-upper-label').textContent = `${ciPercent}% CI Upper (1st category)`;
                
                // Update status message
                document.querySelector('.status-message').textContent = 
                    confidenceLevel === 0.99 ? "Higher confidence requires larger sample sizes." :
                    "Click 'Let's Plot' to update graph";
                
                // Recalculate and update display
                updateGroup1();
                updateGroup2();
            });
        });
        
        // Auto-calculate category 2 counts
        function updateGroup1() {
            const n = parseInt(document.getElementById('group1-n').value) || 0;
            const s1 = parseInt(document.getElementById('group1-success1').value) || 0;
            const s2 = n - s1;
            
            document.getElementById('group1-success2').textContent = s2;
            calculatePercentages();
        }
        
        function updateGroup2() {
            const n = parseInt(document.getElementById('group2-n').value) || 0;
            const s1 = parseInt(document.getElementById('group2-success1').value) || 0;
            const s2 = n - s1;
            
            document.getElementById('group2-success2').textContent = s2;
            calculatePercentages();
        }
        
        function calculatePercentages() {
            const g1n = parseInt(document.getElementById('group1-n').value) || 0;
            const g1s1 = parseInt(document.getElementById('group1-success1').value) || 0;
            const g1s2 = g1n - g1s1;
            const g2n = parseInt(document.getElementById('group2-n').value) || 0;
            const g2s1 = parseInt(document.getElementById('group2-success1').value) || 0;
            const g2s2 = g2n - g2s1;
            
            // Calculate proportions
            const g1p1 = g1n > 0 ? (g1s1 / g1n * 100).toFixed(2) + '%' : '0%';
            const g1p2 = g1n > 0 ? (g1s2 / g1n * 100).toFixed(2) + '%' : '0%';
            const g2p1 = g2n > 0 ? (g2s1 / g2n * 100).toFixed(2) + '%' : '0%';
            const g2p2 = g2n > 0 ? (g2s2 / g2n * 100).toFixed(2) + '%' : '0%';
            
            // Update displayed percentages
            document.getElementById('result-g1-prop1').textContent = g1p1;
            document.getElementById('result-g1-prop2').textContent = g1p2;
            document.getElementById('result-g2-prop1').textContent = g2p1;
            document.getElementById('result-g2-prop2').textContent = g2p2;
            
            // Calculate and update confidence intervals
            updateConfidenceIntervals();
        }
        
        function updateConfidenceIntervals() {
            const z = confidenceLevel === 0.99 ? 2.576 : confidenceLevel === 0.95 ? 1.96 : 1.645;
            
            const g1n = parseInt(document.getElementById('group1-n').value) || 0;
            const g1s1 = parseInt(document.getElementById('group1-success1').value) || 0;
            const g2n = parseInt(document.getElementById('group2-n').value) || 0;
            const g2s1 = parseInt(document.getElementById('group2-success1').value) || 0;
            
            const ci1 = calculateWilsonCI(g1s1, g1n, z);
            const ci2 = calculateWilsonCI(g2s1, g2n, z);
            
            // Update displayed confidence intervals
            document.getElementById('group1-ci').textContent = 
                g1n > 0 ? (ci1.lower * 100).toFixed(1) + '% - ' + (ci1.upper * 100).toFixed(1) + '%' : 'N/A';
            document.getElementById('group2-ci').textContent = 
                g2n > 0 ? (ci2.lower * 100).toFixed(1) + '% - ' + (ci2.upper * 100).toFixed(1) + '%' : 'N/A';
            
            document.getElementById('result-g1-ci-lower1').textContent = 
                g1n > 0 ? (ci1.lower * 100).toFixed(1) + '%' : 'N/A';
            document.getElementById('result-g1-ci-upper1').textContent = 
                g1n > 0 ? (ci1.upper * 100).toFixed(1) + '%' : 'N/A';
            document.getElementById('result-g2-ci-lower1').textContent = 
                g2n > 0 ? (ci2.lower * 100).toFixed(1) + '%' : 'N/A';
            document.getElementById('result-g2-ci-upper1').textContent = 
                g2n > 0 ? (ci2.upper * 100).toFixed(1) + '%' : 'N/A';
        }
        
        function calculateWilsonCI(successes, trials, z) {
            if (trials === 0) return { lower: 0, upper: 0 };
            
            const p = successes / trials;
            const denominator = 2 * (trials + z * z);
            const numerator = 2 * trials * p + z * z;
            const adjustment = z * Math.sqrt(4 * trials * p * (1 - p) + z * z);
            
            return {
                lower: Math.max(0, (numerator - adjustment) / denominator),
                upper: Math.min(1, (numerator + adjustment) / denominator)
            };
        }
        
        function plotData() {
            const g1n = parseInt(document.getElementById('group1-n').value) || 0;
            const g1s1 = parseInt(document.getElementById('group1-success1').value) || 0;
            const g2n = parseInt(document.getElementById('group2-n').value) || 0;
            const g2s1 = parseInt(document.getElementById('group2-success1').value) || 0;
            
            if (g1n === 0 && g2n === 0) {
                alert('Please enter data for at least one group');
                return;
            }
            
            // Calculate proportions
            const p1 = g1n > 0 ? g1s1 / g1n : 0;
            const p2 = g2n > 0 ? g2s1 / g2n : 0;
            
            // Calculate confidence intervals
            const z = confidenceLevel === 0.99 ? 2.576 : confidenceLevel === 0.95 ? 1.96 : 1.645;
            const ci1 = calculateWilsonCI(g1s1, g1n, z);
            const ci2 = calculateWilsonCI(g2s1, g2n, z);
            
            // Create or update chart
            createChart(p1, p2, ci1, ci2, g1n, g2n);
        }
        
        function createChart(p1, p2, ci1, ci2, n1, n2) {
            const ctx = document.getElementById('comparisonChart').getContext('2d');
            
            if (myChart) {
                // Update existing chart data 
                myChart.data.datasets[0].data = [{ x: 1, y: p1 }];
                myChart.data.datasets[1].data = [{ x: 2, y: p2 }];
                myChart.data.datasets[2].data = [
                    { x: 1, y: p1, yLow: ci1.lower, yHigh: ci1.upper },
                    { x: 2, y: p2, yLow: ci2.lower, yHigh: ci2.upper }
                ];
                myChart.update();
                return;
            }
            
            myChart = new Chart(ctx, {
                type: 'scatter',
                data: {
                    datasets: [
    {
        label: 'Group 1 (1st category)',
        data: [{ x: 1, y: p1 }],
        backgroundColor: groupColors[1],
        borderColor: groupColors[1],
        pointRadius: 5,
        pointHoverRadius: 7,
        borderWidth: 0 // Remove border
    },
    {
        label: 'Group 2 (1st category)',
        data: [{ x: 2, y: p2 }],
        backgroundColor: groupColors[2],
        borderColor: groupColors[2],
        pointRadius: 5,
        pointHoverRadius: 7,
        borderWidth: 0 // Remove border
    },
    {
        label: 'Confidence Interval',
        data: [
            { x: 1, y: p1, yLow: ci1.lower, yHigh: ci1.upper },
            { x: 2, y: p2, yLow: ci2.lower, yHigh: ci2.upper }
        ],
        type: 'line',
        backgroundColor: 'rgba(0, 0, 0, 0)',
        borderColor: 'rgba(0, 0, 0, 0)',
        borderWidth: 0,
        pointRadius: 0,
        fill: false
    }
]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            type: 'linear',
                            position: 'bottom',
                            min: 0.5,
                            max: 2.5,
                            ticks: {
                                callback: function(value) {
                                    return value === 1 ? 'Group 1' : value === 2 ? 'Group 2' : '';
                                }
                            }
                        },
                        y: {
                            min: 0,
                            max: 1,
                            ticks: {
                                callback: function(value) {
                                    return (value * 100).toFixed(0) + '%';
                                },
                                stepSize: 0.2
                            },
                            title: {
                                display: true,
                                text: 'Success Rate (1st category)'
                            }
                        }
                    },
                    plugins: {
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    if (context.datasetIndex === 0 || context.datasetIndex === 1) {
                                        const group = context.datasetIndex === 0 ? 1 : 2;
                                        const n = group === 1 ? n1 : n2;
                                        const k = Math.round(context.raw.y * n);
                                        const binomProb = jstat.binomial.pdf(k, n, context.raw.y);
                                        return [
                                            `Success rate: ${(context.raw.y * 100).toFixed(2)}%`,
                                            `Binomial P(X=${k}): ${binomProb.toFixed(6)}`
                                        ];
                                    }
                                    return '';
                                }
                            }
                        },
                        legend: {
                            onClick: null // Disable legend item clicks
                        }
                    }
                }
            });
            
            // Add custom rendering for confidence intervals
            Chart.register({
                id: 'confidenceInterval',
                afterDatasetsDraw(chart, args, options) {
                    const {ctx, data, chartArea: {top, bottom, left, right, width, height}, scales: {x, y}} = chart;
                    
                    ctx.save();
                    
                    // Draw confidence intervals
                    data.datasets.forEach((dataset, datasetIndex) => {
                        if (datasetIndex === 2) {
                            dataset.data.forEach((point, index) => {
                                const xPos = x.getPixelForValue(point.x);
                                const yLow = y.getPixelForValue(point.yLow);
                                const yHigh = y.getPixelForValue(point.yHigh);
                                const yMid = y.getPixelForValue(point.y);
                                
                                // Draw vertical line
                                ctx.beginPath();
                                ctx.moveTo(xPos, yLow);
                                ctx.lineTo(xPos, yHigh);
                                ctx.strokeStyle = index === 0 ? groupColors[1] : groupColors[2];
                                ctx.lineWidth = 2;
                                ctx.stroke();
                                
                                // Draw horizontal lines
                                const capLength = 15 * 0.75;
                                ctx.beginPath();
                                ctx.moveTo(xPos - capLength, yLow);
                                ctx.lineTo(xPos + capLength, yLow);
                                ctx.moveTo(xPos - capLength, yHigh);
                                ctx.lineTo(xPos + capLength, yHigh);
                                ctx.strokeStyle = index === 0 ? groupColors[1] : groupColors[2];
                                ctx.lineWidth = 2;
                                ctx.stroke();
                                
                                // Draw center point that expands on hover
                                const pointRadius = chart.getDatasetMeta(0).data[0].hover ? 7 : 5;
                                ctx.beginPath();
                                ctx.arc(xPos, yMid, pointRadius, 0, Math.PI * 2);
                                ctx.fillStyle = index === 0 ? groupColors[1] : groupColors[2];
                                ctx.fill();
                            });
                        }
                    });
                    
                    ctx.restore();
                }
            });
        }
        
        function clearData() {
            document.getElementById('group1-n').value = '';
            document.getElementById('group1-success1').value = '';
            document.getElementById('group2-n').value = '';
            document.getElementById('group2-success1').value = '';
            
            updateGroup1();
            updateGroup2();
        }
        
        function clearGraph() {
            if (myChart) {
                // Clear the data points and confidence intervals but keep the chart structure
                myChart.data.datasets[0].data = [];
                myChart.data.datasets[1].data = [];
                myChart.data.datasets[2].data = [];
                myChart.update();
            }
        }
        
        // Initialize with default values
        updateGroup1();
        updateGroup2();
        plotData();
    </script>
</body>
</html>
