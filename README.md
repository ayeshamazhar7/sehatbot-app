import streamlit as st
import openai

# Get your API key from Streamlit Secrets
openai.api_key = st.secrets["OPENAI_API_KEY"]

st.set_page_config(page_title="SehatBot", layout="centered")

st.markdown("<h1 style='text-align: center; color: #FF69B4;'>🌸 SehatBot 🌸</h1>", unsafe_allow_html=True)
st.markdown("<h3 style='text-align: center; color: #FFD700;'>Your Personalized PCOS Wellness Guide</h3>", unsafe_allow_html=True)

st.write("### 👋 Welcome to SehatBot!")
st.write("This AI-powered assistant helps you manage PCOS with personalized advice in English & Urdu.")

name = st.text_input("Enter your name")
age = st.number_input("Your age", min_value=12, max_value=50)
cycle = st.selectbox("Your cycle status", ["Regular", "Irregular", "No period"])
goals = st.multiselect("Your goals", ["Weight loss", "Fertility", "Hormonal balance", "Glowing skin", "Regular periods"])

symptoms = st.text_area("Any specific symptoms or concerns? (e.g., facial hair, mood swings, cravings, acne)")

if st.button("Get My Plan"):
    prompt = f"""
    My name is {name}, I am {age} years old. My period cycle is {cycle}.
    My PCOS goals are: {', '.join(goals)}.
    My symptoms include: {symptoms}.
    
    Please create a friendly, Urdu + English response with advice for:
    - Daily routine
    - Food & herbs
    - Exercise
    - Any motivation tips
    """

    with st.spinner("🧠 SehatBot is thinking..."):
        try:
            response = openai.ChatCompletion.create(
                model="gpt-3.5-turbo",
                messages=[{"role": "user", "content": prompt}]
            )
            bot_reply = response["choices"][0]["message"]["content"]
            st.markdown("### 📋 Your Personalized PCOS Plan")
            st.write(bot_reply)
        except Exception as e:
            st.error(f"Something went wrong: {e}")

st.markdown("---")
st.write("💡 Want premium features like reminders, PDF plan & inositol guidance?")
st.write("💸 Starting from Rs. 1000 via EasyPaisa (coming soon!)")
