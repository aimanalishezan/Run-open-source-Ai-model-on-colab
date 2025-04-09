
# 🤖 MAN_Ai: Dolphin-LLaMA3.1 Chatbot on Google Colab

**MAN_Ai** is an interactive AI chatbot built using the Dolphin-LLaMA3.1 model from Cognitive Computations, served through **Ollama**, and deployed via a **Gradio** interface. This setup is fully integrated into a Google Colab environment for a seamless user experience.

---

## 🚀 Features

- Chat with the powerful **Dolphin-LLaMA3.1** model
- Simple Gradio UI with custom styling
- Google Colab integration for on-the-fly setup
- Secure authentication using Colab `userdata`
- Progressive streaming responses for better interactivity

---
Project WorkFLow

![ollama in colab](https://github.com/user-attachments/assets/f0658688-1eab-4d6c-bd86-689aaf5e793f)


## 📦 Installation & Setup (Google Colab)

> Paste the following code blocks into your Colab notebook and run them sequentially.

### 1. Install Required Packages

```bash
!pip install colab-xterm
%load_ext colabxterm
```

### 2. Install Ollama & Pull the Model

```bash
%%xterm
curl -fsSL https://ollama.com/install.sh | sh
ollama serve &
ollama pull CognitiveComputations/dolphin-llama3.1  # Run this twice if needed
```

### 3. Install Python Dependencies

```python
!pip install gradio ollama
```

---

## 🧠 How It Works

The chatbot function builds a message list using the system prompt and chat history, then streams responses from the LLaMA3.1 model served by Ollama. The responses are dynamically streamed to the frontend using Gradio's ChatInterface.

### Main Technologies:
- **Ollama**: For model serving
- **Gradio**: For UI
- **Google Colab**: For interactive deployment

---

## 🧪 Usage

Make sure your `.env` variables or `userdata` in Colab include the following:

- `id`: Username for auth
- `pas`: Password for auth
- `model`: Model name (e.g., `CognitiveComputations/dolphin-llama3.1`)
- `system`: System prompt for the chatbot

```python
from google.colab import userdata
user = userdata.get('id')
pas = userdata.get('pas')
MODEL = userdata.get('model')
system_message = userdata.get('system')
```

Define the chat function and interface:

```python
def chat(message, history):
    messages = [{"role": "system", "content": system_message}] + history + [{"role": "user", "content": message}]
    stream = ollama.chat(model=MODEL, messages=messages, stream=True)
    response = ""
    for chunk in stream:
        response += chunk["message"]["content"] if "message" in chunk else ""
        yield response
```

Customize the UI and launch:

```python
demo.launch(share=True, pwa=True, auth=(user, pas))
```

---

Project WorkFlow



## 🎨 UI Styling

The UI features a dark-themed header with Tailwind-inspired design for modern aesthetics. Customize using the `custom_css` string in the code for personal branding or layout changes.

---

## 🔒 Authentication

This app uses basic authentication via `gradio.auth` using Colab's `userdata` API. You can store and access secrets securely without hardcoding them into your script.

---

## 📎 Notes

- If errors do not appear in Colab, set `debug=True` in `demo.launch()`.
- Model download may need to be run twice depending on Ollama’s caching behavior.
- Always verify that Ollama server is running in the Colab terminal (`%xterm`).

---

## 🌐 Live Demo

After launching, Gradio will provide a public URL like:

```
Running on public URL: https://<random-id>.gradio.live
```

---

## 👨‍💻 Developer

**Ai_MAN**  
Feel free to fork or contribute!

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

```

---
