<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Three-Wheel Twister Spinner</title>
    <style>
        /* Basic layout and mobile-friendly setup */
        body {
            margin: 0;
            padding: 0;
            font-family: sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background: #f9f9f9;
        }

        h1 {
            margin-top: 1rem;
            margin-bottom: 1rem;
            font-size: 1.5rem;
        }

        .wheels {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            justify-content: center;
            align-items: center;
        }

        /* Common wheel styling */
        .wheel-container {
            position: relative;
            width: 160px;
            height: 160px;
        }

        .wheel {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            transition: transform 3s cubic-bezier(0.33, 1, 0.68, 1);
        }

        .pointer {
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 0;
            height: 0;
            border-left: 4px solid transparent;
            border-right: 4px solid transparent;
            border-bottom: 8px solid #333;
        }

        /* Wheel #1: Left / Right */
        #wheel1 {
            background: conic-gradient(
                    /* Top half  */
                    #aaf 0deg 180deg,
                    /* Bottom half */
                    #faa 180deg 360deg);
        }

        /* Wheel #2: Hand / Foot */
        #wheel2 {
            background: conic-gradient(
                    /* Top half  */
                    #aff 0deg 180deg,
                    /* Bottom half */
                    #faf 180deg 360deg);
        }

        /* Wheel #3: 4 colors */
        #wheel3 {
            background: conic-gradient(red 0deg 90deg,
                    blue 90deg 180deg,
                    green 180deg 270deg,
                    yellow 270deg 360deg);
        }

        /* Text overlays for the first two wheels */
        .slice-text {
            position: absolute;
            width: 100%;
            text-align: center;
            font-weight: bold;
            pointer-events: none;
            /* allow clicks through text */
        }

        /* For 2-slice wheels: top half text at ~25% down, bottom half text at ~75% down */
        .top-text {
            top: 25%;
            left: 50%;
            transform: translateX(-50%);
        }

        .bottom-text {
            top: 75%;
            left: 50%;
            transform: translateX(-50%);
        }

        /* Spin button */
        .spin-button {
            background-color: #007bff;
            color: white;
            padding: 0.75rem 1.5rem;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            margin-top: 1rem;
            font-size: 1rem;
        }

        .spin-button:hover {
            background-color: #005dc1;
        }

        /* Result text */
        #result {
            font-size: 1.25rem;
            font-weight: 600;
            margin-top: 1rem;
            text-align: center;
        }

        /* Make wheels smaller on narrower screens for better mobile experience */
        @media (max-width: 600px) {
            .wheel-container {
                width: 120px;
                height: 120px;
            }
        }
    </style>
</head>

<body>

    <h1>3-Wheel Twister Spinner</h1>

    <!-- Wheels Container -->
    <div class="wheels">
        <!-- Wheel #1: Left / Right -->
        <div class="wheel-container">
            <div id="wheel1" class="wheel"></div>
            <div class="pointer"></div>
            <!-- Text overlays (top / bottom) -->
            <div class="slice-text top-text">Left</div>
            <div class="slice-text bottom-text">Right</div>
        </div>

        <!-- Wheel #2: Hand / Foot -->
        <div class="wheel-container">
            <div id="wheel2" class="wheel"></div>
            <div class="pointer"></div>
            <!-- Text overlays -->
            <div class="slice-text top-text">Hand</div>
            <div class="slice-text bottom-text">Foot</div>
        </div>

        <!-- Wheel #3: Colors (Red, Blue, Green, Yellow) -->
        <div class="wheel-container">
            <div id="wheel3" class="wheel"></div>
            <div class="pointer"></div>
        </div>
    </div>

    <!-- Spin Button -->
    <button class="spin-button" onclick="spinWheels()">Spin</button>

    <!-- Result Display -->
    <div id="result"></div>

    <script>
        // Data sets for each wheel
        const leftRight = ["Left", "Right"];
        const handFoot = ["Hand", "Foot"];
        const colors = ["Red", "Blue", "Green", "Yellow"];

        // Track cumulative rotations for each wheel
        let rotation1 = 0;
        let rotation2 = 0;
        let rotation3 = 0;

        function spinWheels() {
            // For wheels #1 and #2 (2 slices each): each slice is 180 degrees
            // For wheel #3 (4 slices): each slice is 90 degrees

            // Each wheel does 5 to 8 full rotations (5*360 to 8*360)
            // plus a random offset to land on a slice.
            const randomSpin1 = 360 * (Math.floor(Math.random() * 4) + 5);
            const randomSpin2 = 360 * (Math.floor(Math.random() * 4) + 5);
            const randomSpin3 = 360 * (Math.floor(Math.random() * 4) + 5);

            // Offsets: 0 to 359
            const offset1 = Math.floor(Math.random() * 360);
            const offset2 = Math.floor(Math.random() * 360);
            const offset3 = Math.floor(Math.random() * 360);

            // Update cumulative rotation
            rotation1 += randomSpin1 + offset1;
            rotation2 += randomSpin2 + offset2;
            rotation3 += randomSpin3 + offset3;

            // Apply CSS transforms
            document.getElementById("wheel1").style.transform =
                `rotate(${rotation1}deg)`;
            document.getElementById("wheel2").style.transform =
                `rotate(${rotation2}deg)`;
            document.getElementById("wheel3").style.transform =
                `rotate(${rotation3}deg)`;

            // After 3s, identify the final slices
            setTimeout(() => {
                const finalAngle1 = rotation1 % 360;
                const finalAngle2 = rotation2 % 360;
                const finalAngle3 = rotation3 % 360;

                // Wheel #1 index (2 slices): 180 degrees each
                const index1 = Math.floor(finalAngle1 / 180);
                // Wheel #2 index (2 slices): 180 degrees each
                const index2 = Math.floor(finalAngle2 / 180);
                // Wheel #3 index (4 slices): 90 degrees each
                const index3 = Math.floor(finalAngle3 / 90);

                // Determine results
                const part1 = leftRight[index1];    // Left or Right
                const part2 = handFoot[index2];     // Hand or Foot
                const color = colors[index3];       // Red, Blue, Green, Yellow

                // Display final combination
                document.getElementById("result").textContent =
                    `${part1} ${part2} on ${color}`;
            }, 3000);
        }
    </script>
</body>

</html>