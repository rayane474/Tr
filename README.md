# Tr
Airson.com
import streamlit as st
import streamlit_themes
import pandas as pd
import sqlite3
import ia_streamlit

# Create a dataframe for products
products = pd.DataFrame({
    "Product Name": ["iPhone 12", "Samsung Galaxy S21", "Google Pixel 5"],
    "Price": [999, 849, 699],
    "Rating": [4.5, 4.4, 4.3]
})

# Connect to the database
conn = sqlite3.connect('comments.db')
c = conn.cursor()

# Create comments table if it doesn't exist
c.execute('''CREATE TABLE IF NOT EXISTS comments
             (product_id INTEGER, comment TEXT)''')
conn.commit()

# Set page configuration
st.set_page_config(
    page_title="Airson E-Commerce Store",
    page_icon="🛒",
    layout="wide",
    initial_sidebar_state="expanded",
    theme="royal"
)

# Dark mode theme
st.set_theme(streamlit_themes.ThemeType.DARKLY)

# Language selector
language = st.sidebar.selectbox("Language", ["English", "French", "Spanish", "Arabic", "German", "Japanese", "Chinese"])
st.sidebar.write("Selected language: ", language)

# Search bar
def search_products(query):
    filtered_products = products[products["Product Name"].str.contains(query, case=False)]
    return filtered_products

query = st.sidebar.text_input("Search for products:")
if query:
    filtered_products = search_products(query)
    st.write(filtered_products)

# Product details
if filtered_products.empty:
    st.write("No products found.")
else:
    selected_product = st.sidebar.selectbox("Select a product:", filtered_products["Product Name"])
    product_details = filtered_products[filtered_products["Product Name"] == selected_product]
    st.write("Product Details:")
    st.write(product_details)

    # Add to cart
    if st.button("Add to Cart"):
        st.write("Product added to cart!")

# Leave a comment
comment = st.text_area("Leave a comment:")
if st.button("Submit Comment"):
    if comment:
        c.execute("INSERT INTO comments (product_id, comment) VALUES (?, ?)", (selected_product, comment))
        conn.commit()
        st.write("Comment added successfully!")

# Display comments
st.write("Product Comments:")

c.execute("SELECT * FROM comments WHERE product_id=?", (selected_product,))
comments = c.fetchall()

if comments:
    for comment in comments:
        st.write(comment[1])
else:
    st.write("No comments yet.")

# Payment method selection
payment_method = st.sidebar.selectbox("Payment Method", ["Credit Card", "PayPal", "Apple Pay", "Alipay"])
if payment_method == "Credit Card":
    st.write("Credit Card payment selected.")
    # TODO: Add credit card payment integration
elif payment_method == "PayPal":
    st.write("PayPal payment selected.")
    # TODO: Add PayPal payment integration
elif payment_method == "Apple Pay":
    st.write("Apple Pay payment selected.")
    # TODO: Add Apple Pay payment integration
elif payment_method == "Alipay":
    st.write("Alipay payment selected.")
    # TODO: Add Alipay payment integration

# Marketing
st.write("Marketing:")

# Social media marketing
if st.checkbox("Social media marketing"):
    st.write("Social media marketing is enabled.")
    # TODO: Add social media marketing integration
else:
    st.write("Social media marketing is disabled.")

# Email marketing
if st.checkbox("Email marketing"):
    st.write("Email marketing is enabled.")
    # TODO: Add email marketing integration
else:
    st.write("Email marketing is disabled
