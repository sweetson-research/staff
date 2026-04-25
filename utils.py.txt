import streamlit as st



def inject_css():

    st.markdown("""

        <style>

        .metric-card {

            padding: 15px;

            border-radius: 10px;

            background-color: #f5f7fa;

            text-align: center;

        }

        .stButton>button {

            border-radius: 8px;

            padding: 0.4rem 1rem;

        }

        </style>

    """, unsafe_allow_html=True)