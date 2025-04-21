# 🐳 Dockerized Flask Web App

This is a simple Flask web application deployed using Docker. It's a beginner-level DevOps project to understand how containerization works using Docker.

## 📌 What It Does

- A basic Python Flask app with one route (`/`)
- Runs inside a Docker container
- Returns a simple "Hello" message

## 🛠️ Technologies Used

- Python 3.9
- Flask
- Docker

## 📂 Project Structure

docker-webapp/ │ ├── app.py ├── requirements.txt └── Dockerfile

## 🚀 How to Run This Project

### 1. Clone the Repo
```bash
git clone https://github.com/yourusername/docker-webapp.git
cd docker-webapp
```
2. Build the Docker Image
```bash
docker build -t flask-docker-app .
```
3. Run the Docker Container
``` bash
docker run -p 5000:5000 flask-docker-app
```
4. View in Browser
Open: http://localhost:5000
You should see: "Hello, this is a Dockerized Flask App! by Shams."

### 🐳 Docker Image
Available on Docker Hub:  
👉 https://hub.docker.com/r/kshamsuddin/docker-webapp

👤 Author
Khaja Shamsuddin Ahmed
