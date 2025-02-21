# muelldizzy7.github.io
Instantly calculate an athlete's truck stick (momentum) using their Fly 10yd or 10m time and body weight!
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Truck Stick Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
        }
        .input-group {
            margin: 10px 0;
        }
        label {
            display: inline-block;
            width: 180px;
        }
        button {
            margin-top: 10px;
            padding: 5px 15px;
        }
        #result {
            margin-top: 15px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h2>Truck Stick Calculator</h2>
    
    <div class="input-group">
        <label for="timeYards">10 Yard Fly Time (seconds):</label>
        <input type="number" id="timeYards" step="0.01" min="0">
    </div>
    
    <div class="input-group">
        <label for="timeMeters">10 Meter Fly Time (seconds):</label>
        <input type="number" id="timeMeters" step="0.01" min="0">
    </div>
    
    <div class="input-group">
        <label for="weight">Weight (lbs):</label>
        <input type="number" id="weight" step="0.1" min="0" required>
    </div>
    
    <button onclick="calculateTruckStick()">Calculate Truck Stick</button>
    
    <div id="result"></div>

    <script>
        function calculateTruckStick() {
            // Get input values
            const timeYards = parseFloat(document.getElementById('timeYards').value);
            const timeMeters = parseFloat(document.getElementById('timeMeters').value);
            const weightLbs = parseFloat(document.getElementById('weight').value);
            
            // Validate inputs
            if (isNaN(weightLbs) || weightLbs <= 0) {
                document.getElementById('result').innerHTML = 
                    'Please enter a valid positive weight.';
                return;
            }
            
            if ((isNaN(timeYards) && isNaN(timeMeters)) || 
                (timeYards <= 0 && timeMeters <= 0) ||
                (!isNaN(timeYards) && !isNaN(timeMeters))) {
                document.getElementById('result').innerHTML = 
                    'Please enter a valid positive time for either 10 Yard Fly OR 10 Meter Fly (not both).';
                return;
            }

            // Calculate speed based on whichever time is provided
            let speedMs, distanceType;
            if (!isNaN(timeYards) && timeYards > 0) {
                const distanceYardsMeters = 10 * 0.9144;  // 10 yards to meters
                speedMs = distanceYardsMeters / timeYards;
                distanceType = "10 Yard Fly";
            } else {
                speedMs = 10 / timeMeters;  // 10 meters directly
                distanceType = "10 Meter Fly";
            }
            
            // Convert weight to kg
            const massKg = weightLbs * 0.453592;
            
            // Calculate momentum (p = m × v) in kg·m/s
            const momentum = massKg * speedMs;
            
            // Display result
            document.getElementById('result').innerHTML = 
                `Truck Stick Value: ${momentum.toFixed(2)} kg·m/s<br>` +
                `Speed: ${speedMs.toFixed(2)} m/s (based on ${distanceType})`;
        }
    </script>
</body>
</html>
