from flask import Flask, request, jsonify
from transformers import pipeline

app = Flask(__name__)

# Load the conversational model
chatbot = pipeline('conversational', model='microsoft/DialoGPT-medium')

@app.route('/chat', methods=['POST'])
def chat():
    user_input = request.json.get('message')
    if user_input:
        bot_response = chatbot(user_input)
        response_text = bot_response[0]['generated_text']
        return jsonify({'response': response_text})
    return jsonify({'response': 'No input provided.'}), 400

if __name__ == '__main__':
    app.run(debug=True)
