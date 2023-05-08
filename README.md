# Task 1  

To create a Flask web application with the features outlined in Task 1, you'll need to follow the steps below:

# 1 :- Install Flask
First, you'll need to install Flask. Open your terminal and run the following command:
Copy code
pip install flask
Create a Flask app
Create a new Python file called app.py and add the following code to create a Flask app:
# python

from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)
Create a simple HTML form
Create a new folder called templates and add a new HTML file called index.html. Add the following code to create a simple form that accepts user input:
# html
<!DOCTYPE html>
<html>
  <head>
    <title>ChatGPT Demo</title>
  </head>
  <body>
    <h1>ChatGPT Demo</h1>
    <form method="POST" action="/result">
      <input type="text" name="message" placeholder="Enter your message">
      <button type="submit">Submit</button>
    </form>
  </body>
</html>
# Create a results page
# Add a new HTML file called result.html to the templates folder. Add the following code to display the message entered by the user:

# html
<!DOCTYPE html>
<html>
  <head>
    <title>ChatGPT Demo</title>
  </head>
  <body>
    <h1>ChatGPT Demo</h1>
    <p>Your message: {{ message }}</p>
  </body>
</html>

# Integrate the ChatGPT API
# To integrate the ChatGPT API, you'll need to install the requests library. Open your terminal and run the following command:

pip install requests
Add the following code to the app.py file to handle API requests:

# python
import requests

CHATGPT_API_URL = 'https://api.openai.com/v1/engines/davinci-codex/completions'

def generate_response(prompt):
    headers = {
        'Content-Type': 'application/json',
        'Authorization': f'Bearer {CHATGPT_API_KEY}'
    }
    data = {
        'prompt': prompt,
        'max_tokens': 50,
        'n': 1,
        'stop': ['\n']
    }
    response = requests.post(CHATGPT_API_URL, headers=headers, json=data)
    if response.status_code == 200:
        return response.json()['choices'][0]['text'].strip()
    else:
        return 'Error: Failed to generate response'

@app.route('/result', methods=['POST'])
def result():
    message = request.form['message']
    prompt = f"User: {message}\nChatbot:"
    response = generate_response(prompt)
    return render_template('result.html', message=response)
# Implement basic web development principles
# 6 :- Add the following code to the index.html file to add some basic styling:
# html
 <!DOCTYPE html>
<html>
  <head>
    <title>ChatGPT Demo</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 0;
      }
      h1 {
        text-align: center;
        margin-top: 50px;
      }
      form {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 
        }
        
# how the Flask app might be structured and implemented to satisfy the requirements of Task 2:

# python

from flask import Flask, render_template, request, jsonify
import openai
import os

# Set up the OpenAI API key
openai.api_key = os.environ.get('OPENAI_API_KEY')

# Initialize the Flask application
app = Flask(__name__)

# Define the route for the homepage
@app.route('/', methods=['GET', 'POST'])
def home():
    if request.method == 'GET':
        return render_template('index.html')
    elif request.method == 'POST':
        prompt = request.form.get('prompt')
        if not prompt:
            error_message = "Please provide a prompt."
            return render_template('index.html', error_message=error_message)
        try:
            response = openai.Completion.create(
                engine='text-davinci-002',
                prompt=prompt,
                max_tokens=1024,
                n=1,
                stop=None,
                temperature=0.5,
            )
            generated_text = response.choices[0].text
            return render_template('results.html', generated_text=generated_text)
        except Exception as e:
            error_message = f"An error occurred: {str(e)}"
            return render_template('index.html', error_message=error_message)

# Define the route for the ChatGPT API integration
@app.route('/api/generate', methods=['POST'])
def generate_text():
    data = request.json
    if not data:
        return jsonify({'error': 'No JSON data provided.'}), 400
    prompt = data.get('prompt')
    if not prompt:
        return jsonify({'error': 'No prompt provided.'}), 400
    try:
        response = openai.Completion.create(
            engine='text-davinci-002',
            prompt=prompt,
            max_tokens=1024,
            n=1,
            stop=None,
            temperature=0.5,
        )
        generated_text = response.choices[0].text
        return jsonify({'generated_text': generated_text}), 200
    except Exception as e:
        return jsonify({'error': str(e)}), 500

# Run the Flask application
if __name__ == '__main__':
    app.run(debug=True)
In this example, the Flask app defines two routes: one for the homepage and one for the ChatGPT API integration. The home() function handles both GET and POST requests to the homepage. GET requests simply render the index.html template, which contains an HTML form for submitting prompts. POST requests process the submitted form data and generate text using the OpenAI API. If no prompt is provided, an error message is displayed on the homepage. If an error occurs during text generation, the error message is displayed on the homepage.

The generate_text() function handles POST requests to the /api/generate endpoint. It expects JSON data with a prompt field. If no JSON data is provided or if no prompt is provided, an error response is returned. Otherwise, text is generated using the OpenAI API and returned as JSON data.

To test the app, you could run it locally using python app.py and use a tool like curl or a web client like Postman to send requests to the endpoints. For example, to test the /api/generate endpoint, you could send a POST request with a JSON body like {"prompt": "Hello, world!"} and check the response to ensure that generated text is returned.


# Task 3:

1a. Purpose and Functionality:
The web application is a text generation tool that uses the GPT-3.5 language model to generate natural language text based on user prompts. The application provides a web-based interface for users to input prompts and receive generated text in response. The ChatGPT API is integrated into the application to generate text based on user prompts.

1b. Instructions for Setup, Installation, and Running the Application:

Clone the repository from GitHub.
Install the required Python packages using pip: pip install -r requirements.txt
Set the environment variable OPENAI_API_KEY to your OpenAI API key.
Run the application using Flask: flask run
Access the application by visiting http://localhost:5000 in your web browser.
1c. Examples of API Calls and Responses:
Example Request to /api/generate:

# bash

POST /api/generate HTTP/1.1
Host: localhost:5000
Content-Type: application/json

{
    "prompt": "Once upon a time",
    "max_tokens": 50,
    "temperature": 0.8,
    "n": 1,
    "stop": "."
}
Example Response from /api/generate:

# less

HTTP/1.1 200 OK
Content-Type: application/json

{
    "text": "Once upon a time, in a far-off land, there lived a powerful wizard named Merlin. He was known throughout the land for his magical powers, and many






