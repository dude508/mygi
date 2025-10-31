import os
from flask import Flask, request, jsonify
from google import genai

# Non Chatbot ou a
BOT_NAME = "ALIX-IA" 

# Li API Key nan Anviwonman Varyab la
GEMINI_API_KEY = os.environ.get("GEMINI_API_KEY")

if not GEMINI_API_KEY:
    raise ValueError("GEMINI_API_KEY pa tabli nan anviwonman an.")

app = Flask(__name__)
client = genai.Client(api_key=GEMINI_API_KEY)

# Konfigirasyon modèl la (ou ka chanje modèl la tou)
model = client.models.get('gemini-2.5-flash')

# Kreye yon sesyon chat
chat = client.chats.create(model=model.name)

@app.route('/')
def home():
    return f"Byenveni sou paj akeyi {BOT_NAME}."

@app.route('/chat', methods=['POST'])
def chat_endpoint():
    try:
        data = request.get_json()
        user_prompt = data.get('prompt')

        if not user_prompt:
            return jsonify({"repons": "Tanpri bay yon kesyon (prompt)."}), 400

        # Voye mesaj la bay Gemini epi tann repons lan
        response = chat.send_message(user_prompt)

        return jsonify({
            "repons_ki_soti_nan": BOT_NAME,
            "kesyon_ou": user_prompt,
            "repons": response.text
        })

    except Exception as e:
        # Aji kòmsi se erè Gemini a sèlman ki parèt men se yon erè sèvè a
        return jsonify({"repons": f"Nou gen yon erè sèvè: {str(e)}"}), 500

if __name__ == '__main__':
    # Sa se pou devlopman lokal SÈLMAN
    app.run(debug=True)
