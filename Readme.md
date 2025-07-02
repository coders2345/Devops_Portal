# DevOps CI/CD: Flask Portfolio App 🚀

A personal portfolio web application built with Python Flask and deployed using Docker, Jenkins CI/CD, and Render.

---

## 🔧 Technologies Used

- **Python Flask** – Web framework for app logic and routing
- **HTML/CSS** – Frontend for UI rendering
- **Docker** – Containerized app for portability
- **Jenkins** – Continuous Integration pipeline
- **DockerHub** – Docker image registry
- **GitHub** – Source code hosting
- **Render.com** – Cloud platform used for deployment

---

## 🌍 Live URL

👉 **[https://devops-portal.onrender.com](https://devops-portal.onrender.com)**

---

## 📁 Project Structure
Devops_Portal/
├── app/
│ └── routes.py, init.py
├── templates/
│ └── index.html, about.html, etc.
├── static/
│ └── style.css
├── run.py
├── Dockerfile
├── Jenkinsfile
├── render.yaml

yaml
Copy
Edit

---

## 🐳 Docker Commands (Manual)

```bash
# Build Docker Image
docker build -t flask-portfolio .

# Run Container
docker run -d -p 5000:5000 flask-portfolio


