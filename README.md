# Task 7: Build a Weather App (Console)
# Requirements:
# pip install requests numpy pandas matplotlib

import requests
import json
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Enter your OpenWeatherMap API Key
API_KEY = "YOUR_API_KEY_HERE"

# Ask user for city name
city = input("Enter city name: ")

# API URL
url = f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={API_KEY}&units=metric"

try:
    # Fetch weather data
    response = requests.get(url)
    data = response.json()

    # Check if city is found
    if data["cod"] != 200:
        print("Error:", data["message"])

    else:
        # Extract weather details
        temperature = data["main"]["temp"]
        humidity = data["main"]["humidity"]
        condition = data["weather"][0]["description"]

        # Display weather information
        print("\n------ Weather Report ------")
        print(f"City       : {city}")
        print(f"Temperature: {temperature} °C")
        print(f"Condition  : {condition}")
        print(f"Humidity   : {humidity}%")

        # Create DataFrame using pandas
        weather_data = pd.DataFrame({
            "Weather Info": ["Temperature", "Humidity"],
            "Values": [temperature, humidity]
        })

        print("\nData Table:")
        print(weather_data)

        # Using numpy array
        values = np.array([temperature, humidity])

        # Plot graph using matplotlib
        plt.figure(figsize=(5, 5))
        plt.bar(["Temperature", "Humidity"], values)
        plt.title(f"Weather Data for {city}")
        plt.ylabel("Values")
        plt.show()

except requests.exceptions.RequestException:
    print("Network error! Please check your internet connection.")

except Exception as e:
    print("Something went wrong:", e)# weather-app