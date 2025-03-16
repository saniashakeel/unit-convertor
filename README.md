import streamlit as st

# Custom CSS for Dark Mode Styling
st.markdown(
    """
    <style>
    body {
        background: #121212;  /* Dark Background */
        color: #ffffff;  /* White Text */
    }
    .stApp {
        background: linear-gradient(135deg, #1e1e1e, #424242); /* Dark Grey Gradient */
        padding: 30px;
        border-radius: 15px;
        box-shadow: 0px 10px 30px rgba(255, 255, 255, 0.1);
    }
    h1 {
        text-align: center;
        font-size: 36px;
        color: #ff9800;  /* Orange Title */
        font-weight: bold;
    }
    .stButton > button {
        background: linear-gradient(45deg, #ff8c00, #ff5722); /* Neon Orange */
        color: white;
        font-size: 18px;
        padding: 12px 24px;
        border-radius: 10px;
        border: none;
        transition: 0.3s;
        cursor: pointer;
        box-shadow: 0px 5px 15px rgba(255, 87, 34, 0.4);
    }
    .stButton > button:hover {
        transform: scale(1.05);
        background: linear-gradient(45deg, #ff5722, #ff8c00);
    }
    .result-box {
        font-size: 22px;
        font-weight: bold;
        text-align: center;
        background: rgba(255, 255, 255, 0.2);
        padding: 20px;
        border-radius: 10px;
        margin-top: 15px;
        box-shadow: 0px 5px 15px rgba(255, 255, 255, 0.2);
        color: white;
    }
        .footer {
        text-align: center;
        margin-top: 50px;
        font-size: 18px;  
        font-weight: bold;
        color: #ff9800;
    }
    </style>
    """,
    unsafe_allow_html=True
)

# App Title
st.markdown("<h1>Unit Converter 🔄</h1>", unsafe_allow_html=True)
st.write("Easily convert between different units of **Length, Weight, and Temperature.**")

# Sidebar Menu
conversion_type = st.sidebar.selectbox("Choose Conversion Type", ["Length", "Weight", "Temperature"])
value = st.number_input("Enter Value", value=0.0, min_value=0.0, step=0.1)

col1, col2 = st.columns(2)

if conversion_type == "Length":
    with col1:
        from_unit = st.selectbox("From", ["Meters", "Kilometers", "Centimeters", "Millimeters", "Miles", "Yards", "Inches", "Feet"])
    with col2:
        to_unit = st.selectbox("To", ["Meters", "Kilometers", "Centimeters", "Millimeters", "Miles", "Yards", "Inches", "Feet"])

elif conversion_type == "Weight":
    with col1:
        from_unit = st.selectbox("From", ["Kilogram", "Grams", "Milligrams", "Pounds", "Ounces"])
    with col2:
        to_unit = st.selectbox("To", ["Kilogram", "Grams", "Milligrams", "Pounds", "Ounces"])

elif conversion_type == "Temperature":
    with col1:
        from_unit = st.selectbox("From", ["Celsius", "Fahrenheit", "Kelvin"])
    with col2:
        to_unit = st.selectbox("To", ["Celsius", "Fahrenheit", "Kelvin"])

# Conversion Functions
def length_converter(value, from_unit, to_unit):
    length_units = {
        "Meters": 1, "Kilometers": 0.001, "Centimeters": 100, "Millimeters": 1000,
        "Miles": 0.000621371, "Yards": 1.09361, "Feet": 3.28084, "Inches": 39.3701
    }
    return (value / length_units[from_unit]) * length_units[to_unit]

def weight_converter(value, from_unit, to_unit):
    weight_units = {
        "Kilogram": 1, "Grams": 1000, "Milligrams": 1000000,
        "Pounds": 2.20462, "Ounces": 35.274
    }
    return (value / weight_units[from_unit]) * weight_units[to_unit]

def temp_converter(value, from_unit, to_unit):
    if from_unit == "Celsius":
        return (value * 9/5 + 32) if to_unit == "Fahrenheit" else value + 273.15 if to_unit == "Kelvin" else value
    elif from_unit == "Fahrenheit":
        return (value - 32) * 5/9 if to_unit == "Celsius" else (value - 32) * 5/9 + 273.15 if to_unit == "Kelvin" else value
    elif from_unit == "Kelvin":
        return value - 273.15 if to_unit == "Celsius" else (value - 273.15) * 9/5 + 32 if to_unit == "Fahrenheit" else value
    return value

# Convert Button
if st.button("🔄 Convert"):
    if conversion_type == "Length":
        result = length_converter(value, from_unit, to_unit)
    elif conversion_type == "Weight":
        result = weight_converter(value, from_unit, to_unit)
    elif conversion_type == "Temperature":
        result = temp_converter(value, from_unit, to_unit)

    st.markdown(f"<div class='result-box'>{value} {from_unit} = {result:.4f} {to_unit}</div>", unsafe_allow_html=True)

# Footer
st.markdown("<div class='footer'>🚀 Created by Sania Shakeel</div>", unsafe_allow_html=True)
