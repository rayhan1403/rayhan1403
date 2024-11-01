from flask import Flask, request, render_template
import requests

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        contacts = request.form['contacts'].split(',')
        message = request.form['message']
        media = request.files['media']

        # Process each contact
        for contact in contacts:
            send_message(contact.strip(), message, media)

        return 'Messages sent!'

    return render_template('index.html')

def send_message(contact, message, media):
    # WhatsApp API endpoint (replace with actual endpoint)
    url = 'https://api.whatsapp.com/send'
    headers = {'Authorization': 'Bearer YOUR_API_TOKEN'}

    data = {
        'to': contact,
        'text': message,
        'media': media.read()  # Read media file
    }

    response = requests.post(url, headers=headers, json=data)
    print(response.json())  # Log response

if __name__ == '__main__':
    app.run(debug=True)
