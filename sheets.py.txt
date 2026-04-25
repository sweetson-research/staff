import streamlit as st

import gspread

from google.oauth2.service_account import Credentials



SCOPE = ["https://www.googleapis.com/auth/spreadsheets"]



@st.cache_resource

def connect_sheet():

    creds = Credentials.from_service_account_info(

        st.secrets["gcp_service_account"],

        scopes=SCOPE

    )

    client = gspread.authorize(creds)

    sheet = client.open_by_key(st.secrets["gcp_service_account"]["sheet_id"])

    return sheet.sheet1



def get_all():

    return connect_sheet().get_all_records()



def add(name, role, email, salary):

    connect_sheet().append_row([name, role, email, salary])



def update(row_index, name, role, email, salary):

    sheet = connect_sheet()

    sheet.update(f"A{row_index}😀{row_index}", [[name, role, email, salary]])



def delete(row_index):

    connect_sheet().delete_rows(row_index)