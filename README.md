from flask import Flask, request, jsonify
from langchain.chat_models import ChatOpenAI
from dotenv import load_dotenv
import os

load_dotenv()

app = Flask(__name__)
llm = ChatOpenAI(model="gpt-4", temperature=0, openai_api_key=os.getenv("OPENAI_API_KEY"))

@app.route("/")
def home():
    return "LangGraph GPT Agent is up!"

@app.route("/summarize", methods=["POST"])
def summarize():
    user_input = request.json.get("content", "")
    response = llm.predict(user_input)
    return jsonify({"summary": response})
