import nltk
import random
from nltk.stem import WordNetLemmatizer

# Ensure nltk data
for resource in ["punkt", "punkt_tab", "wordnet"]:
    try:
        nltk.data.find(resource)
    except LookupError:
        nltk.download(resource)

lemmatizer = WordNetLemmatizer()

# Professional intents
intents = {
    "greeting": ["hello", "hi", "hey", "good morning", "good evening"],
    "internship_info": ["internship", "tasks", "project", "overview"],
    "task_guidance": ["task", "explain", "guide", "help with", "report", "chatbot"],
    "submission": ["deadline", "submit", "upload", "github"],
    "tech_help": ["error", "install", "library", "import", "pip"],
    "bye": ["bye", "thanks", "see you", "goodbye"]
}

responses = {
    "greeting": [
        "Hello 👋, how can I assist you today?",
        "Hi, nice to see you! What would you like help with?"
    ],
    "internship_info": [
        "This internship includes four tasks: API integration, PDF report generation, building a chatbot, and machine learning.",
        "Your goal is to complete all four tasks and upload them to GitHub for evaluation."
    ],
    "task_guidance": [
        "Task 1 is about fetching data from an API and visualizing it with Matplotlib or Seaborn.",
        "The PDF report task requires reading data from a file and creating a professional report using ReportLab.",
        "For the chatbot task, you’ll use NLP (like NLTK or spaCy) to handle user queries."
    ],
    "submission": [
        "The deadline will be mentioned in your internship guidelines. Make sure to submit before that.",
        "You should upload your code and reports to a GitHub repository.",
        "Follow the submission instructions given by your coordinator."
    ],
    "tech_help": [
        "For errors, please share the exact error message so I can help you debug.",
        "You can install matplotlib using: pip install matplotlib",
        "For generating PDFs, use the reportlab library."
    ],
    "bye": [
        "Goodbye 👋, best of luck with your tasks!",
        "Take care, and happy coding 🚀"
    ]
}

def detect_intent(user_tokens):
    scores = {intent: 0 for intent in intents}
    for intent, keywords in intents.items():
        for token in user_tokens:
            if token in keywords:
                scores[intent] += 1
    best_intent = max(scores, key=scores.get)
    return best_intent if scores[best_intent] > 0 else None

def chatbot():
    print("🤖 Internship Assistant Chatbot: Type 'quit' to exit")
    while True:
        user_input = input("> ").lower()
        if user_input in ["quit", "exit"]:
            print("Goodbye 👋, best of luck with your internship!")
            break
        user_tokens = [lemmatizer.lemmatize(w) for w in nltk.word_tokenize(user_input)]
        intent = detect_intent(user_tokens)
        if intent:
            print(random.choice(responses[intent]))
        else:
            print("Hmm 🤔 I didn't quite get that. Could you try asking differently?")

if __name__ == "__main__":
    chatbot()
