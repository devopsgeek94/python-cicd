Introduction
The power of Python is now available to web based projects via frameworks such as Flask and Streamlit. These projects allows the developer to bring the data wrangling and analytics capabilities of Python to the web. When it comes to the deployment and sharing of these projects, development and testing can be hindered by problems with setting up dependencies, requirements, network ports, and execution environments. To solve this, Docker was introduced. Docker is an open source product that offers hardware virtualization at the Operating System (OS) level. This guide will demonstrate how to use Docker to package and ship a Python-based Streamlit web app.

The guide assumes you have at least intermediate knowledge in Python and have a base level of understanding of Streamlit and Docker.

Sample App
Consider the scenario where you would like to build a growth calculator that determines the worth of an investment given the growth rate, initial investment, and time in years.

This app would then be shared as part of your developer portfolio. It is paramount that interested parties will be able to download and run your project with ease. The web being the most common interface, you decide the project will be web based. Since it involves data analysis, it will be in Python. To give your recipients an easy time in setting up and running your app, you decide to package it in docker.

Copy the code below into a Python file and name it app.py

import streamlit as st

st.title("Hello Streamlit")
st.header("Calculate % Growth")
initial = st.number_input("Initial investment in USD")
yr = st.number_input("Growth Period in years")
growth = st.number_input("Growth Rate in %")
terminal_value = 0
current_val = initial
for year in range(int(yr)):
   current_val += growth * current_val
   terminal_value = current_val

# perform cashflow projections for the next 5 years
st.write(f'Terminal value of {initial} after {yr} years at a growth rate of {growth} is {terminal_value}')
python
This project requires Python libraries to be installed for it to run. The libraries will be recorded in a requirements.txt file. Copy the contents below into your requirements.txt file.

streamlit
.env
For ease of handling during the Docker building, place both the Python and requirements files in a folder and name it src/.

Docker Setup
The dockerfile required for this project mainly has to achieve the following logical steps:

Create base image
Copy source code
Install requirements and dependencies
Expose required port
Run the Streamlit app within the Docker environment
Copy the Docker commands below in a file and name it Dockerfile

FROM python:3.8

ENV MICRO_SERVICE=/home/app/webapp
# set work directory
RUN mkdir -p $MICRO_SERVICE
# where your code lives
WORKDIR $MICRO_SERVICE

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# install dependencies
RUN pip install --upgrade pip
# copy project
COPY src/ $MICRO_SERVICE
RUN pip install -r requirements.txt
EXPOSE 8501
CMD streamlit run app.py
dockerfile
Running the Project
With both files set up, you are ready to build and run your image. To build your image, run the command

docker build -t myimage .
bash
After the docker image is built, it is time to run your Docker image.

docker run -p 8501:8501 myimage
bash
The app is now running at the localhost address provided by the Docker interface. Stremlit pageStremlit page

Conclusion
Packaging and shipping a Python web app using Docker are vital skills for developers holding Python developer and Devops related positions. To further build on these skills, do further research on how to run multi-container apps on Docker using docker-compose.
