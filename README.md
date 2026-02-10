import streamlit as st
import google.generativeai as genai

# Configure Gemini API
genai.configure(api_key="YOUR_GEMINI_API_KEY")

model = genai.GenerativeModel("gemini-pro")

st.set_page_config(page_title="Gemini Chatbot")
st.title("🤖 Gemini Chatbot")

# Store chat history
if "chat" not in st.session_state:
    st.session_state.chat = model.start_chat(history=[])

if "messages" not in st.session_state:
    st.session_state.messages = []

# Display previous messages
for msg in st.session_state.messages:
    with st.chat_message(msg["role"]):
        st.markdown(msg["content"])

# User input
prompt = st.chat_input("Ask something...")

if prompt:
    # Show user message
    st.session_state.messages.append({"role": "user", "content": prompt})
    with st.chat_message("user"):
        st.markdown(prompt)

    # Gemini response
    response = st.session_state.chat.send_message(prompt)

    st.session_state.messages.append(
        {"role": "assistant", "content": response.text}
    )
    with st.chat_message("assistant"):
        st.markdown(response.text)
