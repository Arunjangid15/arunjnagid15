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

