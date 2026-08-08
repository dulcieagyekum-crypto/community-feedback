import os
from flask import Flask, request, render_template_string
from google.cloud import firestore

app = Flask(__name__)
db = firestore.Client(database="community-feedback")

HTML = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Community Event Feedback</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 700px;
            margin: 40px auto;
            padding: 20px;
            line-height: 1.5;
            background: #f5f7fa;
        }
        .card {
            background: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.08);
        }
        h1 { text-align: center; }
        label {
            display: block;
            margin-top: 18px;
            font-weight: bold;
        }
        input, select, textarea {
            width: 100%;
            padding: 10px;
            margin-top: 7px;
            box-sizing: border-box;
            border: 1px solid #ccc;
            border-radius: 6px;
        }
        textarea { height: 120px; resize: vertical; }
        button {
            margin-top: 24px;
            padding: 12px 24px;
            border: 0;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
        }
        .success {
            text-align: center;
            padding: 30px 10px;
        }
    </style>
</head>
<body>
<div class="card">
    <h1>Community Event Feedback</h1>
    <p>Thank you for attending our event. Please tell us about your experience.</p>

    <form method="POST">
        <label for="name">Your name</label>
        <input id="name" type="text" name="name" required>

        <label for="event">Which event did you attend?</label>
        <select id="event" name="event" required>
            <option value="">Select an event</option>
            <option value="Summer Fair">Summer Fair</option>
            <option value="Sports Day">Sports Day</option>
            <option value="Community Workshop">Community Workshop</option>
        </select>

        <label for="rating">How would you rate the event?</label>
        <select id="rating" name="rating" required>
            <option value="">Select a rating</option>
            <option value="1">1 - Poor</option>
            <option value="2">2 - Fair</option>
            <option value="3">3 - Good</option>
            <option value="4">4 - Very Good</option>
            <option value="5">5 - Excellent</option>
        </select>

        <label for="comment">Your feedback</label>
        <textarea id="comment" name="comment" required></textarea>

        <button type="submit">Submit Feedback</button>
    </form>
</div>
</body>
</html>
"""

SUCCESS_HTML = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Feedback Submitted</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 700px;
            margin: 80px auto;
            padding: 20px;
            text-align: center;
        }
        a { display: inline-block; margin-top: 20px; }
    </style>
</head>
<body>
    <h1>Thank you, {{ name }}!</h1>
    <p>Your feedback for <strong>{{ event }}</strong> has been submitted successfully.</p>
    <a href="/">Submit another response</a>
</body>
</html>
"""

@app.route("/", methods=["GET", "POST"])
def feedback():
    if request.method == "POST":
        name = request.form["name"].strip()
        event = request.form["event"]
        rating = int(request.form["rating"])
        comment = request.form["comment"].strip()

        feedback_data = {
            "name": name,
            "event": event,
            "rating": rating,
            "comment": comment,
            "created_at": firestore.SERVER_TIMESTAMP,
        }

        db.collection("feedback").add(feedback_data)

        return render_template_string(
            SUCCESS_HTML,
            name=name,
            event=event
        )

    return render_template_string(HTML)

if __name__ == "__main__":
    app.run(
        host="0.0.0.0",
        port=int(os.environ.get("PORT", 8080))
