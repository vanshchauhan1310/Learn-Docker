# Deploying a Streamlit App Using Docker 🐳📊

In this guide, we will walk through the process of deploying a **Streamlit** application using **Docker**. Docker is a powerful platform that allows you to containerize applications, making them portable and independent of the underlying system. Streamlit, on the other hand, is an open-source framework that enables you to create and share beautiful, interactive web apps for machine learning and data science.

By the end of this tutorial, you will have a fully functional Streamlit app running inside a Docker container, accessible via your web browser. 🌐

---

## Prerequisites 📋

Before we begin, ensure you have the following installed on your system:

1. **Docker**: A platform for developing, shipping, and running applications in containers.
2. **Python**: A programming language used to write the Streamlit application.
3. **Streamlit**: A framework for building interactive web apps with Python.

---

## Documentation 📚

For more information, refer to the official documentation:

- [Docker Documentation](https://www.docker.com/products/docker-desktop/)
- [Streamlit Documentation](https://docs.streamlit.io/)
- [Docker Desktop Documentation](https://www.docker.com/products/docker-desktop/)

---

## Installation 🛠️

### Step 1: Install Streamlit
To install Streamlit, run the following command in your terminal:

```bash
pip install streamlit

```

## Step 2: Install Docker Desktop

Download and install Docker Desktop from the official Docker website. Once installed, start Docker Desktop and ensure it is running.

## Verify Installations ✅

Before proceeding, let's verify that Docker and Python are installed correctly.

Check Docker Version

Run the following command in your terminal:

```bash
docker --version
```

You should see an output similar to:

```bash
Docker version 20.10.17, build 100c701
```
Check Python Version

Run the following command in your terminal:

```bash
python --version
```

You should see an output similar to:

```bash
Python 3.9.7
```

## Create the Streamlit Application 🐍

Let’s create a simple Streamlit application. Save the following code in a file named streamlit_app.py:

```bash
from collections import namedtuple
import altair as alt
import math
import pandas as pd
import streamlit as st

"""
# Welcome to Streamlit! 🎉

Edit `/streamlit_app.py` to customize this app to your heart's desire :heart:

If you have any questions, checkout our [documentation](https://docs.streamlit.io) and [community
forums](https://discuss.streamlit.io).

In the meantime, below is an example of what you can do with just a few lines of code:
"""

with st.echo(code_location='below'):
   total_points = st.slider("Number of points in spiral", 1, 5000, 2000)
   num_turns = st.slider("Number of turns in spiral", 1, 100, 9)

   Point = namedtuple('Point', 'x y')
   data = []

   points_per_turn = total_points / num_turns

   for curr_point_num in range(total_points):
      curr_turn, i = divmod(curr_point_num, points_per_turn)
      angle = (curr_turn + 1) * 2 * math.pi * i / points_per_turn
      radius = curr_point_num / total_points
      x = radius * math.cos(angle)
      y = radius * math.sin(angle)
      data.append(Point(x, y))

   st.altair_chart(alt.Chart(pd.DataFrame(data), height=500, width=500)
      .mark_circle(color='#0068c9', opacity=0.5)
      .encode(x='x:Q', y='y:Q'))
```

This application creates an interactive spiral visualization using Streamlit and Altair. 🌀


## Create the Dockerfile 📄

A Dockerfile is a script that contains instructions for Docker to build an image. Create a file named Dockerfile (no file extension) in the same directory as streamlit_app.py and add the following content:

```bash
# Use an official Python runtime as the base image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    software-properties-common \
    git \
    && rm -rf /var/lib/apt/lists/*

# Clone the Streamlit example repository
RUN git clone https://github.com/streamlit/streamlit-example.git .

# Install Python dependencies
RUN pip3 install -r requirements.txt

# Expose port 8501 for Streamlit
EXPOSE 8501

# Health check to ensure the app is running
HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

# Set the entrypoint command to run the Streamlit app
ENTRYPOINT ["streamlit", "run", "streamlit_app.py", "--server.port=8501", "--server.address=0.0.0.0"]

```

This Dockerfile does the following:

1. Uses the official Python 3.9 slim image as the base.
2. Sets the working directory inside the container to /app.
3. Installs system dependencies required for Streamlit.
4. Clones the Streamlit example repository.
5. Installs Python dependencies from requirements.txt.
6. Exposes port 8501 for the Streamlit app.
7. Adds a health check to ensure the app is running.
8. Sets the entrypoint command to run the Streamlit app.

## Build the Docker Image 🏗️

Now that we have our Dockerfile and streamlit_app.py, let's build the Docker image.

Open a terminal in the directory containing the Dockerfile and streamlit_app.py.
Run the following command to build the Docker image:

```bash
docker build -t streamlit_app .
```

* The -t flag tags the image with the name streamlit_app.
* The . at the end specifies the build context (current directory).

## Verify the Docker Image 🖼️

After building the image, let's verify that it was created successfully.

Run the following command:

```bash
docker images
```

You should see an output similar to:

```bash
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
streamlit_app   latest    abc123def456   10 seconds ago   1.02GB
```

## Run the Docker Container 🚀

Finally, let's run the Docker container to execute our Streamlit app.

Run the following command:

```bash
docker run -p 8501:8501 streamlit_app
```

* The -p flag maps port 8501 on your local machine to port 8501 in the container.
* The streamlit_app is the name of the Docker image.
Once the container is running, open your web browser and navigate to:

```bash
http://localhost:8501
```
You should see your Streamlit app running! 🎉


## Conclusion 🎉

Congratulations! 🎉 You’ve successfully deployed a Streamlit application using Docker. This setup ensures that your app is portable, scalable, and independent of the underlying system. Docker and Streamlit together provide a powerful combination for building and deploying data-driven applications.

Keep exploring and building more complex applications with Docker and Streamlit! 🚀🐳

![alt text](Docker_Experiments/2.Streamlit_Docker/Dockers_Streamlit/Dockers_Streamlit/Dockers/Images/imge1.png)

Happy coding! 💻✨