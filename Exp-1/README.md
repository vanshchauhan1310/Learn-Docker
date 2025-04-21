# 🚀 Docker Experiment 1: Streamlit Spiral Visualization App with Docker

Welcome to the *Streamlit Spiral Visualization App! This project demonstrates a simple and interactive Python application built with **Streamlit* to visualize a spiral. You can customize the spiral’s characteristics using adjustable sliders and view the changes in real-time. The app is Dockerized for easy deployment and consistency across environments.

## 🌟 *Features*
- *Interactive Controls*: Use sliders to adjust the number of points and turns in the spiral.
- *Dynamic Visualization*: Watch the spiral change dynamically in response to slider adjustments.
- *Dockerized Application*: The app is packaged within a Docker container, ensuring portability and easy deployment.

## 🚀 *Technologies Used*
- *Python 3*: The core programming language for this app.
- *Streamlit*: A framework for building interactive and beautiful web applications.
- *Altair*: A declarative statistical visualization library for Python used for rendering the spiral.
- *Docker*: Containerizes the app for consistent behavior across different environments.
- *Pandas*: Used for data manipulation and handling data frames.

## ⚙ *Prerequisites*
Before running the application, ensure you have the following installed on your local machine:

- *Docker*: To build and run the app inside a container.
- *Git*: To clone the repository.

If you don't have *Docker* installed, follow the instructions in the official [Docker Installation Guide](https://docs.docker.com/get-docker/).


## 🛠 **Getting Started**
Follow these steps to set up and run the application either locally or inside a Docker container:

### Step 1: **Clone the Repository**
Clone the repository to your local machine by running:

```bash
git clone https://github.com/vanshchauhan1310/Learn-docker.git

cd Learn-Docker
```

### Step 2: **Build the Docker Image**
Build the Docker image from the project directory:

```bash
docker build -t streamlit .
```

This will use the `Dockerfile` to build the image named `streamlit`.


### Step 4: **Run the Docker Container**
Start the app inside a Docker container by running:

```bash
docker run -p 8501:8501 streamlit
```

This command will map port `8501` inside the container to port `8501` on your local machine.

### Step 5: **Access the Streamlit App**
After running the container, open your browser and go to:

```
http://localhost:8501
```

The Streamlit app should now be visible, allowing you to interactively adjust the spiral’s number of points and turns.

___

## Streamlit Python Code for Spiral Visualization

```python
import streamlit as st
import pandas as pd
import numpy as np
import altair as alt
from collections import namedtuple

# Define a namedtuple to store points in the spiral
Point = namedtuple('Point', ['x', 'y'])

# Function to generate the spiral data
def generate_spiral(num_points, num_turns):
    data = []
    for i in range(num_points):
        angle = 2 * np.pi * num_turns * (i / num_points)
        radius = i / num_points
        x = radius * np.cos(angle)
        y = radius * np.sin(angle)
        data.append(Point(x, y))
    return data

# Streamlit App Layout
st.title("Interactive Spiral Visualization")

# Sliders for user input
num_points = st.slider('Number of Points in Spiral', min_value=50, max_value=1000, value=500)
num_turns = st.slider('Number of Turns in Spiral', min_value=1, max_value=10, value=5)

# Generate the spiral data
spiral_data = generate_spiral(num_points, num_turns)

# Convert the spiral data to a DataFrame for visualization
spiral_df = pd.DataFrame(spiral_data, columns=['x', 'y'])

# Create the Altair chart to visualize the spiral
chart = alt.Chart(spiral_df).mark_circle(size=3).encode(
    x='x',
    y='y'
).properties(
    width=600,
    height=600
)

# Display the chart
st.altair_chart(chart, use_container_width=True)
```

## Dockerfile

```dockerfile
# Use official Python image from the Docker Hub
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the requirements.txt into the container
COPY requirements.txt .

# Install the dependencies
RUN pip install -r requirements.txt

# Copy the current directory contents into the container
COPY . .

# Expose the port for Streamlit
EXPOSE 8501

# Run the Streamlit app
CMD ["streamlit", "run", "app.py"]
```

## requirements.txt

```txt
streamlit
altair
pandas
```


### Happy Coding! 🎉


