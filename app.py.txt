import streamlit as st

import pandas as pd

from sheets import get_all, add, update, delete

from utils import inject_css



st.set_page_config(page_title="HR Admin", layout="wide")

inject_css()



st.title("🏢 HR Admin Panel")



menu = st.sidebar.selectbox("Navigation", [

    "Dashboard", "Add Staff", "Manage Staff"

])



data = get_all()

df = pd.DataFrame(data)



# 📊 Dashboard

if menu == "Dashboard":

    st.subheader("Overview")



    col1, col2, col3 = st.columns(3)



    with col1:

        st.metric("Total Staff", len(df))



    with col2:

        if not df.empty:

            st.metric("Avg Salary", int(df["salary"].mean()))

        else:

            st.metric("Avg Salary", 0)



    with col3:

        if not df.empty:

            st.metric("Roles", df["role"].nunique())

        else:

            st.metric("Roles", 0)



    st.divider()

    st.dataframe(df, use_container_width=True)



# ➕ Add Staff

elif menu == "Add Staff":

    st.subheader("Add Employee")



    with st.form("add_form"):

        name = st.text_input("Name")

        role = st.text_input("Role")

        email = st.text_input("Email")

        salary = st.number_input("Salary", min_value=0)



        submit = st.form_submit_button("Add")



        if submit:

            if not name or not email:

                st.warning("Name & Email required")

            else:

                add(name, role, email, salary)

                st.success("✅ Employee added")

                st.rerun()



# 📋 Manage Staff (Edit/Delete)

elif menu == "Manage Staff":

    st.subheader("Manage Employees")



    search = st.text_input("🔍 Search by name")



    if search:

        df = df[df["name"].str.contains(search, case=False)]



    for i, row in df.iterrows():

        with st.expander(f"{row['name']} - {row['role']}"):

            

            col1, col2 = st.columns(2)



            with col1:

                new_name = st.text_input("Name", row["name"], key=f"name{i}")

                new_role = st.text_input("Role", row["role"], key=f"role{i}")



            with col2:

                new_email = st.text_input("Email", row["email"], key=f"email{i}")

                new_salary = st.number_input("Salary", value=int(row["salary"]), key=f"salary{i}")



            c1, c2 = st.columns(2)



            with c1:

                if st.button("💾 Update", key=f"update{i}"):

                    update(i+2, new_name, new_role, new_email, new_salary)

                    st.success("Updated")

                    st.rerun()



            with c2:

                if st.button("🗑 Delete", key=f"delete{i}"):

                    delete(i+2)

                    st.warning("Deleted")

                    st.rerun()