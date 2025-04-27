<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mood Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #f4f4f9;
        }

        .container {
            width: 80%;
            max-width: 600px;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            background: white;
            margin-top: 20px;
        }

        .container h1 {
            margin-bottom: 20px;
            font-size: 24px;
            color: #333;
        }

        label {
            display: block;
            margin: 10px 0 5px;
            font-weight: bold;
        }

        input, textarea, button {
            width: 100%;
            padding: 10px;
            margin: 5px 0 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        button {
            background-color: #007BFF;
            color: white;
            font-size: 16px;
            cursor: pointer;
            border: none;
        }

        button:hover {
            background-color: #0056b3;
        }

        .link {
            text-align: center;
            margin-top: 20px;
        }

        #response {
            margin-top: 20px;
            color: #333;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Mood Tracker</h1>
        <form id="moodForm">
            <label for="mood">How are you feeling today?</label>
            <input type="text" id="mood" name="mood" placeholder="e.g., Happy, Sad" required>
            <label for="notes">Notes (Optional):</label>
            <textarea id="notes" name="notes" rows="4" placeholder="Share your thoughts..."></textarea>
            <button type="submit">Submit</button>
        </form>

        <div id="response"></div>

        <div class="link">
            <a href="/mood_history">View Mood History</a>
        </div>
    </div>

    <script>
        document.getElementById('moodForm').addEventListener('submit', function(event) {
            event.preventDefault();

            const formData = new FormData(event.target);

            fetch('/add_mood', {
                method: 'POST',
                body: formData
            })
            .then(response => response.json())
            .then(data => {
                if (data.message) {
                    document.getElementById('response').innerHTML = `
                        <p>${data.message}</p>
                        <p><strong>Suggestion:</strong> ${data.suggestion}</p>
                    `;
                } else if (data.error) {
                    document.getElementById('response').textContent = data.error;
                }
            })
            .catch(error => {
                document.getElementById('response').textContent = 'An error occurred. Please try again later.';
            });
        });
    </script>
</body>
</html>
