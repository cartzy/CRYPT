<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title> Cryptography</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4; /* Light grey background */
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            flex-direction: column; /* Stack elements vertically */
        }
        .container {
            max-width: 600px;
            margin: auto;
            padding: 20px;
            border: 2px solid teal;
            border-radius: 10px;
            box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.2);
            background-color: white;
            z-index: 1;
        }
        h2 {
            text-align: center;
            color: teal;
        }
        input, button {
            margin-top: 10px;
            padding: 10px;
            font-size: 16px;
            border-radius: 5px;
            border: 1px solid teal;
        }
        button {
            background-color: teal;
            color: white;
        }
        button:hover {
            background-color: #006666;
        }
        #message {
            font-weight: bold;
            margin-top: 10px;
            color: teal;
        }
        .error {
            font-size: 50px;
            color: red;
            font-weight: bold;
            margin-top: 20px;
            text-align: center;
            width: 100%;
            z-index: 2; /* Make sure error message appears above the game container */
        }
        .morse-code {
            font-family: monospace;
            white-space: pre;
            font-size: 18px;
            color: teal; 
        }
        .admin-box {
            margin-top: 20px;
            display: none;
            background-color: #f4f4f4;
            padding: 10px;
            border: 1px solid teal;
            text-align: center;
            width: 80%;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
        }
        #revealedMessage {
            font-size: 18px;
            color: teal;
        }
    </style>
</head>
<body>

    
    <!-- Admin Decryption Box -->
    <div id="adminBox" class="admin-box">
        <h3>Admin Decryption</h3>
        <input type="text" id="adminKey" placeholder="Enter the admin key (password)">
        <button onclick="revealMessage()">Reveal Hidden Message</button>
        <p id="revealedMessage"></p>
    </div>

    <div class="container" id="gameContainer">
        <h2>MORSE CODE CRYPTOGRAPHY</h2>
        <p> <strong>Encrypted Message:</strong> <span id="morseMessage"></span></p>
        <p>  Clue: "A Biblical Phrase about Peace and Tribulation"</p>

        <input type="text" id="userGuess" placeholder="Enter your guess">
        <button onclick="checkGuess()">Submit</button>
        <button onclick="generateNewEncryption()">Retry</button>

        <p id="message"></p>
    </div>

    <script>
        const originalMessage = "John ~ I have said these things to you, that in me you may have peace. In the world you will have tribulation. But take heart; I have overcome the world.";
        let morseCode = "";
        let attempts = 0;
        const maxAttempts = 3;
        const decryptionKey = "bossing?"; // Admin key for decryption

        // Morse code mapping
        const morseCodeMapping = {
            "a": ".-", "b": "-...", "c": "-.-.", "d": "-..", "e": ".", "f": "..-.", "g": "--.", "h": "....",
            "i": "..", "j": ".---", "k": "-.-", "l": ".-..", "m": "--", "n": "-.", "o": "---", "p": ".--.",
            "q": "--.-", "r": ".-.", "s": "...", "t": "-", "u": "..-", "v": "...-", "w": ".--", "x": "-..-",
            "y": "-.--", "z": "--..", "1": ".----", "2": "..---", "3": "...--", "4": "....-", "5": ".....",
            "6": "-....", "7": "--...", "8": "---..", "9": "----.", "0": "-----", " ": " / ", "~": " ~ "
        };

        // Function to convert text to Morse Code
        function textToMorse(text) {
            return text.toLowerCase().split('').map(char => morseCodeMapping[char] || char).join(' '); 
        }

        // Generate new encrypted message
        function generateNewEncryption() {
            morseCode = textToMorse(originalMessage);
            document.getElementById("morseMessage").innerText = morseCode;
            document.getElementById("message").innerText = "";
            document.getElementById("userGuess").value = "";
            document.getElementById("errorMessage").style.display = "none";
            document.getElementById("gameContainer").style.display = "block";
            document.getElementById("adminBox").style.display = "none"; // Hide admin box initially
            attempts = 0;
        }

        // Check the user's guess
        function checkGuess() {
            const userGuess = document.getElementById("userGuess").value.trim();
            if (userGuess === originalMessage) {
                document.getElementById("message").innerHTML = "🎉 Correct! You decrypted the message!";
            } else {
                attempts++;
                if (attempts >= maxAttempts) {
                    document.getElementById("gameContainer").style.display = "none";
                    document.getElementById("errorMessage").style.display = "block";
                    document.getElementById("adminBox").style.display = "block"; // Show admin box when attempts are over
                } else {
                    document.getElementById("message").innerHTML = `❌ Incorrect. Attempts left: ${maxAttempts - attempts}`;
                }
            }
        }

        // Admin reveals the hidden message with the correct key
        function revealMessage() {
            const adminKey = document.getElementById("adminKey").value.trim();
            if (adminKey === decryptionKey) {
                document.getElementById("revealedMessage").innerText = originalMessage;
            } else {
                document.getElementById("revealedMessage").innerText = "❌ Incorrect key!";
            }
        }

        // Start the game
        generateNewEncryption();
    </script>
</body>
</html>
