# Build a docker image for streamlit app

To build a Docker image for your Python Streamlit application, you can follow these steps. I'll break down the process and explain each part of the Dockerfile, `requirements.txt`, and the general steps involved.

### 1. **Directory Structure**

Assuming you have the following structure for your project:

```
/streamlit-app
  ├── Dockerfile
  ├── hello.py
  ├── requirements.txt
```

Where:
- `hello.py` contains the Streamlit code
- `requirements.txt` lists the dependencies for the app.

### 2. **Streamlit app**

````python
from collections import namedtuple
import altair as alt
import math
import pandas as pd
import streamlit as st


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
````

### 3. **requirements.txt**

Ensure that your `requirements.txt` file contains the necessary Python packages for your application. Based on your example, your `requirements.txt` might look like this:

```txt
streamlit==1.12.0
altair==4.2.0
pandas==1.4.2
matplotlib==3.5.1
```

Make sure that the versions are compatible with your Python version. You can install any other dependencies that your app might need in this file.

### 4. **Dockerfile**

This Dockerfile sets up the environment for your Streamlit app. Here's the updated version, with some minor adjustments:

```dockerfile
# Use the official Python image from Docker Hub
FROM python:3.9-slim

# Set the working directory inside the container
WORKDIR /app

# Copy the Python app and requirements file into the container
COPY hello.py /app
COPY requirements.txt /app

# Upgrade pip and install the dependencies from requirements.txt
RUN python -m pip install --upgrade pip
RUN pip install -r requirements.txt

# Expose the Streamlit default port (8501)
EXPOSE 8501

# Set the entrypoint for running the Streamlit app
ENTRYPOINT ["streamlit", "run", "hello.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

### Explanation of the Dockerfile:

1. **`FROM python:3.9-slim`**:
   - We're using the official slim Python 3.9 image, which is a lightweight version of Python and perfect for production environments.

2. **`WORKDIR /app`**:
   - Sets the working directory inside the container to `/app`. This is where the Streamlit app will be stored and run.

3. **`COPY hello.py /app`**:
   - Copies the `hello.py` file (which contains your Streamlit code) into the container's `/app` directory.

4. **`COPY requirements.txt /app`**:
   - Copies the `requirements.txt` file (which lists your app's dependencies) into the container.

5. **`RUN python -m pip install --upgrade pip`**:
   - Ensures that pip is up to date before installing dependencies.

6. **`RUN pip install -r requirements.txt`**:
   - Installs all the required dependencies from the `requirements.txt` file.

7. **`EXPOSE 8501`**:
   - Exposes port `8501`, which is the default port that Streamlit runs on.

8. **`ENTRYPOINT ["streamlit", "run", "hello.py", "--server.port=8501", "--server.address=0.0.0.0"]`**:
   - Specifies the command that will be run when the container starts. This runs the Streamlit app and ensures it binds to all network interfaces (`0.0.0.0`), which is necessary for accessing it externally.

### 5. **Build the Docker Image**

Once your `Dockerfile` and `requirements.txt` are set up, you can build the Docker image.

In your terminal, navigate to the project directory where the `Dockerfile` is located and run:

```bash
docker build -t streamlit-app .
```

This will create a Docker image named `streamlit-app`.

### 6. **Run the Docker Container**

After building the image, you can run it in a Docker container with the following command:

```bash
docker run -p 8501:8501 streamlit-app
```

This will map port `8501` on your local machine to port `8501` in the container, which is the port used by Streamlit.

### 7. **Access the Application**

Once the container is running, you can open your browser and visit:

```
http://localhost:8501
```

You should see your Streamlit app running!

### Conclusion

By following these steps, you've created a Docker image for your Python Streamlit application. Docker allows you to package your application, including all dependencies, in a way that makes it easy to deploy and run consistently across different environments.